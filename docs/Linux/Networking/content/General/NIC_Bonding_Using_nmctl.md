---
title: How to Use `nmcli` to Create NIC Bonding
---

## **How to Use `nmcli` to Create NIC Bonding**

### **Table of Contents**

1. [Introduction](#1-introduction)
2. [Step 1: Check Existing Network Interfaces 🖥️](#2-step-1-check-existing-network-interfaces)
3. [Step 2: Create the Bond Interface 🔗](#3-step-2-create-the-bond-interface)
4. [Step 3: Add Slaves to the Bond Interface ➕](#4-step-3-add-slaves-to-the-bond-interface)
5. [Step 4: Configure IP Settings 🌐](#5-step-4-configure-ip-settings)
6. [Step 5: Bring Up the Bonded Interface 🆙](#6-step-5-bring-up-the-bonded-interface)
7. [Step 6: Verify Your Setup ✅](#7-step-6-verify-your-setup)
8. [Conclusion 🎯](#8-conclusion)

---

### 1. **Introduction**

Network bonding (or NIC bonding) combines multiple network interfaces into one logical interface to improve redundancy and throughput. Let’s use `nmcli` to create NIC bonding on a Linux system! 🚀

---

### 2. **Step 1: Check Existing Network Interfaces 🖥️**

First, list all your network interfaces:

```bash
nmcli device status
```

🔍 Identify the interfaces you want to bond (e.g., `eth0`, `eth1`).

---

### 3. **Step 2: Create the Bond Interface 🔗**

Create the bond interface using `nmcli`:

```bash
nmcli connection add type bond con-name bond0 ifname bond0 mode active-backup
```

Explanation:

- `con-name bond0`: Name of the bonded connection.
- `ifname bond0`: Name of the bonded interface.
- `mode active-backup`: Bonding mode. You can choose modes like `balance-rr`, `802.3ad`, etc.

---

### 4. **Step 3: Add Slaves to the Bond Interface ➕**

Add the identified interfaces as slaves to the bond:

```bash
nmcli connection add type ethernet con-name slave-eth0 ifname eth0 master bond0
nmcli connection add type ethernet con-name slave-eth1 ifname eth1 master bond0
```

🔗 This attaches `eth0` and `eth1` as slaves to the `bond0` interface.

---

### 5. **Step 4: Configure IP Settings 🌐**

Assign an IP address to the bond:

```bash
nmcli connection modify bond0 ipv4.addresses 192.168.1.100/24
nmcli connection modify bond0 ipv4.method manual
nmcli connection up bond0
```

Replace `192.168.1.100/24` with the correct IP and subnet for your network.

---

### 6. **Step 5: Bring Up the Bonded Interface 🆙**

Activate the bond and its slaves:

```bash
nmcli connection up bond0
nmcli connection up slave-eth0
nmcli connection up slave-eth1
```

🎉 Your bonded interface is now active!

---

### 7. **Step 6: Verify Your Setup ✅**

Check if everything is working correctly:

```bash
nmcli connection show
```

You can also check the bonding details:

```bash
cat /proc/net/bonding/bond0
```

---

### 8. **Conclusion 🎯**

You’ve successfully created NIC bonding using `nmcli`! Your setup should now provide better network resilience and potentially increased throughput.

---
