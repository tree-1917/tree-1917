# 🚀 Creating a Kickstart File for Network Installation on Fedora

This guide will show you how to create a **Kickstart file** and make it available over the network for automated Fedora installations.

---

### 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Requirements](#2-requirements)
3. [Creating a Kickstart File](#3-creating-a-kickstart-file)
4. [Setting up a Network Server](#4-setting-up-a-network-server)
5. [Accessing the Kickstart File via Network](#5-accessing-the-kickstart-file-via-network)
6. [Testing the Network Boot](#6-testing-the-network-boot)
7. [Summary Script](#7-summary-script)

---

### 1. Introduction

A **Kickstart file** is used to automate Fedora installations, which saves time by providing pre-configured installation parameters. By serving this file over the network, you can use it on multiple machines without needing a USB stick or CD.

---

### 2. Requirements

To set up and access a Kickstart file over the network, you need:

- **A Fedora-based server** or any Linux machine with web or FTP server installed.
- A **Kickstart file** (`.ks` file).
- **Client machines** that support network boot (PXE boot).

---

### 3. Creating a Kickstart File

First, you need to create a basic **Kickstart file**.

1. **Generate a Kickstart File**:
   You can use the `anaconda-ks.cfg` file, which is generated after every Fedora installation. The file can be found at `/root/anaconda-ks.cfg`.

   To create or modify a Kickstart file:

   ```bash
   sudo nano /path/to/your/kickstart.ks
   ```

2. **Basic Example of a Kickstart File**:

   ```bash
   # Kickstart Config for Automated Fedora Installation

   # Language
   lang en_US.UTF-8

   # Keyboard layout
   keyboard us

   # Network settings
   network --bootproto=dhcp

   # Root password
   rootpw --iscrypted <encrypted_password>

   # Partitioning
   autopart --type=lvm

   # Package Selection
   %packages
   @base
   @core
   %end
   ```

---

### 4. Setting up a Network Server

Next, you need to make this file available over the network. There are two popular methods to host a Kickstart file over the network: **HTTP/HTTPS** and **FTP**.

#### A. Using HTTP Server

1. **Install Apache HTTP Server**:

   ```bash
   sudo dnf install httpd -y
   ```

2. **Start the Apache Server**:

   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

3. **Move the Kickstart File to the Web Directory**:
   Place your Kickstart file (`kickstart.ks`) in the default web directory `/var/www/html`:

   ```bash
   sudo cp /path/to/your/kickstart.ks /var/www/html/kickstart.ks
   ```

4. **Ensure Permissions**:
   Make sure the web server can access the file:
   ```bash
   sudo chmod 644 /var/www/html/kickstart.ks
   ```

#### B. Using FTP Server

1. **Install vsftpd (FTP Server)**:

   ```bash
   sudo dnf install vsftpd -y
   ```

2. **Start and Enable FTP Service**:

   ```bash
   sudo systemctl start vsftpd
   sudo systemctl enable vsftpd
   ```

3. **Move Kickstart File to FTP Directory**:
   By default, FTP serves files from `/var/ftp/pub`. Copy the Kickstart file there:

   ```bash
   sudo cp /path/to/your/kickstart.ks /var/ftp/pub/kickstart.ks
   ```

4. **Enable FTP Access** by modifying `/etc/vsftpd/vsftpd.conf` and restarting the service.

---

### 5. Accessing the Kickstart File via Network

On the client machine, you will specify the location of the Kickstart file during installation:

1. **For HTTP**:
   During boot, pass the following option:

   ```bash
   inst.ks=http://<server-ip>/kickstart.ks
   ```

2. **For FTP**:
   ```bash
   inst.ks=ftp://<server-ip>/pub/kickstart.ks
   ```

Make sure to replace `<server-ip>` with the actual IP address of your network server.

---

### 6. Testing the Network Boot

1. **Configure PXE Boot**:
   On the client machine, ensure PXE boot is enabled. This will allow the machine to boot from the network.
2. **Start the Installation**:
   - Boot the machine.
   - Access the boot menu (usually by pressing `F12`, `Esc`, or `Del`).
   - Select **Network Boot**.
   - Specify the Kickstart file’s URL using `inst.ks=` as shown above.

The Fedora installer will use the Kickstart file from the network and proceed with an automated installation.

---

### 7. Summary Script

Here’s a summary of what you’ve done to access a Kickstart file over the network:

```bash
# On the server:
# Install Apache HTTP server
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

# Copy the Kickstart file to the web directory
sudo cp /path/to/your/kickstart.ks /var/www/html/kickstart.ks
sudo chmod 644 /var/www/html/kickstart.ks

# On the client:
# During boot, pass this argument (for HTTP)
inst.ks=http://<server-ip>/kickstart.ks

# Or for FTP (if FTP server is set up)
inst.ks=ftp://<server-ip>/pub/kickstart.ks
```

---
