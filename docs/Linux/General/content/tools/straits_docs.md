### 🖥️ **Stratis Storage Management Tutorial** 🖥️

---

## 📑 **Table of Contents** 📑

1. [What is Stratis?](#1-what-is-stratis)
2. [Why Do We Need Stratis?](#2-why-do-we-need-stratis)
3. [Installing Stratis](#3-installing-stratis)
4. [Creating and Managing Stratis Pools](#4-creating-and-managing-stratis-pools)
   - 4.1 [Create Stratis Pool](#41-create-stratis-pool)
   - 4.2 [Create File System in Stratis](#42-create-file-system-in-stratis)
   - 4.3 [Mount File System](#43-mount-file-system)
   - 4.4 [Extend Stratis Pool](#44-extend-stratis-pool)
   - 4.5 [Take a Snapshot](#45-take-a-snapshot)
5. [Common Stratis Commands](#5-common-stratis-commands)
6. [Practical Stratis Script](#6-practical-stratis-script)
7. [Summary](#7-summary)

---

### 1️⃣ **What is Stratis?**

**Stratis** is a storage management solution designed to simplify the process of managing storage pools and file systems on Linux. It combines features from **LVM** and **ZFS** while offering a more straightforward interface. Stratis allows you to manage pools made from different storage devices and provides features like:

- **Thin provisioning** 🌐: Allocate space dynamically.
- **Snapshots** 📸: Quickly save the state of a file system.
- **Tiered Storage** 🗂️: Combine SSDs and HDDs for optimal performance.

---

### 2️⃣ **Why Do We Need Stratis?**

Stratis makes advanced storage management tasks easy to perform. You need Stratis when you:

- Manage **multiple storage devices**.
- Require **easy-to-use snapshots**.
- Want to **expand storage** dynamically without manual partition resizing.
- Need a **user-friendly alternative** to LVM or ZFS.

---

### 3️⃣ **Installing Stratis**

Before using Stratis, ensure it's installed and the service is running.

```bash
# Install Stratis on CentOS/RHEL
sudo yum install -y stratisd stratis-cli

# Start and enable Stratis service
sudo systemctl start stratisd
sudo systemctl enable stratisd
```

---

### 4️⃣ **Creating and Managing Stratis Pools**

#### 4.1 **Create Stratis Pool**

To create a Stratis pool, you must specify one or more storage devices. In this example, we will create a pool called `mystorage` using `/dev/sdb` and `/dev/sdc`.

```bash
# Create a Stratis pool named 'mystorage'
sudo stratis pool create mystorage /dev/sdb /dev/sdc
```

#### 4.2 **Create File System in Stratis**

After creating the pool, you can create a file system within the pool. Let’s create a file system named `mystoragefs`.

```bash
# Create a file system named 'mystoragefs'
sudo stratis filesystem create mystorage mystoragefs
```

#### 4.3 **Mount File System**

To use the file system, you need to mount it to a directory. We will mount `mystoragefs` to `/mnt/mystorage`.

```bash
# Create the mount point
sudo mkdir -p /mnt/mystorage

# Mount the Stratis file system
sudo mount /stratis/mystorage/mystoragefs /mnt/mystorage
```

#### 4.4 **Extend Stratis Pool**

To add more storage to an existing Stratis pool, simply add another disk to the pool.

```bash
# Add /dev/sdd to the existing 'mystorage' pool
sudo stratis pool add-data mystorage /dev/sdd
```

#### 4.5 **Take a Snapshot**

Snapshots are useful for backups or creating test environments. Here’s how to take a snapshot of the `mystoragefs` file system.

```bash
# Create a snapshot named 'snapshot1'
sudo stratis filesystem snapshot mystorage mystoragefs snapshot1
```

---

### 5️⃣ **Common Stratis Commands**

| **Command**                   | **Description**                                  |
| ----------------------------- | ------------------------------------------------ |
| `stratis pool create`         | Create a new Stratis storage pool.               |
| `stratis filesystem create`   | Create a file system within a pool.              |
| `stratis filesystem snapshot` | Create a snapshot of a file system.              |
| `stratis pool add-data`       | Add a device to an existing pool.                |
| `stratis filesystem list`     | List file systems in a Stratis pool.             |
| `stratis pool list`           | List all Stratis pools.                          |
| `stratis pool destroy`        | Destroy a Stratis pool and all its file systems. |
| `stratis filesystem destroy`  | Destroy a file system in a pool.                 |

---

### 6️⃣ **Practical Stratis Script**

Here’s a script demonstrating the steps to create a pool, file system, and snapshot using Stratis.

```bash
#!/bin/bash

# Stage 1: Install Stratis if not installed
echo "🔄 Installing Stratis..."
sudo yum install -y stratisd stratis-cli

# Stage 2: Start and enable Stratis daemon
echo "🚀 Starting Stratis daemon..."
sudo systemctl start stratisd
sudo systemctl enable stratisd

# Stage 3: Create a Stratis pool using /dev/sdb and /dev/sdc
echo "💾 Creating Stratis pool 'mystorage'..."
sudo stratis pool create mystorage /dev/sdb /dev/sdc

# Stage 4: Create a file system in the pool
echo "📝 Creating file system 'mystoragefs'..."
sudo stratis filesystem create mystorage mystoragefs

# Stage 5: Create a mount point and mount the file system
echo "📂 Mounting file system to /mnt/mystorage..."
sudo mkdir -p /mnt/mystorage
sudo mount /stratis/mystorage/mystoragefs /mnt/mystorage

# Stage 6: Take a snapshot of the file system
echo "📸 Taking a snapshot of the file system..."
sudo stratis filesystem snapshot mystorage mystoragefs snapshot1

echo "✅ Stratis setup completed successfully!"
```

---

### 7️⃣ **Summary**

**Stratis** offers a powerful, user-friendly approach to managing storage pools and file systems on Linux systems. With features like **thin provisioning**, **snapshots**, and **tiered storage**, Stratis simplifies complex storage tasks. Whether you're managing multiple disks or taking snapshots for backups, Stratis provides a flexible, efficient solution for system administrators.

By following this tutorial, you’ve learned how to:

- Install Stratis and start the daemon.
- Create and extend Stratis pools.
- Manage file systems and snapshots using practical commands.

---
