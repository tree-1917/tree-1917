---
title: 📦 Understanding NIC Bonding in Linux
description: A comprehensive guide to NIC bonding in Linux, including its benefits, modes, and configuration steps.
---

# 📦 Understanding NIC Bonding in Linux

Network Interface Card (NIC) bonding is a technique used to combine multiple network interfaces into a single logical interface to achieve redundancy and/or improved performance. This guide will explore the benefits of NIC bonding, its various modes, and how to configure it in a Linux environment.

## 📋 Table of Contents

- [What is NIC Bonding?](#what-is-nic-bonding)
- [How Does NIC Bonding Work?](#how-does-nic-bonding-work)
  - [Bonding Modes](#bonding-modes)
- [How to Configure NIC Bonding](#how-to-configure-nic-bonding)
  - [Step 1: Install Bonding Module](#step-1-install-bonding-module)
  - [Step 2: Create Bonding Configuration](#step-2-create-bonding-configuration)
  - [Step 3: Configure Slave Interfaces](#step-3-configure-slave-interfaces)
  - [Step 4: Restart Networking](#step-4-restart-networking)
  - [Step 5: Verify Bonding](#step-5-verify-bonding)
- [Summary](#summary)

---

## 🔍 What is NIC Bonding?

NIC bonding involves combining multiple physical network interfaces (NICs) into a single logical interface. This approach provides:

- **Increased Bandwidth**: Aggregates bandwidth from multiple NICs.
- **Redundancy**: Offers failover capabilities if one NIC fails.
- **Load Balancing**: Distributes network traffic across multiple NICs.

## 🔧 How Does NIC Bonding Work?

NIC bonding uses a bonding driver that supports various modes to achieve different objectives. Each mode offers specific features and requirements.

### 🧩 Bonding Modes

| **Mode**                         | **Description**                                                      | **Benefit**                                                             | **Requirement**                         |
| -------------------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------------------- | --------------------------------------- |
| **Mode 0 (Balance-Round-Robin)** | Distributes packets in a round-robin fashion across all NICs.        | Load balancing and redundancy.                                          | Switch must support this mode.          |
| **Mode 1 (Active-Backup)**       | Only one NIC is active at a time; others are standby.                | Failover redundancy; no special switch configuration needed.            | No special switch configuration needed. |
| **Mode 2 (Balance-XOR)**         | Uses XOR operation on MAC addresses to determine packet routing.     | Load balancing with redundancy.                                         | Switch must support this mode.          |
| **Mode 3 (Broadcast)**           | Transmits packets to all NICs in the bond.                           | Ensures all NICs receive all traffic; useful for monitoring.            | No special switch configuration needed. |
| **Mode 4 (802.3ad - LACP)**      | Aggregates links using LACP (Link Aggregation Control Protocol).     | Load balancing and redundancy; requires switch support for LACP.        | Switch must support LACP.               |
| **Mode 5 (Balance-TLB)**         | Balances outbound traffic based on NIC load.                         | Load balancing with redundancy; no special switch configuration needed. | No special switch configuration needed. |
| **Mode 6 (Balance-ALB)**         | Balances both inbound and outbound traffic; includes ARP monitoring. | Load balancing and redundancy; no special switch configuration needed.  | No special switch configuration needed. |

## 🔧 How to Configure NIC Bonding

Follow these steps to configure NIC bonding on a Linux system:

### Step 1: Install Bonding Module

Ensure the bonding module is installed and loaded.

```bash
sudo modprobe bonding
```

### Step 2: Create Bonding Configuration

Create a configuration file for the bonded interface. For example, on systems using `ifcfg` files (like RHEL/CentOS):

```bash
sudo vi /etc/sysconfig/network-scripts/ifcfg-bond0
```

**Example Configuration**:

```ini
DEVICE=bond0
BONDING_OPTS="mode=1 miimon=100"
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.1.10
NETMASK=255.255.255.0
GATE_WAY=192.168.0.1
```

### Step 3: Configure Slave Interfaces

Edit the configuration files for each NIC that will be part of the bond. For example:

```bash
sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

**Example Configuration**:

```ini
TYPE=Ethernet
DEVICE=eth0
MASTER=bond0
SLAVE=yes
BOOTPROTO=none
ONBOOT=yes
HWADDR=e8:84:a5:36:0b:af
```

Repeat this for other NICs (`eth1`, `eth2`, etc.).

### Step 4: Restart Networking

Restart the networking service to apply changes.

```bash
sudo systemctl restart network
```

### Step 5: Verify Bonding

Check the bonding status to ensure it's configured correctly.

```bash
cat /proc/net/bonding/bond0
```

## 📚 Summary

NIC bonding in Linux allows for combining multiple network interfaces into a single logical interface, offering benefits like increased bandwidth, redundancy, and load balancing. By configuring bonding modes and following setup steps, you can optimize network performance and reliability.

Happy configuring! 💻🚀

---
s