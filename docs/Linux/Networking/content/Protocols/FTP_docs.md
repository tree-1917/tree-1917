---
title: 📂 FTP TutorialV
---

# 📂 FTP Tutorial: Installing, Configuring, and Using FTP on Linux

Welcome to this tutorial on FTP (File Transfer Protocol). In this guide, we will cover the basics of FTP, how to install and configure an FTP server on Linux, and how to transfer files between a Linux client and server.

## 📑 Table of Contents

1. [📄 What is FTP?](#-what-is-ftp)
2. [🔍 What is a Protocol?](#-what-is-a-protocol)
3. [🔢 What is the Default FTP Port?](#-what-is-the-default-ftp-port)
4. [🛠️ Installing and Configuring FTP on a Remote Server and Client](#-installing-and-configuring-ftp-on-a-remote-server-and-client)
5. [📁 Examples: Moving Files Between Linux Client and Server](#-examples-moving-files-between-linux-client-and-server)

---

## 📄 What is FTP?

**FTP** (File Transfer Protocol) is a standard network protocol used to transfer files between a client and a server over a TCP/IP network, such as the internet or a local network. FTP allows users to:

- 📥 Upload files
- 📤 Download files
- 📝 Rename files
- ❌ Delete files
- 📂 Move files on a server

FTP operates in two modes:

- **Active Mode** 📶: The client opens a port and waits for the server to connect.
- **Passive Mode** 🌐: The server opens a port and waits for the client to connect.

## 🔍 What is a Protocol?

A **Protocol** is a set of rules or standards that dictate how data is transmitted and received over a network. Protocols define the methods and formats for communication between devices, ensuring that data is sent, received, and interpreted correctly. Examples of protocols include:

- **HTTP** 🕸️: HyperText Transfer Protocol
- **FTP** 📂: File Transfer Protocol
- **TCP/IP** 🌐: Transmission Control Protocol/Internet Protocol
- **SMTP** 📧: Simple Mail Transfer Protocol

## 🔢 What is the Default FTP Port?

The **Default FTP Port 21** is the port number assigned to the control connection in the FTP protocol. When an FTP client initiates a connection to an FTP server, it connects to the server on port 21 to send commands. Data transfers typically occur over a separate port (usually port 20 or a dynamically assigned port in passive mode).

## 🛠️ Installing and Configuring FTP on a Remote Server and Client

### 📡 On the FTP Server:

1. **Install FTP Server Software:**

   - On a Debian/Ubuntu server:
     ```bash
     sudo apt update
     sudo apt install vsftpd
     ```
   - On a CentOS/RHEL server:
     ```bash
     sudo yum install vsftpd
     ```

2. **Configure the FTP Server:**

   - Open the configuration file:
     ```bash
     sudo nano /etc/vsftpd.conf
     ```
   - Modify the following settings as needed:
     - Ensure `anonymous_enable=NO` (to disable anonymous login).
     - Set `local_enable=YES` (to enable local user login).
     - Set `write_enable=YES` (to allow file uploads).
     - Enable passive mode by adding the following:
       ```bash
       pasv_enable=YES
       pasv_min_port=10000
       pasv_max_port=10100
       ```
     - **Enable local time** by adding:
       ```bash
       use_localtime=YES
       ```
   - Save and close the file.

3. **Start and Enable the FTP Server:**

   ```bash
   sudo systemctl start vsftpd
   sudo systemctl enable vsftpd
   ```

4. **Allow FTP through the Firewall:**
   - On Debian/Ubuntu:
     ```bash
     sudo ufw allow 21/tcp
     sudo ufw allow 10000:10100/tcp
     ```
   - On CentOS/RHEL:
     ```bash
     sudo firewall-cmd --permanent --add-port=21/tcp
     sudo firewall-cmd --permanent --add-port=10000-10100/tcp
     sudo firewall-cmd --reload
     ```

### 💻 On the FTP Client:

1. **Install FTP Client Software:**

   - On a Debian/Ubuntu client:
     ```bash
     sudo apt install ftp
     ```
   - On a CentOS/RHEL client:
     ```bash
     sudo yum install ftp
     ```

2. **Connect to the FTP Server:**
   - Open a terminal and connect using the FTP command:
     ```bash
     ftp your_server_ip_or_hostname
     ```
   - Enter the username and password when prompted.

## 📁 Examples: Moving Files Between Linux Client and Server

### 📤 Upload Files

To upload a file from your local machine to the server:

```bash
ftp your_server_ip
# Login with your username and password
put /path/to/local/file /path/to/remote/directory/
```

### 📥 Download Files

To download a file from the server to your local machine:

```bash
ftp your_server_ip
# Login with your username and password
get /path/to/remote/file /path/to/local/directory/
```

### 📝 Rename Files

To rename a file on the server:

```bash
ftp your_server_ip
# Login with your username and password
rename oldfile.txt newfile.txt
```

### ❌ Delete Files

To delete a file on the server:

```bash
ftp your_server_ip
# Login with your username and password
delete file_to_delete.txt
```

### 📂 Move Files on a Server

To move a file from one directory to another on the server:

```bash
ftp your_server_ip
# Login with your username and password
rename /current/path/file.txt /new/path/file.txt
```

This tutorial provides an overview of FTP, its installation, configuration, and basic usage for file transfers between Linux systems. Happy file transferring! 📂🚀

---
