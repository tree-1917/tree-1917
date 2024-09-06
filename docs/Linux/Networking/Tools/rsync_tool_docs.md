---
title: 📂 Rsync Backup Tutorial 
---

# 📂 **Rsync Backup Tutorial: Types and Scripts**

Welcome to this tutorial on using **`rsync`** for different types of backups. We’ll cover how to create full, incremental, differential, mirror, snapshot, and incremental forever backups using `rsync`. Plus, we’ll provide options for both local and remote backups. 🚀

## 📑 **Table of Contents**

1. [🔍 What is **rsync**?](#-what-is-rsync)
2. [🗂️ Types of Backup](#-types-of-backup)
3. [🛠️ Creating Backups with **rsync**](#-creating-backups-with-rsync)
   - [1. Full Backup](#1-full-backup)
   - [2. Incremental Backup](#2-incremental-backup)
   - [3. Differential Backup](#3-differential-backup)
   - [4. Mirror Backup](#4-mirror-backup)
   - [5. Snapshot Backup](#5-snapshot-backup)
   - [6. Incremental Forever Backup](#6-incremental-forever-backup)
4. [📦 How to Archive and Compress Data Before **rsync**](#-how-to-archive-and-compress-data-before-rsync)

---

## 🔍 **What is `rsync`?**

**`rsync`** is a powerful command-line tool for syncing files and directories between two locations. It’s highly efficient and flexible, ideal for backups and mirroring. Key features include:

- **Delta Transfers** 🌐: Only changes are transferred.
- **Compression** 💨: Reduces data transfer size.
- **Incremental Transfers** 🔄: Syncs only changed files.
- **Versatility** 📁: Use locally or remotely.

---

## 🗂️ **Types of Backup**

1. **Full Backup** 📦: All data from the source is copied.
2. **Incremental Backup** ⏳: Only changes since the last backup are copied.
3. **Differential Backup** 📈: Changes since the last full backup are copied.
4. **Mirror Backup** 🪞: Exact mirror of the source directory.
5. **Snapshot Backup** 📸: State of data at a specific point in time.
6. **Incremental Forever Backup** ♾️: Continuous incremental backups after the initial full backup.

---

## 🛠️ **Creating Backups with `rsync`**

### 1. **Full Backup** 📦

A full backup copies all data from the source to the backup destination.

**Script Example:**

```bash
#!/bin/bash
# Full backup with rsync

SOURCE="/path/to/source/"

# Uncomment the following line for a local backup
# rsync -avz --progress $SOURCE /path/to/backup/

# Uncomment the following lines for a remote backup
REMOTE_USER="user"
REMOTE_HOST="remote_server"
DESTINATION="/path/to/backup/"
rsync -avz --progress $SOURCE $REMOTE_USER@$REMOTE_HOST:$DESTINATION

# Uncomment the following line to archive and compress before syncing
# tar -cvzf /tmp/backup.tar.gz $SOURCE

# Uncomment the following line to compress the archive after syncing
# gzip /tmp/backup.tar.gz
```

### 2. **Incremental Backup** ⏳

An incremental backup only copies files that have changed since the last backup.

**Script Example:**

```bash
#!/bin/bash
# Incremental backup with rsync

SOURCE="/path/to/source/"

# Uncomment the following line for a local backup
# rsync -avz --progress --link-dest=/path/to/backup/incremental/ $SOURCE /path/to/backup/

# Uncomment the following lines for a remote backup
REMOTE_USER="user"
REMOTE_HOST="remote_server"
DESTINATION="/path/to/backup/"
INCREMENTAL_DIR="/path/to/backup/incremental/"
rsync -avz --progress --link-dest=$REMOTE_USER@$REMOTE_HOST:$INCREMENTAL_DIR $SOURCE $REMOTE_USER@$REMOTE_HOST:$DESTINATION

# Uncomment the following line to archive and compress before syncing
# tar -cvzf /tmp/incremental.tar.gz $SOURCE

# Uncomment the following line to compress the archive after syncing
# gzip /tmp/incremental.tar.gz
```

### 3. **Differential Backup** 📈

A differential backup copies files that have changed since the last full backup.

**Script Example:**

```bash
#!/bin/bash
# Differential backup with rsync

SOURCE="/path/to/source/"

# Uncomment the following line for a local backup
# rsync -avz --progress --compare-dest=/path/to/backup/full/ $SOURCE /path/to/backup/

# Uncomment the following lines for a remote backup
REMOTE_USER="user"
REMOTE_HOST="remote_server"
DESTINATION="/path/to/backup/"
FULL_BACKUP="/path/to/backup/full/"
rsync -avz --progress --compare-dest=$REMOTE_USER@$REMOTE_HOST:$FULL_BACKUP $SOURCE $REMOTE_USER@$REMOTE_HOST:$DESTINATION

# Uncomment the following line to archive and compress before syncing
# tar -cvzf /tmp/differential.tar.gz $SOURCE

# Uncomment the following line to compress the archive after syncing
# gzip /tmp/differential.tar.gz
```

### 4. **Mirror Backup** 🪞

A mirror backup creates an exact copy of the source directory, including deletions.

**Script Example:**

```bash
#!/bin/bash
# Mirror backup with rsync

SOURCE="/path/to/source/"

# Uncomment the following line for a local backup
# rsync -avz --delete --progress $SOURCE /path/to/backup/

# Uncomment the following lines for a remote backup
REMOTE_USER="user"
REMOTE_HOST="remote_server"
DESTINATION="/path/to/backup/"
rsync -avz --delete --progress $SOURCE $REMOTE_USER@$REMOTE_HOST:$DESTINATION

# Uncomment the following line to archive and compress before syncing
# tar -cvzf /tmp/mirror.tar.gz $SOURCE

# Uncomment the following line to compress the archive after syncing
# gzip /tmp/mirror.tar.gz
```

### 5. **Snapshot Backup** 📸

A snapshot backup captures the state of the data at a specific point in time using hard links for unchanged files.

**Script Example:**

```bash
#!/bin/bash
# Snapshot backup with rsync

SOURCE="/path/to/source/"
SNAPSHOT_NAME="backup-$(date +%Y%m%d-%H%M%S)"
LATEST="/path/to/backup_root/latest"

# Uncomment the following line for a local backup
# rsync -avz --link-dest=$LATEST $SOURCE /path/to/backup_root/$SNAPSHOT_NAME

# Uncomment the following lines for a remote backup
REMOTE_USER="user"
REMOTE_HOST="remote_server"
BACKUP_ROOT="/path/to/backup_root/"
ssh $REMOTE_USER@$REMOTE_HOST "mkdir -p $BACKUP_ROOT/$SNAPSHOT_NAME"
rsync -avz --link-dest=$REMOTE_USER@$REMOTE_HOST:$LATEST $SOURCE $REMOTE_USER@$REMOTE_HOST:$BACKUP_ROOT/$SNAPSHOT_NAME
ssh $REMOTE_USER@$REMOTE_HOST "rm -f $LATEST; ln -s $BACKUP_ROOT/$SNAPSHOT_NAME $LATEST"

# Uncomment the following line to archive and compress before syncing
# tar -cvzf /tmp/snapshot.tar.gz $SOURCE

# Uncomment the following line to compress the archive after syncing
# gzip /tmp/snapshot.tar.gz
```

### 6. **Incremental Forever Backup** ♾️

An incremental forever backup involves an initial full backup followed by incremental backups, maintaining a chain of changes.

**Script Example:**

```bash
#!/bin/bash
# Incremental forever backup with rsync

SOURCE="/path/to/source/"

# Uncomment the following line for a local backup
# rsync -avz --link-dest=/path/to/backup/incremental/ $SOURCE /path/to/backup/backup-$(date +%Y%m%d-%H%M%S)/

# Uncomment the following lines for a remote backup
REMOTE_USER="user"
REMOTE_HOST="remote_server"
DESTINATION="/path/to/backup/"
INCREMENTAL_DIR="/path/to/backup/incremental/"
rsync -avz --link-dest=$REMOTE_USER@$REMOTE_HOST:$INCREMENTAL_DIR $SOURCE $REMOTE_USER@$REMOTE_HOST:$DESTINATION/backup-$(date +%Y%m%d-%H%M%S)/

# Uncomment the following line to archive and compress before syncing
# tar -cvzf /tmp/incremental_forever.tar.gz $SOURCE

# Uncomment the following line to compress the archive after syncing
# gzip /tmp/incremental_forever.tar.gz
```

---

## 📦 **How to Archive and Compress Data Before `rsync`**

To archive and compress data before syncing with `rsync`, you can use `tar` and `gzip`. Uncomment the relevant lines in the scripts if you want to archive and compress before syncing.

### **Archiving and Compressing Example:**

```bash
# Create a tar archive
tar -cvzf /tmp/backup.tar.gz $SOURCE

# Compress the tar archive
gzip /tmp/backup.tar.gz
```

This tutorial now offers flexibility for both local and remote backups, with clear instructions on how to adjust the scripts accordingly. Happy backing up! 🚀📂

---
