# 📚 Tutorial: Managing System Time with `timedatectl`

In this tutorial, you'll learn about `timedatectl`, a versatile command-line tool for managing system time settings. We'll explore how it replaces the old `date` command, how to synchronize time with NTP servers, and walk through common `timedatectl` commands. The tutorial includes a practical lab, a summarized shell script for quick reference, and a visual guide.

---

### **Table of Contents**

1. [What is `timedatectl`?](#1-what-is-timedatectl)
2. [Common `timedatectl` Options](#2-common-timedatectl-options)
3. [Lab: Common Usage of `timedatectl`](#3-lab-common-usage-of-timedatectl)
4. [Lab Summary in a Shell Script](#4-lab-summary-in-a-shell-script)
5. [Visual Guide](#5-visual-guide)

---

### **1️⃣ What is `timedatectl`?**

`timedatectl` is a command-line utility for querying and changing the system clock and its settings. It is part of the `systemd` suite and replaces the older `date` command. Beyond setting the time and date, `timedatectl` can synchronize the system clock with remote NTP (Network Time Protocol) servers.

#### Key Features:

- **Replacement for `date` Command:** Provides a modern interface for managing system time.
- **Time Synchronization:**
  - Use NTP servers by enabling either `chronyd` or `ntpd` services and setting NTP to yes in `timedatectl`.
  - Alternatively, use the `systemd-timesyncd` daemon to synchronize time, which replaces `ntpd` and `chronyd`.

---

### **2️⃣ Common `timedatectl` Options**

`timedatectl` offers several options to manage system time settings efficiently. Here are some of the most commonly used commands:

1. **Display System Time Status:**

   - Shows the current time, date, and time zone settings, along with the NTP synchronization status.
   - **Command:**
     ```bash
     timedatectl status
     ```

2. **Set the Hardware Clock (RTC):**

   - Adjust the hardware clock to local time or UTC.
   - **Command:**
     ```bash
     sudo timedatectl set-local-rtc 1
     ```
   - **Example:**
     ```bash
     sudo timedatectl set-local-rtc 0  # Use UTC
     ```

3. **Adjust NTP Settings:**

   - Enable or disable NTP synchronization.
   - **Command to Enable NTP:**
     ```bash
     sudo timedatectl set-ntp true
     ```
   - **Command to Disable NTP:**
     ```bash
     sudo timedatectl set-ntp false
     ```

4. **Set the System Time Directly:**

   - Set both the system date and time in one command.
   - **Command:**
     ```bash
     sudo timedatectl set-time 'YYYY-MM-DD HH:MM:SS'
     ```

5. **View and Select Time Zones:**
   - List all available time zones and set the system to a preferred one.
   - **Command to List Time Zones:**
     ```bash
     timedatectl list-timezones
     ```
   - **Command to Set Time Zone:**
     ```bash
     sudo timedatectl set-timezone Your/TimeZone
     ```

---

### **3️⃣ Lab: Common Usage of `timedatectl`**

This lab covers practical examples of using `timedatectl`. Follow these steps to manage your system's time settings:

1. **Check the Current Time Status:**

   - **Command:**
     ```bash
     timedatectl
     ```

2. **View All Available Time Zones:**

   - **Command:**
     ```bash
     timedatectl list-timezones
     ```

3. **Set the Time Zone:**

   - **Command:**
     ```bash
     sudo timedatectl set-timezone America/New_York
     ```

4. **Set the Date:**

   - **Command:**
     ```bash
     sudo timedatectl set-time '2024-09-10'
     ```

5. **Set the Date and Time:**

   - **Command:**
     ```bash
     sudo timedatectl set-time '2024-09-10 14:30:00'
     ```

6. **Enable NTP Synchronization:**
   - **Command:**
     ```bash
     sudo timedatectl set-ntp true
     ```

---

### **4️⃣ Lab Summary in a Shell Script**

Here's a shell script that encapsulates all the commands from the lab. Save it as `timedatectl_lab.sh` and run it to execute all the steps in sequence.

```bash
#!/bin/bash

# 🕒 Lab: Common Usage of timedatectl

# 1️⃣ Check the current time status
echo "🕒 Current system time, date, and time zone:"
timedatectl

# 2️⃣ View all available time zones
echo "🌐 Listing all available time zones:"
timedatectl list-timezones

# 3️⃣ Set the time zone (example: 'America/New_York')
echo "🌍 Setting time zone to 'America/New_York':"
sudo timedatectl set-timezone America/New_York

# 4️⃣ Set the date (example: '2024-09-10')
echo "📅 Setting date to '2024-09-10':"
sudo timedatectl set-time '2024-09-10'

# 5️⃣ Set the date and time (example: '2024-09-10 14:30:00')
echo "⏰ Setting date and time to '2024-09-10 14:30:00':"
sudo timedatectl set-time '2024-09-10 14:30:00'

# 6️⃣ Enable NTP synchronization
echo "🔄 Enabling NTP synchronization:"
sudo timedatectl set-ntp true

# 🏁 End of lab
echo "✅ Lab completed. Please review the outputs above."
```

---
