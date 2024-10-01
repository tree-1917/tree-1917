---
title: Chrony - NTP Implementation ⏲️
---

# Chrony - NTP Implementation ⏲️

### Table of Contents

- [What is Chrony? 🌐](#what-is-chrony-)
- [Purpose of Chrony 🎯](#purpose-of-chrony-)
- [Chrony Package 📦](#chrony-package-)
- [Chrony Configuration File 🛠️](#chrony-configuration-file-)
  - [`/etc/chrony/chrony.conf`](#etcchronychronyconf)
- [Chrony Log File 🗂️](#chrony-log-file-)
  - [`/var/log/chrony`](#varlogchrony)
- [Chrony Service ⚙️](#chrony-service-)
- [Chronyc Program 🛠️](#chronyc-program-)

---

## What is Chrony? 🌐

**Chrony** is a versatile implementation of the Network Time Protocol (NTP), designed to synchronize the system clock with NTP servers, reference clocks (e.g., GPS receivers), or manually set time. It is particularly useful on systems that are frequently suspended, or on networks with intermittent connections.

---

## Purpose of Chrony 🎯

Chrony is designed to perform well under a variety of conditions, including:

- **Fast and Accurate Synchronization**: Chrony can quickly bring the system clock into synchronization and maintain accuracy over time.
- **Adaptive Behavior**: It handles systems with unstable or variable network connections better than traditional NTP daemons.
- **Low Network Load**: Suitable for systems where network load should be minimized, as it can synchronize with servers less frequently.

Chrony is often used as a replacement for the older `ntpd` (Network Time Protocol Daemon) due to its improved performance and flexibility.

---

## Chrony Package 📦

Chrony is available as a package in most Linux distributions. Here's how to install it:

### Installation Commands:

- **Debian/Ubuntu**:

  ```bash
  sudo apt install chrony
  ```

- **CentOS/RHEL**:

  ```bash
  sudo yum install chrony
  ```

- **Fedora**:
  ```bash
  sudo dnf install chrony
  ```

---

## Chrony Configuration File 🛠️

The main configuration file for Chrony is `/etc/chrony/chrony.conf`. This file is where you configure the NTP servers, options, and other settings for the Chrony daemon.

### `/etc/chrony/chrony.conf`

- **Purpose**: The configuration file defines how Chrony will operate, including which NTP servers to use, how to handle drift, and other important settings.
- **Location**: `/etc/chrony/chrony.conf`

### Example of `/etc/chrony/chrony.conf`:

```bash
# Use public servers from the pool.ntp.org project.
server 0.pool.ntp.org iburst
server 1.pool.ntp.org iburst
server 2.pool.ntp.org iburst
server 3.pool.ntp.org iburst

# Allow Chrony to correct large clock deviations
makestep 1.0 3

# Drift file to compensate for hardware clock inaccuracies
driftfile /var/lib/chrony/drift

# Log measurements of system clock error
log tracking

# Enable the monitoring command
allow 192.168.1.0/24
```

### Key Directives:

- **`server`**: Specifies the NTP servers to synchronize with.
- **`makestep`**: Allows Chrony to correct the system clock immediately if the offset is larger than the specified threshold.
- **`driftfile`**: Location where Chrony stores the drift data to adjust the system clock over time.
- **`log`**: Specifies what kind of data Chrony should log.
- **`allow`**: Defines which IP addresses are allowed to connect to Chrony for monitoring.

---

## Chrony Log File 🗂️

Chrony logs its operations in the `/var/log/chrony` directory. This log file contains important information about the synchronization process and any errors or warnings encountered by Chrony.

### `/var/log/chrony`

- **Purpose**: To store logs related to Chrony’s operation, including information about time adjustments, server status, and any issues.
- **Location**: `/var/log/chrony`

### Viewing the Chrony Log:

You can view the Chrony log using a command like:

```bash
sudo cat /var/log/chrony/chrony.log
```

Or, if you want to monitor the log in real-time:

```bash
sudo tail -f /var/log/chrony/chrony.log
```

---

## Chrony Service ⚙️

Chrony runs as a service, which can be managed using standard `systemctl` commands. This service is responsible for keeping the system clock synchronized with the configured NTP servers.

### Managing the Chrony Service:

- **Start the Chrony Service**:

  ```bash
  sudo systemctl start chronyd
  ```

- **Enable the Chrony Service to Start on Boot**:

  ```bash
  sudo systemctl enable chronyd
  ```

- **Restart the Chrony Service**:

  ```bash
  sudo systemctl restart chronyd
  ```

- **Check the Status of the Chrony Service**:
  ```bash
  sudo systemctl status chronyd
  ```

---

## Chronyc Program 🛠️

**Chronyc** is a command-line tool used to interact with the Chrony daemon. It allows you to monitor and control Chrony's operation, providing information about the synchronization process and server status.

### Key Commands with `chronyc`:

- **Check Chrony’s Tracking Status**:

  ```bash
  chronyc tracking
  ```

  This command shows the current status of Chrony, including the current system time, the last offset, and more.

- **List NTP Servers and Their Status**:

  ```bash
  chronyc sources -v
  ```

  This command provides detailed information about the NTP servers Chrony is using, including their stratum level, polling interval, and more.

- **Manually Force a Synchronization**:

  ```bash
  chronyc makestep
  ```

  This command forces Chrony to immediately correct the system clock if the time difference is too large.

- **Interactive Mode**:
  ```bash
  chronyc
  ```
  This command launches an interactive mode where you can issue various commands to monitor and control Chrony.

---
