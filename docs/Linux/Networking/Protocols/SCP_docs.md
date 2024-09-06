---
title: 📂 SCP Tutorial
---

# 📂 SCP Tutorial: Understanding, Installing, and Using SCP on Linux

In this tutorial, we'll explore SCP (Secure Copy Protocol), compare it with FTP (File Transfer Protocol), and guide you through the process of installing and configuring SCP on a Linux server and client. We'll also provide examples of using SCP for file transfers.

## 📑 Table of Contents

1. [📄 What is SCP?](#-what-is-scp)
2. [🔍 SCP vs. FTP: A Comparison](#-scp-vs-ftp-a-comparison)
3. [🔢 What is the Default SCP Port?](#-what-is-the-default-scp-port)
4. [🛠️ Installing and Configuring SCP on a Remote Server and Client](#-installing-and-configuring-scp-on-a-remote-server-and-client)
5. [📁 Examples: Using SCP for File Transfers](#-examples-using-scp-for-file-transfers)

---

## 📄 What is SCP?

**SCP** (Secure Copy Protocol) is a network protocol that securely transfers files between a local and a remote host or between two remote hosts. SCP is based on SSH (Secure Shell), providing strong encryption and authentication, making it more secure than FTP.

SCP operates over SSH, using port 22 by default. It ensures that both the authentication and data transfer are encrypted, protecting sensitive information from being intercepted during transmission.

## 🔍 SCP vs. FTP: A Comparison

| Feature                    | **SCP**                                      | **FTP**                                                   |
| -------------------------- | -------------------------------------------- | --------------------------------------------------------- |
| **Security**               | High (Encrypted via SSH)                     | Low (Data is not encrypted)                               |
| **Default Port**           | 22                                           | 21                                                        |
| **Authentication**         | SSH Keys, Password                           | Username, Password                                        |
| **File Transfer Modes**    | Secure (SCP over SSH)                        | Active, Passive                                           |
| **Ease of Use**            | Command-line based, simple syntax            | GUI and command-line options                              |
| **Transfer Speed**         | Fast (minimal overhead due to encryption)    | Can be slower (depending on mode and settings)            |
| **Firewall Compatibility** | Works well with firewalls                    | May require additional ports (especially in passive mode) |
| **Use Case**               | Secure file transfer between trusted systems | General-purpose file transfer                             |

## 🔢 What is the Default SCP Port?

The **Default SCP Port 22** is the same as the default SSH port. SCP operates over SSH, so when you initiate an SCP connection, it uses port 22 by default to establish a secure, encrypted communication channel.

## 🛠️ Installing and Configuring SCP on a Remote Server and Client

### 📡 On the SCP Server:

1. **Ensure SSH is Installed:**

   - On a Debian/Ubuntu server:
     ```bash
     sudo apt update
     sudo apt install openssh-server
     ```
   - On a CentOS/RHEL server:
     ```bash
     sudo yum install openssh-server
     ```

2. **Start and Enable the SSH Service:**

   ```bash
   sudo systemctl start sshd
   sudo systemctl enable sshd
   ```

3. **Configure the SSH Server (Optional):**

   - Open the configuration file:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Modify the following settings as needed:
     - To change the default port (from 22 to another), add or modify:
       ```bash
       Port 2222
       ```
     - Ensure `PermitRootLogin` is set to `no` for security:
       ```bash
       PermitRootLogin no
       ```
     - Save and close the file.

4. **Restart the SSH Service:**
   ```bash
   sudo systemctl restart sshd
   ```

### 💻 On the SCP Client:

1. **Ensure SCP is Installed:**

   - On a Debian/Ubuntu client:
     ```bash
     sudo apt install openssh-client
     ```
   - On a CentOS/RHEL client:
     ```bash
     sudo yum install openssh-clients
     ```

2. **Connect to the Remote Server Using SCP:**
   - SCP is ready to use with the SSH service running on the server. You can start transferring files as needed.

## 📁 Examples: Using SCP for File Transfers

### Example 1: Easy - Copying a Single File from Local to Remote

```bash
scp /path/to/local/file.txt username@remote_host:/path/to/remote/directory/
```

This command copies `file.txt` from your local machine to the specified directory on the remote host.

### Example 2: Mid - Copying a Directory Recursively from Remote to Local

```bash
scp -r username@remote_host:/path/to/remote/directory/ /path/to/local/directory/
```

The `-r` option is used to copy a directory and its contents recursively from the remote server to your local machine.

### Example 3: Mid - Copying Files Between Two Remote Servers

```bash
scp username1@remote_host1:/path/to/remote/file.txt username2@remote_host2:/path/to/destination/
```

This command copies a file from one remote server to another, using your local machine as a relay.

### Example 4: Hard - Using a Non-Default Port and SSH Key Authentication

```bash
scp -P 2222 -i /path/to/private_key username@remote_host:/path/to/remote/file.txt /path/to/local/directory/
```

- The `-P 2222` option specifies a non-default port (2222).
- The `-i` option specifies the path to your private SSH key for authentication.
- This command securely copies a file from a remote server to your local machine.

---

This tutorial provides a comprehensive overview of SCP, its installation, configuration, and basic usage for secure file transfers between Linux systems. Happy file transferring! 📂🔐

---
