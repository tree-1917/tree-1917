---
title: DNS - Domain Name System 🧭
---

# DNS - Domain Name System 🧭

### Table of Contents

- [Introduction to DNS 🌐](#introduction-to-dns)
- [Installing and Configuring DNS 🛠️](#installing-and-configuring-dns-)
- [Creating a DNS Server with a Shell Script 🤖](#creating-a-dns-server-with-a-shell-script-)
- [Types of Hostname Records 📝](#types-of-hostname-records-)
- [Key Configuration Files 📂](#key-configuration-files-)
- [Managing the DNS Service ⚙️](#managing-the-dns-service-)
- [Example Zone File with a Table Format 🗂️](#example-zone-file-with-a-table-format-)

---

## Introduction to DNS 🌐

DNS (Domain Name System) is the backbone of the internet, translating human-readable domain names like `example.com` into machine-readable IP addresses like `192.0.2.1`. This system allows users to access websites using easy-to-remember names rather than numeric IP addresses.

---

## Installing and Configuring DNS 🛠️

To set up a DNS server on Linux, follow these steps:

### 1. Install the DNS Server Software

Depending on your Linux distribution, the installation command may vary:

- **Debian/Ubuntu**:

  ```bash
  sudo apt install bind9
  ```

- **CentOS/RHEL**:
  ```bash
  sudo yum install bind bind-utils
  ```

### 2. Configure the DNS Server

After installation, the DNS server needs to be configured. This involves editing the main configuration file and creating zone files.

- **Edit `/etc/named.conf`**:  
  This file is the main configuration file where server settings and zones are defined. Open it with your preferred text editor:

  ```bash
  sudo nano /etc/named.conf
  ```

- **Example configuration**:

  ```bash
  options {
      directory "/var/named";
      allow-query { any; };
      recursion yes;

      # 🧭 Listen on specified IP addresses
      listen-on port 53 { 127.0.0.1; 192.0.2.1; };
  };

  zone "example.com" IN {
      type master;
      file "/var/named/example.com.zone";
  };

  zone "2.0.192.in-addr.arpa" IN {
      type master;
      file "/var/named/2.0.192.rev";
  };
  ```

  - **listen-on**: The `listen-on` directive tells the DNS server which IP addresses to listen on for DNS queries. In this example, the server listens on `127.0.0.1` (localhost) and `192.0.2.1`.

### 3. Start and Enable the DNS Service

After configuring, start the DNS service and enable it to start on boot:

```bash
sudo systemctl start named
sudo systemctl enable named
```

---

## Creating a DNS Server with a Shell Script 🤖

Here’s a shell script that automates the setup of a basic DNS server with 10 dummy websites, including the `listen-on` directive:

```bash
#!/bin/bash

# 🛠️ Install BIND DNS server
echo "Installing BIND DNS server..."
sudo apt install -y bind9

# 📝 Backup the original configuration
echo "Backing up the original configuration..."
sudo cp /etc/named.conf /etc/named.conf.backup

# ⚙️ Basic configuration setup
echo "Configuring DNS server..."
sudo tee /etc/named.conf > /dev/null <<EOL
options {
    directory "/var/named";
    allow-query { any; };
    recursion yes;

    # 🧭 Listen on specified IP addresses
    listen-on port 53 { 127.0.0.1; 192.0.2.1; };
};

zone "example.com" IN {
    type master;
    file "/var/named/example.com.zone";
};

zone "2.0.192.in-addr.arpa" IN {
    type master;
    file "/var/named/2.0.192.rev";
};
EOL

# 🗂️ Create zone files
echo "Creating zone files..."

# Forward Zone File
sudo tee /var/named/example.com.zone > /dev/null <<EOL
\$TTL 86400
@   IN  SOA     ns1.example.com. admin.example.com. (
        2024090101  ; Serial
        3600        ; Refresh
        1800        ; Retry
        1209600     ; Expire
        86400 )     ; Minimum TTL
    IN  NS      ns1.example.com.

ns1 IN  A       192.0.2.1
www IN  A       192.0.2.2
site1 IN A      192.0.2.3
site2 IN A      192.0.2.4
site3 IN A      192.0.2.5
site4 IN A      192.0.2.6
site5 IN A      192.0.2.7
site6 IN A      192.0.2.8
site7 IN A      192.0.2.9
site8 IN A      192.0.2.10
site9 IN A      192.0.2.11
site10 IN A     192.0.2.12
EOL

# Reverse Zone File
sudo tee /var/named/2.0.192.rev > /dev/null <<EOL
\$TTL 86400
@   IN  SOA     ns1.example.com. admin.example.com. (
        2024090101  ; Serial
        3600        ; Refresh
        1800        ; Retry
        1209600     ; Expire
        86400 )     ; Minimum TTL
    IN  NS      ns1.example.com.

1   IN  PTR     ns1.example.com.
2   IN  PTR     www.example.com.
3   IN  PTR     site1.example.com.
4   IN  PTR     site2.example.com.
5   IN  PTR     site3.example.com.
6   IN  PTR     site4.example.com.
7   IN  PTR     site5.example.com.
8   IN  PTR     site6.example.com.
9   IN  PTR     site7.example.com.
10  IN  PTR     site8.example.com.
11  IN  PTR     site9.example.com.
12  IN  PTR     site10.example.com.
EOL

# 🔄 Restart DNS service to apply changes
echo "Restarting DNS service..."
sudo systemctl restart named

echo "DNS server setup complete with 10 dummy websites."
```

### Explanation of the Script:

1. **Installation**: The script installs the `bind9` package, which is the DNS server software.
2. **Configuration**: It includes the `listen-on` directive in `/etc/named.conf` to specify that the DNS server should listen on `127.0.0.1` and `192.0.2.1`.
3. **Zone Files**: The script generates two zone files:
   - **Forward Zone File**: Maps hostnames (`ns1`, `www`, `site1`, etc.) to IP addresses (`192.0.2.1` to `192.0.2.12`).
   - **Reverse Zone File**: Maps IP addresses back to their corresponding hostnames.
4. **Restart DNS Service**: The DNS service is restarted to apply the new configuration and zone files.

This script creates a DNS setup with 10 dummy websites (`site1.example.com` to `site10.example.com`) and dummy IP addresses. The DNS server listens on the specified IP addresses (`127.0.0.1` and `192.0.2.1`) for incoming DNS queries.

---

## Types of Hostname Records 📝

- **PTR Record (IP to Hostname)**: Maps an IP address to a hostname, typically used for reverse DNS lookups.

  Example:

  ```bash
  2.0.192.in-addr.arpa. IN PTR www.example.com.
  ```

- **A Record (Hostname to IP)**: Maps a hostname to an IP address.

  Example:

  ```bash
  www.example.com. IN A 192.0.2.2
  ```

- **CNAME Record (Hostname to Hostname)**: Maps one hostname to another, useful for domain aliasing.

  Example:

  ```bash
  mail.example.com. IN CNAME www.example.com.
  ```

---

## Key Configuration Files 📂

- **`/etc/named.conf`**: The primary configuration file for the BIND DNS server. It includes options, logging settings, and zone declarations.

- **`/var/named`**: The directory where zone files are stored. These files contain DNS records for the domains managed by the server.

---

## Managing the DNS Service ⚙️

### Restarting the DNS Service

Whenever you make changes to the configuration, restart the DNS service to apply them:

```bash
sudo systemctl restart named
```

Use `systemctl` commands to manage the service status, start, stop, and enable it on boot.

---

## Example Zone File with a Table Format 🗂️

Here's an example of how to organize DNS records in a zone file using

a table format for clarity:

### Zone File Example: `example.com.zone`

| Website Name       | IP Address | Record Type | Record                 |
| ------------------ | ---------- | ----------- | ---------------------- |
| ns1.example.com    | 192.0.2.1  | A           | ns1 IN A 192.0.2.1     |
| www.example.com    | 192.0.2.2  | A           | www IN A 192.0.2.2     |
| site1.example.com  | 192.0.2.3  | A           | site1 IN A 192.0.2.3   |
| site2.example.com  | 192.0.2.4  | A           | site2 IN A 192.0.2.4   |
| site3.example.com  | 192.0.2.5  | A           | site3 IN A 192.0.2.5   |
| site4.example.com  | 192.0.2.6  | A           | site4 IN A 192.0.2.6   |
| site5.example.com  | 192.0.2.7  | A           | site5 IN A 192.0.2.7   |
| site6.example.com  | 192.0.2.8  | A           | site6 IN A 192.0.2.8   |
| site7.example.com  | 192.0.2.9  | A           | site7 IN A 192.0.2.9   |
| site8.example.com  | 192.0.2.10 | A           | site8 IN A 192.0.2.10  |
| site9.example.com  | 192.0.2.11 | A           | site9 IN A 192.0.2.11  |
| site10.example.com | 192.0.2.12 | A           | site10 IN A 192.0.2.12 |

---
