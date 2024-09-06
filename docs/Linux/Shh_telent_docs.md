---
title: 🌐 Understanding Telnet and SSH
---

# 🌐 **Understanding Telnet and SSH**

This tutorial covers the basics of Telnet and SSH, their packages, and their roles in network services. We'll explore the differences between client and server packages and provide common examples.

## 📑 **Table of Contents**

1. [🔍 What is Telnet?](#-what-is-telnet)
2. [🔐 What is SSH?](#-what-is-ssh)
3. [📦 Types of Packages for Network Services](#-types-of-packages-for-network-services)
   - [Client Package](#-client-package)
   - [Server Package](#-server-package)
4. [🔧 Common Commands and Examples](#-common-commands-and-examples)

---

## 🔍 **What is Telnet?**

**Telnet** is a network protocol used to provide a command-line interface for communication with a remote device or server. It operates over TCP/IP and allows users to access remote systems as if they were local. However, Telnet transmits data, including passwords, in plain text, making it less secure compared to modern protocols.

### **Common Telnet Commands**

- **Connect to a Remote Host:**

  ```bash
  telnet <hostname> <port>
  ```

- **Example:**

  ```bash
  telnet example.com 23
  ```

- **Exit Telnet Session:**
  - Press `Ctrl + ]`, then type `quit`.

---

## 🔐 **What is SSH?**

**SSH** (Secure Shell) is a protocol used to securely access and manage remote systems over a network. Unlike Telnet, SSH encrypts the data transmitted between the client and the server, providing a secure channel over an unsecured network.

### **Common SSH Commands**

- **Connect to a Remote Host:**

  ```bash
  ssh <username>@<hostname>
  ```

- **Example:**

  ```bash
  ssh user@example.com
  ```

- **Copy Files Over SSH:**

  ```bash
  scp <local_file> <username>@<hostname>:<remote_path>
  ```

- **Example:**

  ```bash
  scp myfile.txt user@example.com:/home/user/
  ```

- **Exit SSH Session:**
  - Type `exit` or press `Ctrl + D`.

---

## 📦 **Types of Packages for Network Services**

### **Client Package**

Client packages are used to connect to remote services. For Telnet and SSH, these packages are installed on the client machine to initiate connections.

- **Telnet Client Package:**

  - **Debian/Ubuntu:**
    ```bash
    sudo apt install telnet
    ```
  - **CentOS/RHEL:**
    ```bash
    sudo yum install telnet
    ```

- **SSH Client Package:**
  - **Debian/Ubuntu:**
    ```bash
    sudo apt install openssh-client
    ```
  - **CentOS/RHEL:**
    ```bash
    sudo yum install openssh-clients
    ```

### **Server Package**

Server packages are used to provide services that clients can connect to. These packages are installed on the server machine.

- **Telnet Server Package:**

  - **Debian/Ubuntu:**
    ```bash
    sudo apt install telnetd
    ```
  - **CentOS/RHEL:**
    ```bash
    sudo yum install telnet-server
    ```

- **SSH Server Package:**
  - **Debian/Ubuntu:**
    ```bash
    sudo apt install openssh-server
    ```
  - **CentOS/RHEL:**
    ```bash
    sudo yum install openssh-server
    ```

---

## 🔧 **Common Commands and Examples**

### **Telnet Examples**

- **Check if Telnet Service is Running:**

  ```bash
  sudo systemctl status telnet
  ```

- **Configure Telnet Server:**
  - Edit `/etc/xinetd.d/telnet` to enable or disable Telnet service.

### **SSH Examples**

- **Start SSH Service:**

  ```bash
  sudo systemctl start ssh
  ```

- **Enable SSH to Start at Boot:**

  ```bash
  sudo systemctl enable ssh
  ```

- **Configure SSH Server:**
  - Edit `/etc/ssh/sshd_config` to adjust settings like port, permit root login, etc.

---
