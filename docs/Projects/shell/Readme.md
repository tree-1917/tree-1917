---
title: camelFaster 
---

# 🐫 camelFaster

**camelFaster** is a simple Bash script that allows you to limit the network bandwidth for specific processes by using their PID (Process ID). This is useful for throttling the network usage of individual programs, such as browsers or background services.

## 🚀 Features
- Apply network bandwidth limits to specific processes.
- Easy-to-use with customizable bandwidth limits.
- Works with `tc` (traffic control) and `iptables`.
- Automatically installs required tools if missing (`tc` and `iptables`).

## ⚙️ Requirements
- `tc` (part of `iproute2`).
- `iptables`.

If these tools are not already installed, the script will handle installation automatically.

## 📦 Installation

### Step 1: Clone the Repository
To get started, first clone the repository from GitHub:
```bash
git clone https://github.com/yourusername/camelFaster.git
cd camelFaster
```

### Step 2: Make the Script Executable
Ensure that the script is executable:
```bash
chmod +x camelFaster.sh
```

### Step 3: Move the Script to `/usr/local/bin`
For global usage (so you can run the script from anywhere without needing `./`), move it to `/usr/local/bin`:
```bash
sudo mv camelFaster.sh /usr/local/bin/camelFaster
```

Now you can run `camelFaster` directly from anywhere in your system!

### Step 4: Install Requirements
The script automatically checks if `tc` and `iptables` are installed. If they are missing, it will install them for you. If you need to install them manually, you can run:
```bash
sudo apt update
sudo apt install -y iproute2 iptables
```

## 🛠️ Usage

To run the script, use the following format:
```bash
sudo camelFaster [limit] [PID] [interface]
```

- **`limit`**: The desired bandwidth limit (e.g., `80kbps`, `1mbps`).
- **`PID`**: The Process ID of the application you want to limit (you can find the PID using `ps` or `top`).
- **`interface`**: Your network interface (e.g., `wlan0`, `eth0`).

### Example:

```bash
sudo camelFaster 80kbps 3003 wlan0
```

In this example:
- A bandwidth limit of **80kbps** is applied to the process with PID **3003**.
- The limit is applied to the **wlan0** network interface.

## 📤 Removing the Bandwidth Limit
The script automatically removes the bandwidth limit when it finishes or is interrupted. However, if you need to manually remove the limit, you can do so by stopping the script or rebooting the system.

## 📝 Notes
- The script uses `iptables` to mark packets belonging to the process, and `tc` (traffic control) to apply the bandwidth limit to those marked packets.
- **Root privileges** are required to run this script due to the need for modifying network settings.

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

