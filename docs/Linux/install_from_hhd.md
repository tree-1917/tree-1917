## 🚀 Booting Pop!\_OS from an ISO File on a Hard Drive (HP Laptop)

### 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Requirements](#2-requirements)
3. [Step-by-Step Guide](#3-step-by-step-guide)
   - [1. Boot to GRUB](#31-boot-to-grub)
   - [2. Use GRUB to Boot the ISO](#32-use-grub-to-boot-the-iso)
4. [Summary Script](#4-summary-script)

---

### 1. Introduction

If your computer has no operating system installed, but you already have a Pop!\_OS `.iso` file stored on a partition (e.g., `/dev/sda3`), this tutorial will help you boot directly from the ISO using the GRUB bootloader. This is especially useful when you don't want to create a bootable USB.

By the end of this tutorial, you'll be able to boot into Pop!\_OS and install it to your system without any external media. 🎉

---

### 2. Requirements

🔧 To complete this process, you need:

- A **Pop!\_OS ISO** file already stored on your hard drive in a partition (e.g., `/dev/sda3/ISO/pop.iso`).
- Access to **GRUB** either through your system or by booting from a **live USB**.
- Basic familiarity with the BIOS and boot menus.

---

### 3. Step-by-Step Guide

#### 3.1 Boot to GRUB

If your system has no OS installed and does not have GRUB configured, you’ll need to boot using a **live USB** that contains GRUB (any Linux distro like Ubuntu or Pop!\_OS). Follow these steps:

1. **Create a Live USB** on another machine.
2. **Insert the USB Stick** into your HP laptop.
3. **Enter BIOS** by pressing `Esc`, `F2`, `F10`, or `Del` during startup (the exact key depends on your model).
4. **Change Boot Order**: Prioritize booting from the USB stick and reboot.

Once booted from the USB, you should see the **GRUB menu**.

#### 3.2 Use GRUB to Boot the ISO

Once you're inside the **GRUB menu**:

1. **Enter GRUB Command Line**: Press `c` to open the command line interface.

2. **Locate the Partition**:

   - To find your ISO file on the correct partition, run:
     ```bash
     ls
     ```
     This lists the available partitions. In this example, the partition is `/dev/sda3`, so you may see something like `(hd0,gpt3)`.

3. **Set the Partition**:

   - Set the partition where your ISO is stored (e.g., `/dev/sda3`):
     ```bash
     set root=(hd0,gpt3)
     ```

4. **Create a Loopback Device**:

   - Load the ISO file into GRUB using loopback:
     ```bash
     loopback loop (hd0,gpt3)/ISO/pop.iso
     ```

5. **Load the Kernel**:

   - Load the Linux kernel from the ISO:
     ```bash
     linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=/ISO/pop.iso quiet splash
     ```

6. **Load the Initial RAM Disk (initrd)**:

   - Load the `initrd` from the ISO:
     ```bash
     initrd (loop)/casper/initrd
     ```

7. **Boot the System**:
   - Finally, boot into Pop!\_OS:
     ```bash
     boot
     ```

Pop!\_OS should now boot from the ISO on your hard drive! 🎉

---

### 4. Summary Script

To help you quickly perform the steps above, here’s a summary of the GRUB commands you need to run in order:

```bash
# Step 1: Set the partition where the ISO is stored
set root=(hd0,gpt3)

# Step 2: Create a loopback device for the ISO
loopback loop (hd0,gpt3)/ISO/pop.iso

# Step 3: Load the kernel from the ISO
linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=/ISO/pop.iso quiet splash

# Step 4: Load the initial RAM disk from the ISO
initrd (loop)/casper/initrd

# Step 5: Boot the system
boot
```

---

### 📝 Key Points to Remember

- Ensure the path to the ISO is correct. In this example, it is `/ISO/pop.iso` on `/dev/sda3`.
- The `set root` command must point to the correct partition where the ISO is stored (e.g., `(hd0,gpt3)`).
- If anything goes wrong, you can always retry or use a **Live USB** to access more tools for troubleshooting.

Good luck booting Pop!\_OS! 🎉
