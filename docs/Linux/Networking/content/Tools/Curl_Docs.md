---
title: How to Use curl to Download Files (with Advanced Configuration for Speed) in Linux
---

## **How to Use `curl` to Download Files (with Advanced Configuration for Speed) in Linux**

### **Table of Contents**

1. [Introduction](#1-introduction)
2. [Step 1: Basic Usage of `curl` 📥](#2-step-1-basic-usage-of-curl)
3. [Step 2: Resume Interrupted Downloads 🔄](#3-step-2-resume-interrupted-downloads)
4. [Step 3: Downloading Multiple Files in Bulk 📂](#4-step-3-downloading-multiple-files-in-bulk)
5. [Step 4: Speeding Up Downloads with Parallel Requests 🚀](#5-step-4-speeding-up-downloads-with-parallel-requests)
6. [Step 5: Set Up a Supercharged Alias (`surl`) ⚙️](#6-step-5-set-up-a-supercharged-alias-surl)
7. [Common `curl` Options (Cheat Sheet) 📋](#7-common-curl-options-cheat-sheet)
8. [Conclusion 🎯](#8-conclusion)

---

### 1. **Introduction**

`curl` is a versatile command-line tool for transferring data with URLs. It's not just for downloading files but can be used for various protocols. This tutorial will cover basic and advanced uses of `curl`, focusing on optimizing download speed.

---

### 2. **Step 1: Basic Usage of `curl` 📥**

To download a file using `curl`, use the `-O` option to save the file with its original name:

```bash
curl -O <URL>
```

For example:

```bash
curl -O https://example.com/file.zip
```

This command downloads `file.zip` to your current directory.

---

### 3. **Step 2: Resume Interrupted Downloads 🔄**

To resume an interrupted download, use the `-C -` option:

```bash
curl -C - -O <URL>
```

This will continue downloading the file from where it left off.

---

### 4. **Step 3: Downloading Multiple Files in Bulk 📂**

You can download multiple files by using a loop with a list of URLs:

1. Create a text file (e.g., `urls.txt`) containing the URLs:

```
https://example.com/file1.zip
https://example.com/file2.zip
https://example.com/file3.zip
```

2. Use a loop to download each file:

```bash
while IFS= read -r url; do
    curl -O "$url"
done < urls.txt
```

---

### 5. **Step 4: Speeding Up Downloads with Parallel Requests 🚀**

`curl` doesn’t natively support parallel downloads, but you can use tools like `xargs` to achieve this:

```bash
cat urls.txt | xargs -n 1 -P 4 curl -O
```

Explanation:

- `-n 1`: Processes one URL at a time.
- `-P 4`: Runs up to 4 parallel downloads.

---

### 6. **Step 5: Set Up a Supercharged Alias (`surl`) ⚙️**

To create an alias called `surl` with optimized options for faster and more resilient downloads:

1. Open your `.bashrc` or `.zshrc` file:

```bash
nano ~/.bashrc
```

2. Add the following alias:

```bash
alias surl='curl -O -C - --retry 5 --retry-delay 2 --max-time 120 --fail --silent --show-error'
```

Explanation of the options:

- `-O`: Save the file with its original name.
- `-C -`: Resume downloads.
- `--retry 5`: Retry up to 5 times on failure.
- `--retry-delay 2`: Wait 2 seconds between retries.
- `--max-time 120`: Set a maximum time of 120 seconds for the entire operation.
- `--fail`: Fail silently on server errors.
- `--silent`: Suppress progress output.
- `--show-error`: Show error messages if they occur.

3. Save and exit the editor (`Ctrl + O`, `Enter`, then `Ctrl + X`).

4. Apply the changes:

```bash
source ~/.bashrc
```

Now you can use `surl` for optimized `curl` downloads:

```bash
surl https://example.com/file.zip
```

---

### 7. **Common `curl` Options (Cheat Sheet) 📋**

Here’s a table of useful `curl` options:

| Option                    | Description                                            |
| ------------------------- | ------------------------------------------------------ |
| `-O`                      | Save the file with its original name                   |
| `-C -`                    | Resume a partially downloaded file                     |
| `-L`                      | Follow redirects                                       |
| `-s`                      | Silent mode (no progress or error messages)            |
| `-S`                      | Show errors (use with `-s`)                            |
| `--retry <num>`           | Number of times to retry on failure                    |
| `--retry-delay <seconds>` | Delay between retries (in seconds)                     |
| `--max-time <seconds>`    | Maximum time allowed for the operation                 |
| `--fail`                  | Fail silently on server errors (no output)             |
| `-x <proxy>`              | Use the specified proxy                                |
| `-u <user:password>`      | Use the specified user and password for authentication |
| `--header <header>`       | Add a custom header to the request                     |

---

### 8. **Conclusion 🎯**

`curl` is a powerful tool for downloading files with various options to handle interruptions and optimize performance. With the `surl` alias, you can streamline and enhance your downloading process for greater efficiency.

---
