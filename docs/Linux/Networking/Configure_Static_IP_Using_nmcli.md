---
title: How to Configure a Static IP Address with nmcli
---

# **How to Configure a Static IP Address with `nmcli`**

### **Table of Contents**

1. [Introduction](#1-introduction)
2. [Step 1: Identify the Network Interface 🖥️](#2-step-1-identify-the-network-interface)
3. [Step 2: Configure the Primary Static IP Address 🌐](#3-step-2-configure-the-primary-static-ip-address)
4. [Step 3: Add a Secondary Static IP Address ➕](#4-step-3-add-a-secondary-static-ip-address)
5. [Step 4: Set DNS Servers (Optional) 🛠️](#5-step-4-set-dns-servers-optional)
6. [Step 5: Bring Up the Interface 🆙](#6-step-5-bring-up-the-interface)
7. [Step 6: Verify the Configuration ✅](#7-step-6-verify-the-configuration)
8. [Conclusion 🎯](#8-conclusion)

---

### 1. **Introduction**

Setting a static IP address is important for devices that require a consistent network address. Additionally, in some cases, you may need to configure multiple IP addresses on the same interface. This tutorial will guide you through configuring both primary and secondary static IP addresses using the `nmcli` tool.

---

### 2. **Step 1: Identify the Network Interface 🖥️**

First, identify the network interface you want to configure:

```bash
nmcli device status
```

You will see a list of interfaces, such as `eth0`, `enp0s3`, or `wlp3s0`. Choose the one you want to assign static IPs to.

---

### 3. **Step 2: Configure the Primary Static IP Address 🌐**

Let’s first configure the primary static IP address. Replace `<interface>` with your interface name, and set the IP address, gateway, and subnet:

```bash
nmcli connection modify <interface> ipv4.addresses 192.168.1.100/24
nmcli connection modify <interface> ipv4.gateway 192.168.1.1
nmcli connection modify <interface> ipv4.method manual
```

Explanation:

- `ipv4.addresses 192.168.1.100/24`: The static IP address (`192.168.1.100`) and subnet mask (`/24`).
- `ipv4.gateway 192.168.1.1`: The gateway address for your network.
- `ipv4.method manual`: Configures the interface to use manual (static) IP settings instead of DHCP.

---

### 4. **Step 3: Add a Secondary Static IP Address ➕**

To add a secondary (or additional) IP address to the same interface, use the following command:

```bash
nmcli connection modify <interface> +ipv4.addresses 192.168.1.101/24
```

Explanation:

- The `+ipv4.addresses` option appends the additional IP address (`192.168.1.101`) to the interface.

You can add more secondary IPs similarly if needed.

---

### 5. **Step 4: Set DNS Servers (Optional) 🛠️**

Optionally, specify DNS servers:

```bash
nmcli connection modify <interface> ipv4.dns "8.8.8.8 8.8.4.4"
```

This sets Google’s public DNS servers (`8.8.8.8` and `8.8.4.4`).

---

### 6. **Step 5: Bring Up the Interface 🆙**

Apply the configuration and bring the interface up:

```bash
nmcli connection up <interface>
```

If the connection is already active, reload the settings:

```bash
nmcli connection reload
```

---

### 7. **Step 6: Verify the Configuration ✅**

Make sure both the primary and secondary IP addresses are correctly configured:

```bash
ip addr show <interface>
```

You should see both the primary IP (`192.168.1.100`) and the secondary IP (`192.168.1.101`) listed.

You can also verify the connection details with:

```bash
nmcli device show <interface>
```

---

### 8. **Conclusion 🎯**

You’ve now configured both a primary and a secondary static IP address using `nmcli`. This setup can be useful when a single interface needs to be accessible via multiple IP addresses.

---
