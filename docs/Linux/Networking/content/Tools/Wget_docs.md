---
title: How to Use wget to Download Files (with Advanced Configuration for Speed) in Linux
---

# **How to Use `wget` to Download Files (with Advanced Configuration for Speed) in Linux**

### **Table of Contents**

1. [Introduction](#1-introduction)
2. [Step 1: Basic Usage of `wget` 📥](#2-step-1-basic-usage-of-wget)
3. [Step 2: Resume Interrupted Downloads 🔄](#3-step-2-resume-interrupted-downloads)
4. [Step 3: Downloading Multiple Files in Bulk 📂](#4-step-3-downloading-multiple-files-in-bulk)
5. [Step 4: Speeding Up Downloads with Parallel Connections 🚀](#5-step-4-speeding-up-downloads-with-parallel-connections)
6. [Step 5: Set Up a Supercharged Alias (`sget`) ⚙️](#6-step-5-set-up-a-supercharged-alias-sget)
7. [Common `wget` Options (Cheat Sheet) 📋](#7-common-wget-options-cheat-sheet)
8. [Conclusion 🎯](#8-conclusion)

---

### 1. **Introduction**

`wget` is a powerful command-line tool in Linux used to download files from the web. It’s especially useful for automated scripts and advanced file downloads. This guide will cover basic and advanced uses of `wget`, focusing on optimizing speed.

---

### 2. **Step 1: Basic Usage of `wget` 📥**

To download a file using `wget`, the simplest command is:

```bash
wget <URL>
```

For example:

```bash
wget https://example.com/file.zip
```

This downloads the file to your current directory.

---

### 3. **Step 2: Resume Interrupted Downloads 🔄**

If your download is interrupted, you can resume it by using the `-c` (continue) option:

```bash
wget -c https://example.com/file.zip
```

This resumes the download from where it left off.

---

### 4. **Step 3: Downloading Multiple Files in Bulk 📂**

You can download multiple files at once by listing URLs in a text file and using `wget` with the `-i` option:

1. Create a text file (e.g., `urls.txt`) containing the list of URLs:

```
https://example.com/file1.zip
https://example.com/file2.zip
https://example.com/file3.zip
```

2. Download all files in the list:

```bash
wget -i urls.txt
```

---

### 5. **Step 4: Speeding Up Downloads with Parallel Connections 🚀**

`wget` doesn’t support parallel downloads natively, but you can still achieve this using the `--limit-rate`, `--no-check-certificate`, and other options:

```bash
wget -c --limit-rate=2M --no-check-certificate --tries=3 <URL>
```

This command does the following:

- `-c`: Continues from where it left off.
- `--limit-rate=2M`: Limits the download speed to 2MB/s, which can stabilize and prevent connection issues.
- `--no-check-certificate`: Skips SSL certificate verification (use with caution).
- `--tries=3`: Retries up to 3 times if the download fails.

---

### 6. **Step 5: Set Up a Supercharged Alias (`sget`) ⚙️**

To create an alias called `sget` (superget) with optimized options for faster and more resilient downloads:

1. Open your `.bashrc` or `.zshrc` file:

```bash
nano ~/.bashrc
```

2. Add the following alias:

```bash
alias sget='wget -c --limit-rate=2M --no-check-certificate --tries=5 --timeout=10 --no-clobber --random-wait --retry-connrefused'
```

Explanation of the options:

- `-c`: Resume downloads.
- `--limit-rate=2M`: Cap download speed at 2MB/s for stability.
- `--no-check-certificate`: Skip SSL checks for faster downloads (use with caution).
- `--tries=5`: Retry up to 5 times on failure.
- `--timeout=10`: Set a timeout of 10 seconds for slow responses.
- `--no-clobber`: Prevent overwriting existing files.
- `--random-wait`: Randomizes wait times between retries to avoid server overload.
- `--retry-connrefused`: Retry if the connection is refused.

3. Save and exit the editor (`Ctrl + O`, `Enter`, then `Ctrl + X`).

4. Apply the changes:

```bash
source ~/.bashrc
```

Now, you can use `sget` as a shortcut for optimized `wget` downloads:

```bash
sget https://example.com/file.zip
```

---

### 7. **Common `wget` Options (Cheat Sheet) 📋**

Here’s a table of some useful `wget` options:

| Option                   | Description                                                                     |
| ------------------------ | ------------------------------------------------------------------------------- |
| `-c, --continue`         | Resume broken downloads                                                         |
| `-i <file>`              | Download files listed in a text file                                            |
| `-b, --background`       | Run `wget` in the background                                                    |
| `--limit-rate=<rate>`    | Limit download speed (e.g., `2M` for 2MB/s)                                     |
| `--no-check-certificate` | Skip SSL certificate verification (use cautiously)                              |
| `--tries=<number>`       | Set the number of retries for failed downloads                                  |
| `--timeout=<seconds>`    | Set a timeout period for unresponsive servers                                   |
| `--no-clobber`           | Avoid overwriting files                                                         |
| `--random-wait`          | Randomize wait times between requests to avoid overloading servers              |
| `--retry-connrefused`    | Retry downloads if the connection is refused                                    |
| `-r, --recursive`        | Download websites or directories recursively                                    |
| `--mirror`               | Create a mirror of a website (includes `-r`, `-N`, `-l inf`, and `-np` options) |

---

### 8. **Conclusion 🎯**

`wget` is a versatile tool for downloading files in Linux. With a few tweaks, like the `sget` alias, you can optimize download speed and reliability. Whether you’re downloading single files or managing bulk downloads, `wget` can handle it all with ease.

---
