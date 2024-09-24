# 189. LVM Configuration During Installation (CentOS) 💽

---

## Table of Contents 📑

1. [Overview of LVM (Logical Volume Management) 💡](#1-overview-of-lvm-logical-volume-management)
2. [Steps to Configure LVM During CentOS Installation 🖥️](#2-steps-to-configure-lvm-during-centos-installation)
   - [1. Start CentOS Installation 🚀](#1-start-centos-installation)
   - [2. Select Installation Destination 📍](#2-select-installation-destination)
   - [3. Choose LVM for Partitioning ⚙️](#3-choose-lvm-for-partitioning)
   - [4. Create a Volume Group (VG) 🗂️](#4-create-a-volume-group-vg)
   - [5. Create Logical Volumes (LVs) 📂](#5-create-logical-volumes-lvs)
   - [6. Allocate Physical Volumes (PVs) 📀](#6-allocate-physical-volumes-pvs)
   - [7. Configure Filesystem for Each Logical Volume 📝](#7-configure-filesystem-for-each-logical-volume)
   - [8. Configure Swap Partition 🔁](#8-configure-swap-partition)
   - [9. Save the Configuration and Proceed with Installation ✅](#9-save-the-configuration-and-proceed-with-installation)
3. [Example Configuration for CentOS LVM Layout 🔧](#example-configuration-for-centos-lvm-layout)
4. [Post-Installation LVM Management 🛠️](#post-installation-lvm-management)
5. [Benefits of LVM 🎯](#benefits-of-lvm)

---

## 1. Overview of LVM (Logical Volume Management) 💡

LVM (Logical Volume Management) is a powerful disk management tool in Linux that allows flexible and dynamic management of disk space. It lets you resize, add, or remove partitions without downtime, making it easier to handle growing storage needs. 🛠️

---

## 2. Steps to Configure LVM During CentOS Installation 🖥️

### 1. Start CentOS Installation 🚀

- Boot the system using the **CentOS Installation Media** (DVD/USB).
- Proceed through the initial steps of installation until you reach the **Installation Destination** screen.

### 2. Select Installation Destination 📍

- On the **Installation Destination** screen, select the disk to install CentOS.
- Choose **Custom** under **Storage Configuration** to configure partitions manually using LVM. 🛠️

### 3. Choose LVM for Partitioning ⚙️

- In the partitioning section, click **Create automatically** to let the system create default partitions.
- Alternatively, manually create partitions and set the **Device Type** to **LVM** for each partition. This configures your partitions as logical volumes (LVs). 📂

### 4. Create a Volume Group (VG) 🗂️

- A **Volume Group (VG)** is a collection of one or more physical volumes (PVs). To create a VG:
  - Click the **Volume Group** button.
  - Name the volume group (e.g., `vg_centos`).
  - Select the disks or physical volumes you want to include in the VG.
  - Click **Save** to complete the VG setup.

### 5. Create Logical Volumes (LVs) 📂

- Logical Volumes (LVs) are the partitions created within a volume group. For each partition:
  1. Click the **+** button to add a new mount point (e.g., `/`, `/home`, `/var`, `swap`).
  2. Set the **Device Type** to **LVM** and assign the mount point.
  3. Allocate the size for each LV.
  4. Repeat this process for all necessary partitions.

### 6. Allocate Physical Volumes (PVs) 📀

- **Physical Volumes (PVs)** are the actual disk partitions or entire disks that form the base of the LVM hierarchy.
  - Select the disks and allocate space as **Physical Volume** for LVM. 📀
  - These physical volumes will be grouped into the Volume Group created in the previous step.

### 7. Configure Filesystem for Each Logical Volume 📝

- For each logical volume (LV), assign the appropriate filesystem type:
  - `/boot`: Usually **ext4** or **XFS** (not part of LVM).
  - `/`: **XFS** (default filesystem in CentOS 7 and above).
  - `/home`: **XFS**.
  - `/var`: **XFS** or **ext4**.
- Ensure each LV has the correct mount point and size for your needs.

### 8. Configure Swap Partition 🔁

- Create a **swap** partition as a logical volume within the volume group:
  - Choose **LVM** and assign the mount point as `swap`.
  - Size it based on your system's RAM (typically 1.5x or 2x your physical memory). 🔄

### 9. Save the Configuration and Proceed with Installation ✅

- After setting up the LVM configuration, click **Done**.
- Review the summary of partitions and proceed with the installation if everything is correct. 📦

---

## Example Configuration for CentOS LVM Layout 🔧

Here’s an example of how you might configure an LVM layout:

| Mount Point | Logical Volume Name | Size | Filesystem Type | Volume Group |
| ----------- | ------------------- | ---- | --------------- | ------------ |
| `/boot`     | (Not under LVM)     | 1GB  | ext4            | N/A          |
| `/`         | lv_root             | 30GB | XFS             | vg_centos    |
| `/home`     | lv_home             | 50GB | XFS             | vg_centos    |
| `/var`      | lv_var              | 10GB | XFS             | vg_centos    |
| `swap`      | lv_swap             | 4GB  | swap            | vg_centos    |

---

## Post-Installation LVM Management 🛠️

LVM offers flexibility for managing storage **after installation**. Some useful commands include:

- **Extend a Logical Volume**:

  ```bash
  lvextend -L +10G /dev/vg_centos/lv_root
  ```

  Followed by resizing the filesystem:

  ```bash
  xfs_growfs /
  ```

- **Create a Snapshot**:
  ```bash
  lvcreate -L 5G -s -n lv_root_snapshot /dev/vg_centos/lv_root
  ```

---

## Benefits of LVM 🎯

- **Flexibility**: Resize partitions dynamically, without needing to reboot the system. 🔄
- **Snapshots**: Create snapshots of logical volumes for backup or testing purposes. 🗃️
- **Disk Management**: Easily add new disks to extend existing volume groups. 🛠️

By configuring **LVM** during CentOS installation, you ensure a flexible and scalable disk management system that can adapt to your growing storage needs without requiring downtime! 🎉
