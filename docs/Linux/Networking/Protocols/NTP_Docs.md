---
title: NTP - Network Time Protocol ⏰
---

# NTP - Network Time Protocol ⏰

### Table of Contents

- [What is NTP? 🌐](#what-is-ntp-)
- [Purpose of NTP 🎯](#purpose-of-ntp-)
- [NTP Configuration File 📂](#ntp-configuration-file-)
  - [`/etc/ntp.conf`](#etcntpconf)
- [NTP Service ⚙️](#ntp-service-)
  - [Managing the NTP Service](#managing-the-ntp-service)
- [NTP Command Line Tool 🛠️](#ntp-command-line-tool-)
  - [`ntpq`](#ntpq)

---

## What is NTP? 🌐

**NTP (Network Time Protocol)** is a protocol used to synchronize the clocks of computers over a network. NTP is essential for ensuring that all devices on a network have the same accurate time, which is critical for various network functions, such as logging events, coordinating operations, and maintaining security protocols.

---

## Purpose of NTP 🎯

The main purpose of NTP is to keep the system time of all computers within a network synchronized to a precise reference time, usually a time source from the internet or a local time server.

### Key Benefits:

- **Consistency**: Ensures that all systems in a network maintain the same time.
- **Accuracy**: Keeps clocks accurate by adjusting the time based on discrepancies.
- **Security**: Ensures that time-based operations (e.g., certificates, authentication) function correctly.

---

## NTP Configuration File 📂

The primary configuration file for NTP is `/etc/ntp.conf`. This file is used to define various settings, including the servers that the system will use to synchronize time.

### `/etc/ntp.conf`

- **Purpose**: The `/etc/ntp.conf` file contains the configuration for the NTP daemon, including the list of time servers, logging options, and other NTP settings.
- **Location**: `/etc/ntp.conf`

### Example of `/etc/ntp.conf`:

```bash
# /etc/ntp.conf - NTP configuration file

# Specify the servers NTP should use for synchronization
server 0.pool.ntp.org iburst
server 1.pool.ntp.org iburst
server 2.pool.ntp.org iburst
server 3.pool.ntp.org iburst

# Drift file to compensate for hardware clock inaccuracies
driftfile /var/lib/ntp/drift

# Access control for NTP queries
restrict default kod nomodify notrap nopeer noquery
restrict 127.0.0.1
restrict ::1
```

- **Server Configuration**: Lists the NTP servers to synchronize with.
- **Drift File**: Specifies the location of the drift file, which is used to adjust for the inherent inaccuracies of the hardware clock.
- **Access Control**: Defines who can access the NTP server and what they can do.

---

## NTP Service ⚙️

NTP operates as a service that runs in the background, continuously synchronizing the system clock with the specified NTP servers.

### Managing the NTP Service

To manage the NTP service, you can use `systemctl` commands:

- **Start the NTP Service**:

  ```bash
  sudo systemctl start ntp
  ```

- **Enable the NTP Service to Start on Boot**:

  ```bash
  sudo systemctl enable ntp
  ```

- **Restart the NTP Service**:

  ```bash
  sudo systemctl restart ntp
  ```

- **Check the Status of the NTP Service**:

  ```bash
  sudo systemctl status ntp
  ```

- **Common Mistake**: The command `systemctl restart ntpq` is incorrect. The correct service name is `ntp`.

---

## NTP Command Line Tool 🛠️

NTP includes several command-line tools for managing and querying the NTP service. One of the most commonly used tools is `ntpq`.

### `ntpq`

- **Purpose**: The `ntpq` command is used to monitor NTP daemon operations and to query the status of NTP servers.
- **Usage**: It allows you to check which servers are being used for synchronization and their status.

### Common `ntpq` Commands:

- **Check NTP Server Peers**:

  ```bash
  ntpq -p
  ```

  Output:

  ```
       remote           refid      st t when poll reach   delay   offset  jitter
  ==============================================================================
  *0.pool.ntp.org   .GPS.            1 u   44   64  377   30.938    1.273   0.512
  +1.pool.ntp.org   .PPS.            1 u   40   64  377   31.456    0.987   0.498
  ```

  - `remote`: The NTP server.
  - `refid`: The reference ID (time source).
  - `st`: Stratum level of the server.
  - `when`: Time since the last response.
  - `poll`: Polling interval.
  - `reach`: Reachability status.
  - `delay`: Network delay to the server.
  - `offset`: The time difference between your system and the server.
  - `jitter`: The stability of the connection.

- **Check NTP Server Status**:

  ```bash
  ntpq -c rv
  ```

- **Interactive Mode**:
  ```bash
  ntpq
  ```
  In interactive mode, you can type commands such as `peers` to see the list of peers.

---
