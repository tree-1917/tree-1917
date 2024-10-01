# 🛠️ Full System Partitioning with LVM on CentOS (with `/boot/efi`, `/boot`, `/var`, `/swap`, `/opt`, `/home`, `/root`, and `/recovery`)

This tutorial will guide you through setting up a complete CentOS system with multiple partitions, utilizing **LVM (Logical Volume Management)** where applicable. You'll configure `/boot/efi`, `/boot` (without LVM), and use LVM for `/var`, `/swap`, `/opt`, `/home`, `/root`, and `/recovery`.

---

## 📋 Table of Contents

1. [System Overview 💡](#system-overview)
2. [Boot into CentOS Installation Mode 🖥️](#boot-into-centos-installation-mode)
3. [Partitioning Disk Layout 📊](#partitioning-disk-layout)
4. [Creating Physical Volumes (PV) for LVM 📦](#creating-physical-volumes-pv-for-lvm)
5. [Creating Volume Groups (VG) for LVM 📚](#creating-volume-groups-vg-for-lvm)
6. [Creating Logical Volumes (LV) 🔧](#creating-logical-volumes-lv)
7. [Formatting the Partitions and Logical Volumes 📝](#formatting-the-partitions-and-logical-volumes)
8. [Mounting Partitions and Logical Volumes 🗄️](#mounting-partitions-and-logical-volumes)
9. [Updating `/etc/fstab` for Automatic Mounting on Boot 🔄](#updating-etc-fstab-for-automatic-mounting-on-boot)
10. [Verifying the Setup ✅](#verifying-the-setup)
11. [Summary Script 📜](#summary-script)

---

### 1. System Overview 💡

For this installation, we will create the following partitions:

- **Non-LVM Partitions:**

  - `/boot/efi` (EFI partition)
  - `/boot` (Standard boot partition)

- **LVM Partitions:**
  - `/var` (for system logs and temporary data)
  - `/swap` (swap space)
  - `/opt` (for optional software installations)
  - `/home` (user directories)
  - `/root` (root user directory)
  - `/recovery` (for backups or recovery data)

---

### 2. Boot into CentOS Installation Mode 🖥️

1. Insert your **CentOS installation media** (USB or DVD).
2. Boot the system and choose the **Installation Mode**.
3. At the installation summary screen, select **Manual Partitioning**.

---

### 3. Partitioning Disk Layout 📊

#### Step 1: Create `/boot/efi` (Non-LVM)

- Size: **512MB** (Recommended)
- Filesystem: **EFI System Partition** (`/boot/efi`)

#### Step 2: Create `/boot` (Non-LVM)

- Size: **1GB**
- Filesystem: **ext4** (`/boot`)

#### Step 3: Create an LVM Partition for All Other Mount Points

1. **Create a Physical Volume (PV)** on the remaining disk space for LVM. We will do this in the next section.

2. Once the LVM is created, we will assign the following mount points using logical volumes:
   - `/var`: **5GB** (ext4)
   - `/swap`: **4GB** (swap partition)
   - `/opt`: **3GB** (ext4)
   - `/home`: **10GB** (ext4)
   - `/root`: **5GB** (ext4)
   - `/recovery`: **5GB** (ext4)

---

### 4. Creating Physical Volumes (PV) for LVM 📦

Once your partitions for `/boot` and `/boot/efi` are created, you can allocate the remaining disk space for **LVM**. Open the terminal or use a virtual console (press `Ctrl + Alt + F2`) and run the following commands:

```bash
# Stage 1: Create a physical volume on the remaining disk space
pvcreate /dev/sda3  # Replace with your actual disk partition (usually after /boot)

# Verify the physical volume creation
pvs
```

---

### 5. Creating Volume Groups (VG) for LVM 📚

Next, we will create a **Volume Group** (VG) to manage all the logical volumes:

```bash
# Stage 2: Create a volume group named vg_main
vgcreate vg_main /dev/sda3

# Verify the volume group
vgs
```

---

### 6. Creating Logical Volumes (LV) 🔧

Now, create **Logical Volumes** (LV) for each of the mount points (`/var`, `/swap`, `/opt`, `/home`, `/root`, `/recovery`):

```bash
# Stage 3: Create logical volumes for each mount point
lvcreate -L 5G -n lv_var vg_main     # /var
lvcreate -L 4G -n lv_swap vg_main    # /swap
lvcreate -L 3G -n lv_opt vg_main     # /opt
lvcreate -L 10G -n lv_home vg_main   # /home
lvcreate -L 5G -n lv_root vg_main    # /root
lvcreate -L 5G -n lv_recovery vg_main # /recovery

# Verify the logical volumes
lvs
```

---

### 7. Formatting the Partitions and Logical Volumes 📝

Now that we have the logical volumes created, format them using the appropriate filesystems:

```bash
# Stage 4: Format non-LVM partitions
mkfs.vfat /dev/sda1  # /boot/efi
mkfs.ext4 /dev/sda2  # /boot

# Format the LVM partitions
mkfs.ext4 /dev/vg_main/lv_var     # /var
mkswap /dev/vg_main/lv_swap       # /swap
mkfs.ext4 /dev/vg_main/lv_opt     # /opt
mkfs.ext4 /dev/vg_main/lv_home    # /home
mkfs.ext4 /dev/vg_main/lv_root    # /root
mkfs.ext4 /dev/vg_main/lv_recovery # /recovery
```

---

### 8. Mounting Partitions and Logical Volumes 🗄️

Once the filesystems are created, we can now mount each partition:

```bash
# Stage 5: Mount non-LVM partitions
mount /dev/sda2 /mnt/boot       # /boot
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi   # /boot/efi

# Mount LVM partitions
mount /dev/vg_main/lv_var /mnt/var        # /var
mount /dev/vg_main/lv_opt /mnt/opt        # /opt
mount /dev/vg_main/lv_home /mnt/home      # /home
mount /dev/vg_main/lv_root /mnt/root      # /root
mount /dev/vg_main/lv_recovery /mnt/recovery # /recovery

# Enable swap
swapon /dev/vg_main/lv_swap               # /swap
```

---

### 9. Updating `/etc/fstab` for Automatic Mounting on Boot 🔄

To ensure the partitions are automatically mounted during boot, update the `/etc/fstab` file:

```bash
# Stage 6: Open /etc/fstab in an editor
nano /mnt/etc/fstab

# Add the following entries

/dev/sda1  /boot/efi   vfat    defaults    0 0
/dev/sda2  /boot       ext4    defaults    0 2
/dev/vg_main/lv_var    /var     ext4       defaults    0 2
/dev/vg_main/lv_opt    /opt     ext4       defaults    0 2
/dev/vg_main/lv_home   /home    ext4       defaults    0 2
/dev/vg_main/lv_root   /root    ext4       defaults    0 2
/dev/vg_main/lv_recovery /recovery ext4    defaults    0 2
/dev/vg_main/lv_swap   none     swap       sw          0 0
```

---

### 10. Verifying the Setup ✅

Finally, verify that all partitions are mounted correctly:

```bash
# Stage 7: Check the partition table and mounted partitions
lsblk

# Verify swap
swapon --show

# Reboot and check if everything mounts correctly
reboot
```

---

### 11. Summary Script 📜

Here’s a summary script that encapsulates the steps in a single script format with stage comments:

```bash
#!/bin/bash

# Stage 1: Create Physical Volume
pvcreate /dev/sda3

# Stage 2: Create Volume Group
vgcreate vg_main /dev/sda3

# Stage 3: Create Logical Volumes
lvcreate -L 5G -n lv_var vg_main
lvcreate -L 4G -n lv_swap vg_main
lvcreate -L 3G -n lv_opt vg_main
lvcreate -L 10G -n lv_home vg_main
lvcreate -L 5G -n lv_root vg_main
lvcreate -L 5G -n lv_recovery vg_main

# Stage 4: Format Partitions
mkfs.vfat /dev/sda1      # /

boot/efi
mkfs.ext4 /dev/sda2      # /boot
mkfs.ext4 /dev/vg_main/lv_var
mkswap /dev/vg_main/lv_swap
mkfs.ext4 /dev/vg_main/lv_opt
mkfs.ext4 /dev/vg_main/lv_home
mkfs.ext4 /dev/vg_main/lv_root
mkfs.ext4 /dev/vg_main/lv_recovery

# Stage 5: Mount Partitions
mount /dev/sda2 /mnt/boot
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
mount /dev/vg_main/lv_var /mnt/var
mount /dev/vg_main/lv_opt /mnt/opt
mount /dev/vg_main/lv_home /mnt/home
mount /dev/vg_main/lv_root /mnt/root
mount /dev/vg_main/lv_recovery /mnt/recovery
swapon /dev/vg_main/lv_swap

# Stage 6: Update /etc/fstab
cat <<EOL >> /mnt/etc/fstab
/dev/sda1  /boot/efi   vfat    defaults    0 0
/dev/sda2  /boot       ext4    defaults    0 2
/dev/vg_main/lv_var    /var     ext4       defaults    0 2
/dev/vg_main/lv_opt    /opt     ext4       defaults    0 2
/dev/vg_main/lv_home   /home    ext4       defaults    0 2
/dev/vg_main/lv_root   /root    ext4       defaults    0 2
/dev/vg_main/lv_recovery /recovery ext4    defaults    0 2
/dev/vg_main/lv_swap   none     swap       sw          0 0
EOL

# Stage 7: Verify Setup
lsblk
swapon --show
```

---

### 📝 Conclusion

This guide has provided a comprehensive approach to partitioning a CentOS system using LVM. By following the steps outlined, you can ensure a robust and flexible partitioning scheme tailored to your needs. Don't forget to back up your data regularly! 🚀
