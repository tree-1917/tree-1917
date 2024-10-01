# 📚 Swap Space Management in Linux: A Complete Guide

---

## 📋 Table of Contents

1. [What is Swap? 🔄](#1-what-is-swap)
2. [Recommended Swap Size 📏](#2-recommended-swap-size)
3. [Tools for Managing Swap 🛠️](#3-tools-for-managing-swap)
   - `dd`
   - `mkswap`
   - `swapon`
   - `swapoff`
4. [Creating and Managing Swap: Step-by-Step 📝](#4-creating-and-managing-swap-step-by-step)
5. [Dynamic Swap Space Script 📜](#5-dynamic-swap-space-script)
6. [Conclusion 🎯](#6-conclusion)

---

### 1. What is Swap? 🔄

**Swap** space in Linux is used as a "backup" for physical memory (RAM). When the RAM is fully utilized, inactive pages in memory are moved to the swap space, allowing active applications to keep running. This prevents system crashes and slowdowns in case of memory shortages.

> 📝 **Note**: Swap can exist as either a **swap file** or a **dedicated swap partition** on your disk.

---

### 2. Recommended Swap Size 📏

The recommended swap size depends on the amount of RAM and the system's usage:

| **RAM Size** | **Recommended Swap Size**              |
| ------------ | -------------------------------------- |
| ≤ 2 GB       | 2x the size of RAM                     |
| 2–8 GB       | Equal to the size of RAM               |
| ≥ 8 GB       | 0.5x the size of RAM or no swap needed |

For example:

- A system with **4 GB RAM** → **4 GB Swap**.
- A system with **16 GB RAM** → **8 GB Swap**.

---

### 3. Tools for Managing Swap 🛠️

Linux provides several utilities to manage swap space efficiently. Let’s explore the most important commands:

#### `dd`

`dd` is a command-line utility used to copy and convert files. It's also used to allocate space for a swap file. For example:

```bash
dd if=/dev/zero of=/swapfile bs=1G count=4 status=progress
```

This command creates a 4GB swap file.

#### `mkswap`

`mkswap` sets up a Linux swap area on a file or partition. This prepares the allocated space for use as swap:

```bash
mkswap /swapfile
```

#### `swapon`

`swapon` enables the swap space on a file or partition:

```bash
swapon /swapfile
```

#### `swapoff`

`swapoff` disables the swap space, which might be useful for reconfiguring swap:

```bash
swapoff /swapfile
```

---

### 4. Creating and Managing Swap: Step-by-Step 📝

Let’s break down the process of adding swap to your system in **5 easy steps**:

#### 🛠️ Step 1: Check Current Swap Space

To see how much swap space is currently available, run:

```bash
swapon --show
free -h
```

#### 🛠️ Step 2: Create a Swap File

You can create a swap file of the desired size using the `dd` command. For example, to create a 2 GB swap file:

```bash
sudo dd if=/dev/zero of=/swapfile bs=1G count=2 status=progress
```

#### 🛠️ Step 3: Secure the Swap File

Ensure that only root has access to the swap file:

```bash
sudo chmod 600 /swapfile
```

#### 🛠️ Step 4: Prepare and Enable Swap

Prepare the file to be used as swap space:

```bash
sudo mkswap /swapfile
```

Activate the swap file:

```bash
sudo swapon /swapfile
```

Verify that the swap is active:

```bash
swapon --show
```

#### 🛠️ Step 5: Make Swap Persistent

To ensure the swap is available after a reboot, add the swap file entry to `/etc/fstab`:

```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

### 5. Dynamic Swap Space Script 📜

Let’s automate the process of creating swap space dynamically based on the system’s RAM size.

```bash
#!/bin/bash

# Stage 1: Detect RAM Size
RAM_SIZE=$(free -g | awk '/^Mem:/{print $2}')  # RAM size in GB
echo "Total RAM Size: $RAM_SIZE GB"

# Stage 2: Define recommended swap size
if [ "$RAM_SIZE" -le 2 ]; then
    SWAP_SIZE=$(($RAM_SIZE * 2))  # 2x RAM for systems with ≤ 2 GB
elif [ "$RAM_SIZE" -le 8 ]; then
    SWAP_SIZE=$RAM_SIZE  # Equal to RAM for systems with 2-8 GB RAM
else
    SWAP_SIZE=$(($RAM_SIZE / 2))  # 0.5x RAM for systems with > 8 GB
fi
echo "Recommended Swap Size: $SWAP_SIZE GB"

# Stage 3: Create the swap file
echo "Creating a swap file of ${SWAP_SIZE}GB..."
dd if=/dev/zero of=/swapfile bs=1G count=$SWAP_SIZE status=progress

# Stage 4: Set up the swap file
echo "Setting up swap..."
chmod 600 /swapfile
mkswap /swapfile

# Stage 5: Activate swap
echo "Activating swap space..."
swapon /swapfile

# Stage 6: Add swap to /etc/fstab
if ! grep -q '/swapfile' /etc/fstab; then
    echo "/swapfile none swap sw 0 0" >> /etc/fstab
fi

# Stage 7: Verify swap
echo "Swap status:"
swapon --show
```

### 📝 Explanation of the Script:

1. **Stage 1**: The script determines the system's RAM size using the `free` command.
2. **Stage 2**: Based on the amount of RAM, the script calculates the recommended swap size.
3. **Stage 3**: It uses `dd` to create a swap file of the appropriate size.
4. **Stage 4**: The `mkswap` and `chmod` commands prepare the swap file and secure it.
5. **Stage 5**: The `swapon` command enables the swap space.
6. **Stage 6**: The swap is added to `/etc/fstab` to persist after reboots.
7. **Stage 7**: The script verifies that swap has been successfully enabled.

---

### 6. Conclusion 🎯

Swap space is an essential feature in Linux, helping your system perform better under high load conditions. With this guide, you now know how to manually create and manage swap space, as well as automate the process using a dynamic script that adjusts the swap size based on the system’s RAM. 🚀

Whether you're running out of memory or optimizing for performance, managing swap is a valuable skill for any Linux user.
