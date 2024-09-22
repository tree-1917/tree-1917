# Comprehensive Guide to System Performance Tuning and Process Management

## Table of Contents 📚

1. [What Does "Tune System Performance" Mean?](#what-does-tune-system-performance-mean)
2. [Optimizing System Performance with **tuned** Daemon](#optimizing-system-performance-with-tuned-daemon)
3. [Understanding **nice** and **renice** Commands](#understanding-nice-and-renice-commands)
4. [What is **tuned**?](#what-is-tuned)
5. [Common **tuned** Profiles](#common-tuned-profiles)
6. [Installing and Configuring **tuned**](#installing-and-configuring-tuned)
7. [**tuned** Usage Script](#tuned-usage-script)
8. [**nice** and **renice** Commands Explained](#nice-and-renice-commands-explained)
9. [Prioritizing Processes with **nice**](#prioritizing-processes-with-nice)
10. [**nice** Use Case Script](#nice-use-case-script)
11. [Tables of Common Options for **tuned** and **nice**](#tables-of-common-options-for-tuned-and-nice)
12. [Use Cases for **tuned** and **nice**](#use-cases-for-tuned-and-nice)

---

## What Does "Tune System Performance" Mean? 🔧

**Tuning system performance** refers to the process of optimizing various system parameters to improve the performance of a computer system. This process involves:

- **Adjusting hardware settings**: Modifying system BIOS settings or hardware configurations.
- **Optimizing software settings**: Tweaking system and application settings for improved performance.
- **Monitoring performance**: Using tools to track and analyze system performance metrics.

Effective tuning helps in achieving a balance between system speed, responsiveness, and resource usage, tailored to specific workloads or applications.

## Optimizing System Performance with **tuned** Daemon ⚙️

**Optimizing system performance** with **tuned** involves using predefined profiles to adjust system settings automatically based on different usage scenarios. This ensures that the system operates efficiently according to the current workload.

### What is **tuned**?

**Tuned** is a Linux daemon designed to dynamically adjust system settings to optimize performance. It applies predefined profiles to the system to optimize various parameters such as CPU frequency, I/O scheduler, and power management.

### Common **tuned** Profiles 🛠️

Here is a table listing some common **tuned** profiles:

| **Profile**              | **Description**                                         |
| ------------------------ | ------------------------------------------------------- |
| `balanced`               | Balances performance and power consumption.             |
| `high-performance`       | Maximizes performance at the expense of power usage.    |
| `power-saving`           | Reduces power consumption to extend battery life.       |
| `throughput-performance` | Optimizes for high data throughput and performance.     |
| `virtual-guest`          | Optimizes for virtualized environments.                 |
| `network-latency`        | Minimizes network latency for real-time applications.   |
| `virtual-host`           | Optimizes for performance in virtual host environments. |

### Installing and Configuring **tuned**

**Installation:**

To install **tuned**, use the following command:

```bash
sudo apt-get update
sudo apt-get install tuned
```

**Configuration:**

1. **List Available Profiles:**

   ```bash
   tuned-adm list
   ```

2. **Apply a Profile:**

   ```bash
   sudo tuned-adm profile <profile-name>
   ```

   Replace `<profile-name>` with the profile you wish to apply.

3. **Check Current Profile:**

   ```bash
   tuned-adm active
   ```

4. **View Profile Details:**

   ```bash
   tuned-adm profile <profile-name> --details
   ```

### **tuned** Usage Script

Here’s a script to automate **tuned** profile management:

```bash
#!/bin/bash

# List available profiles
echo "Available tuned profiles:"
tuned-adm list

# Apply a specific profile
PROFILE="balanced"
echo "Applying profile: $PROFILE"
sudo tuned-adm profile $PROFILE

# Display the active profile
echo "Current active profile:"
tuned-adm active

# Display details of the applied profile
echo "Profile details:"
tuned-adm profile $PROFILE --details
```

## Understanding **nice** and **renice** Commands ⏫

The **nice** and **renice** commands are used to adjust the priority of processes, which can influence their CPU time allocation.

### What are **nice** and **renice**?

- **nice**: Starts a new process with a specified priority.
- **renice**: Changes the priority of an already running process.

The priority range for **nice** is from `-20` (highest priority) to `19` (lowest priority). Processes inherit their nice value from their parent process, which is usually `0` by default.

### Prioritizing Processes with **nice**

- **`-20`**: Highest priority.
- **`19`**: Lowest priority.
- **Default**: `0`.

### Nice Levels vs. PRI

- **Nice Level Priority**: A user-space value, affecting the process's CPU scheduling priority, ranges from `-20` to `19`.
- **PRI (Kernel Priority)**: The actual priority used by the Linux kernel for process scheduling, ranges from `0` (highest) to `139` (lowest) for real-time processes, and `100` to `139` for user processes.

### **nice** Use Case Script

Here’s a script to manage process priorities using **nice**:

```bash
#!/bin/bash

# Start a high-priority process
echo "Starting a high-priority process..."
nice -n -10 your_command_here &

# Start a low-priority process
echo "Starting a low-priority process..."
nice -n 10 your_command_here &
```

## Tables of Common Options for **tuned** and **nice** 📊

### Common Options for **tuned**

| **Command**                          | **Description**                      |
| ------------------------------------ | ------------------------------------ |
| `tuned-adm list`                     | List available profiles.             |
| `tuned-adm profile <name>`           | Apply a specific profile.            |
| `tuned-adm active`                   | Show the current active profile.     |
| `tuned-adm profile <name> --details` | Show details of the applied profile. |

### Common Options for **nice**

| **Option**                   | **Description**                                 |
| ---------------------------- | ----------------------------------------------- |
| `-n <value>`                 | Set the priority value for the process.         |
| `<command>`                  | Command to be executed with specified priority. |
| `renice -n <value> -p <pid>` | Change the priority of an existing process.     |

## Use Cases for **tuned** and **nice** 🛠️

### Use Cases for **tuned**

1. **Server Optimization**: Apply the `high-performance` profile on servers to maximize performance for critical applications.
2. **Power Management**: Use the `power-saving` profile on laptops to extend battery life.
3. **Virtualization**: Apply the `virtual-guest` profile in virtualized environments to optimize performance.

### Use Cases for **nice**

1. **High-Priority Tasks**: Use **nice** to start a crucial process with higher priority to ensure it gets more CPU time.
2. **Low-Priority Background Tasks**: Use **nice** to start background processes with lower priority to minimize their impact on interactive tasks.
3. **Adjusting Running Processes**: Use **renice** to adjust the priority of running processes to manage system load dynamically.

---
