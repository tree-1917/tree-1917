### 🛠️ VProfile Setup Tutorial Using Vagrant

This tutorial provides a detailed breakdown of the VProfile setup using Vagrant. Each section includes enhanced steps, configurations, and additional context for each service.

---

## 📋 Table of Contents

1. [📦 Setup Vagrant and Common VM Configuration](#setup-vagrant-and-common-vm-configuration)
2. [📂 Setup MySQL](#setup-mysql)
3. [📡 Setup RabbitMQ](#setup-rabbitmq)
4. [🚀 Setup Application](#setup-application)
5. [🌐 Setup Nginx](#setup-nginx)
6. [⚡ Setup Memcache](#setup-memcache)
7. [🗂️ Full Vagrantfile](#full-vagrantfile)

---

### 1. 📦 Setup Vagrant and Common VM Configuration

The common configuration defines the base VM, networking, and shared settings.

```ruby
# Base configuration for all VMs
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64" # Use Ubuntu 18.04 base image
end
```

#### Explanation:

- **Base Image**: The `ubuntu/bionic64` box provides a lightweight Ubuntu system.
- **Vagrant Version**: The `Vagrant.configure("2")` ensures compatibility with Vagrant 2.0+.

---

### 2. 📂 Setup MySQL

The MySQL VM sets up the database server, creates a database, user, and applies security settings.

```ruby
# MySQL VM
config.vm.define "mysql" do |mysql|
  mysql.vm.hostname = "mysql-vm"
  mysql.vm.network "private_network", ip: "192.168.56.101" # Private network for internal communication
  mysql.vm.provider "virtualbox" do |vb|
    vb.memory = 1024 # Allocate 1GB of memory
    vb.cpus = 2 # Use 2 CPUs
  end
  mysql.vm.provision "shell", inline: <<-SHELL
    echo "🚀 Setting up MySQL VM..."
    sudo apt update
    sudo apt install -y mysql-server

    # Start MySQL service and enable at boot
    sudo systemctl enable --now mysql

    # Configure MySQL for remote access
    sudo sed -i "s/bind-address.*/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf
    sudo systemctl restart mysql

    # Secure installation
    sudo mysql -e "UPDATE mysql.user SET authentication_string=PASSWORD('root_password') WHERE User='root';"
    sudo mysql -e "DELETE FROM mysql.user WHERE User='';"
    sudo mysql -e "DROP DATABASE IF EXISTS test;"
    sudo mysql -e "FLUSH PRIVILEGES;"

    # Create database and user for VProfile
    sudo mysql -e "CREATE DATABASE vprofiledb;"
    sudo mysql -e "CREATE USER 'vprofileuser'@'%' IDENTIFIED BY 'your_password';"
    sudo mysql -e "GRANT ALL PRIVILEGES ON vprofiledb.* TO 'vprofileuser'@'%';"
    sudo mysql -e "FLUSH PRIVILEGES;"

    # Allow MySQL traffic in firewall
    sudo ufw allow 3306
  SHELL
end
```

#### Explanation:

- **Remote Access**: Updates `mysqld.cnf` to allow connections from any IP.
- **Firewall Rules**: Opens port `3306` for MySQL traffic.
- **Security**: Implements best practices like removing anonymous users and test databases.

---

### 3. 📡 Setup RabbitMQ

RabbitMQ facilitates message brokering in the VProfile app.

```ruby
# RabbitMQ VM
config.vm.define "rabbitmq" do |rabbitmq|
  rabbitmq.vm.hostname = "rabbitmq-vm"
  rabbitmq.vm.network "private_network", ip: "192.168.56.102"
  rabbitmq.vm.provider "virtualbox" do |vb|
    vb.memory = 512 # Allocate 512MB of memory
    vb.cpus = 1 # Use 1 CPU
  end
  rabbitmq.vm.provision "shell", inline: <<-SHELL
    echo "🚀 Setting up RabbitMQ VM..."
    sudo apt update
    sudo apt install -y rabbitmq-server

    # Enable RabbitMQ service at boot
    sudo systemctl enable --now rabbitmq-server

    # Add RabbitMQ user for VProfile
    sudo rabbitmqctl add_user vprofileuser your_password
    sudo rabbitmqctl set_permissions -p / vprofileuser ".*" ".*" ".*"

    # Allow RabbitMQ traffic in firewall
    sudo ufw allow 5672
    sudo ufw allow 15672 # For RabbitMQ management console
  SHELL
end
```

#### Explanation:

- **User Management**: Adds `vprofileuser` with appropriate permissions.
- **Firewall**: Opens ports `5672` (message broker) and `15672` (management console).

---

### 4. 🚀 Setup Application

The Application VM hosts the Java-based VProfile application.

```ruby
# Application VM
config.vm.define "app" do |app|
  app.vm.hostname = "app-vm"
  app.vm.network "private_network", ip: "192.168.56.103"
  app.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
  end
  app.vm.provision "shell", inline: <<-SHELL
    echo "🚀 Setting up Application VM..."
    sudo apt update

    # Install Java Development Kit
    sudo apt install -y openjdk-11-jdk

    # Install Tomcat
    sudo apt install -y tomcat9 tomcat9-admin

    # Deploy VProfile application
    sudo wget -O /var/lib/tomcat9/webapps/vprofile.war https://example.com/vprofile.war

    # Adjust permissions
    sudo chown tomcat:tomcat /var/lib/tomcat9/webapps/vprofile.war
    sudo systemctl restart tomcat9

    # Firewall configuration
    sudo ufw allow 8080 # Tomcat default port
  SHELL
end
```

#### Explanation:

- **Deployment**: Deploys the `vprofile.war` file to Tomcat.
- **Firewall**: Opens port `8080` for HTTP traffic to Tomcat.

---

### 5. 🌐 Setup Nginx

Nginx acts as a reverse proxy to route traffic to the Application VM.

```ruby
# Nginx VM
config.vm.define "nginx" do |nginx|
  nginx.vm.hostname = "nginx-vm"
  nginx.vm.network "private_network", ip: "192.168.56.104"
  nginx.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.cpus = 1
  end
  nginx.vm.provision "shell", inline: <<-SHELL
    echo "🚀 Setting up Nginx VM..."
    sudo apt update
    sudo apt install -y nginx

    # Configure Nginx as a reverse proxy
    sudo tee /etc/nginx/sites-available/vprofile <<EOF
    server {
        listen 80;
        server_name localhost;
        location / {
            proxy_pass http://192.168.56.103:8080;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
    EOF

    # Enable configuration and restart Nginx
    sudo ln -s /etc/nginx/sites-available/vprofile /etc/nginx/sites-enabled/
    sudo systemctl restart nginx

    # Allow HTTP traffic
    sudo ufw allow 80
  SHELL
end
```

#### Explanation:

- **Reverse Proxy**: Routes requests to `192.168.56.103:8080`.

---

### 6. ⚡ Setup Memcache

Memcache improves performance by caching frequently accessed data.

```ruby
# Memcache VM
config.vm.define "memcache" do |memcache|
  memcache.vm.hostname = "memcache-vm"
  memcache.vm.network "private_network", ip: "192.168.56.105"
  memcache.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.cpus = 1
  end
  memcache.vm.provision "shell", inline: <<-SHELL
    echo "🚀 Setting up Memcache VM..."
    sudo apt update
    sudo apt install -y memcached
    sudo systemctl enable --now memcached
  SHELL
end
```

#### Explanation:

- **Default Configuration**: Uses Memcache's default port `11211`.

---

### 7. 🗂️ Full Vagrantfile

Below is the complete `Vagrantfile`, combining all the sections for the **VProfile setup**:

```ruby
Vagrant.configure("2") do |config|
  # Common Base Box
  config.vm.box = "ubuntu/bionic64"

  # MySQL VM
  config.vm.define "mysql" do |mysql|
    mysql.vm.hostname = "mysql-vm"
    mysql.vm.network "private_network", ip: "192.168.56.101"
    mysql.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 2
    end
    mysql.vm.provision "shell", inline: <<-SHELL
      echo "🚀 Setting up MySQL VM..."
      sudo apt update
      sudo apt install -y mysql-server
      sudo systemctl enable --now mysql
      sudo sed -i "s/bind-address.*/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf
      sudo systemctl restart mysql
      sudo mysql -e "UPDATE mysql.user SET authentication_string=PASSWORD('root_password') WHERE User='root';"
      sudo mysql -e "DELETE FROM mysql.user WHERE User='';"
      sudo mysql -e "DROP DATABASE IF EXISTS test;"
      sudo mysql -e "FLUSH PRIVILEGES;"
      sudo mysql -e "CREATE DATABASE vprofiledb;"
      sudo mysql -e "CREATE USER 'vprofileuser'@'%' IDENTIFIED BY 'your_password';"
      sudo mysql -e "GRANT ALL PRIVILEGES ON vprofiledb.* TO 'vprofileuser'@'%';"
      sudo mysql -e "FLUSH PRIVILEGES;"
      sudo ufw allow 3306
    SHELL
  end

  # RabbitMQ VM
  config.vm.define "rabbitmq" do |rabbitmq|
    rabbitmq.vm.hostname = "rabbitmq-vm"
    rabbitmq.vm.network "private_network", ip: "192.168.56.102"
    rabbitmq.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 1
    end
    rabbitmq.vm.provision "shell", inline: <<-SHELL
      echo "🚀 Setting up RabbitMQ VM..."
      sudo apt update
      sudo apt install -y rabbitmq-server
      sudo systemctl enable --now rabbitmq-server
      sudo rabbitmqctl add_user vprofileuser your_password
      sudo rabbitmqctl set_permissions -p / vprofileuser ".*" ".*" ".*"
      sudo ufw allow 5672
      sudo ufw allow 15672
    SHELL
  end

  # Application VM
  config.vm.define "app" do |app|
    app.vm.hostname = "app-vm"
    app.vm.network "private_network", ip: "192.168.56.103"
    app.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 2
    end
    app.vm.provision "shell", inline: <<-SHELL
      echo "🚀 Setting up Application VM..."
      sudo apt update
      sudo apt install -y openjdk-11-jdk
      sudo apt install -y tomcat9 tomcat9-admin
      sudo wget -O /var/lib/tomcat9/webapps/vprofile.war https://example.com/vprofile.war
      sudo chown tomcat:tomcat /var/lib/tomcat9/webapps/vprofile.war
      sudo systemctl restart tomcat9
      sudo ufw allow 8080
    SHELL
  end

  # Nginx VM
  config.vm.define "nginx" do |nginx|
    nginx.vm.hostname = "nginx-vm"
    nginx.vm.network "private_network", ip: "192.168.56.104"
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 1
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      echo "🚀 Setting up Nginx VM..."
      sudo apt update
      sudo apt install -y nginx
      sudo tee /etc/nginx/sites-available/vprofile <<EOF
      server {
          listen 80;
          server_name localhost;
          location / {
              proxy_pass http://192.168.56.103:8080;
              proxy_set_header Host $host;
              proxy_set_header X-Real-IP $remote_addr;
          }
      }
      EOF
      sudo ln -s /etc/nginx/sites-available/vprofile /etc/nginx/sites-enabled/
      sudo systemctl restart nginx
      sudo ufw allow 80
    SHELL
  end

  # Memcache VM
  config.vm.define "memcache" do |memcache|
    memcache.vm.hostname = "memcache-vm"
    memcache.vm.network "private_network", ip: "192.168.56.105"
    memcache.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 1
    end
    memcache.vm.provision "shell", inline: <<-SHELL
      echo "🚀 Setting up Memcache VM..."
      sudo apt update
      sudo apt install -y memcached
      sudo systemctl enable --now memcached
    SHELL
  end
end
```

---

### 🎯 Final Notes

- **Testing**: Once all VMs are up, you can test the communication between components. For example:

  - Use MySQL's `mysql` command-line client to ensure the `vprofiledb` is accessible.
  - Check RabbitMQ's management interface via `http://192.168.56.102:15672`.
  - Validate the application is accessible through `http://192.168.56.104`.

- **Improving Security**: You can configure SSL/TLS for both Nginx and RabbitMQ for better security.

---
