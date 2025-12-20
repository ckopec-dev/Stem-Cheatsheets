
# 📁 rsync Tutorial — The Essential File Sync Tool

`rsync` is a fast, reliable command-line tool for **copying, syncing, and backing up files** locally or over a network. It only transfers **differences**, making it far more efficient than `cp` or `scp`.

---

## 1️⃣ What rsync Is (and Why It’s Powerful)

### Key features

* 🔁 Incremental transfers (only changed data)
* 🧠 Intelligent delta algorithm
* 🔐 Secure transfers over SSH
* 🧹 Can delete files to mirror directories
* 🧾 Preserves permissions, ownership, timestamps
* 📊 Detailed progress output

---

## 2️⃣ Installing rsync

### Linux

```bash
sudo apt install rsync        # Debian / Ubuntu
sudo dnf install rsync        # Fedora
sudo pacman -S rsync          # Arch
```

### macOS

```bash
brew install rsync
```

Check installation:

```bash
rsync --version
```

---

## 3️⃣ Basic rsync Syntax

```bash
rsync [options] source destination
```

Examples:

```bash
rsync file.txt /backup/
rsync /data/ /backup/data/
```

⚠️ **Trailing slashes matter**

* `/data/` → copies *contents*
* `/data` → copies the *directory itself*

---

## 4️⃣ Your First Useful Command

```bash
rsync -av source/ destination/
```

### What this does

| Flag | Meaning                                          |
| ---- | ------------------------------------------------ |
| `-a` | Archive mode (permissions, symlinks, timestamps) |
| `-v` | Verbose output                                   |

This is the **default recommended starting point**.

---

## 5️⃣ Seeing What rsync Will Do (Dry Run)

```bash
rsync -av --dry-run source/ destination/
```

💡 Always use `--dry-run` before destructive operations.

---

## 6️⃣ Copying with Progress

```bash
rsync -av --progress source/ destination/
```

For large files:

```bash
rsync -av --info=progress2 source/ destination/
```

---

## 7️⃣ Syncing Directories (Mirror Mode)

To make destination **exactly match** source:

```bash
rsync -av --delete source/ destination/
```

⚠️ This **deletes files** in destination that don’t exist in source.

Safe check first:

```bash
rsync -av --delete --dry-run source/ destination/
```

---

## 8️⃣ Using rsync Over SSH (Remote Transfers)

### Copy local → remote

```bash
rsync -av source/ user@server:/path/to/destination/
```

### Copy remote → local

```bash
rsync -av user@server:/path/to/source/ destination/
```

### Specify SSH port

```bash
rsync -av -e "ssh -p 2222" source/ user@server:/backup/
```

---

## 9️⃣ Excluding Files and Directories

### Exclude a directory

```bash
rsync -av --exclude node_modules source/ destination/
```

### Exclude multiple patterns

```bash
rsync -av \
  --exclude "*.log" \
  --exclude ".git/" \
  source/ destination/
```

### Exclude from file

```bash
rsync -av --exclude-from=exclude.txt source/ destination/
```

**exclude.txt**

```
node_modules/
*.log
.cache/
```

---

## 🔟 Preserving Ownership & Permissions (Backups)

```bash
sudo rsync -aAXv source/ destination/
```

| Flag | Purpose                      |
| ---- | ---------------------------- |
| `-A` | Preserve ACLs                |
| `-X` | Preserve extended attributes |

Used for **system backups**.

---

## 1️⃣1️⃣ Compressing Data During Transfer

Great for slow networks:

```bash
rsync -avz source/ user@server:/backup/
```

`-z` = compression

---

## 1️⃣2️⃣ Limiting Bandwidth

```bash
rsync -av --bwlimit=5000 source/ destination/
```

Limits to ~5 MB/s.

---

## 1️⃣3️⃣ Incremental Backups (Snapshots)

```bash
rsync -av --link-dest=/backup/latest source/ /backup/$(date +%F)/
```

Creates **hard-linked snapshots** that save space.

Common layout:

```
/backup/
 ├── 2025-01-01/
 ├── 2025-01-02/
 └── latest -> 2025-01-02/
```

---

## 1️⃣4️⃣ Resuming Interrupted Transfers

```bash
rsync -av --partial --progress source/ destination/
```

For large files:

```bash
rsync -av --append-verify source/ destination/
```

---

## 1️⃣5️⃣ Deleting Files Older Than X Days

```bash
rsync -av --delete --ignore-existing source/ destination/
```

Or combine with `find`:

```bash
find /backup -type f -mtime +30 -delete
```

---

## 1️⃣6️⃣ Common rsync Recipes

### Backup home directory

```bash
rsync -av --exclude=".cache" ~/ /mnt/backup/home/
```

### Sync website to server

```bash
rsync -avz --delete ./site/ user@server:/var/www/html/
```

### Clone a directory tree

```bash
rsync -a source/ destination/
```

---

## 1️⃣7️⃣ Understanding rsync Exit Codes

| Code | Meaning             |
| ---- | ------------------- |
| 0    | Success             |
| 23   | Partial transfer    |
| 24   | Some files vanished |
| 12   | Protocol error      |

Check after scripts:

```bash
echo $?
```

---

## 1️⃣8️⃣ rsync in Automation (Cron Example)

```bash
crontab -e
```

```bash
0 2 * * * rsync -av --delete /data/ /backup/data/
```

Runs nightly at 2 AM.

---

## 1️⃣9️⃣ When NOT to Use rsync

❌ Real-time sync → use `syncthing`
❌ Versioned backups → use `borg`, `restic`
❌ Cloud sync → use `rclone`

---

## 2️⃣0️⃣ Best Practices Summary

✔ Always use `--dry-run` first
✔ Be careful with `--delete`
✔ Use trailing slashes correctly
✔ Use SSH for remote syncs
✔ Combine with cron for automation

---

## 📌 Quick Cheat Sheet

```bash
rsync -av source/ dest/             # Basic copy
rsync -av --delete source/ dest/    # Mirror
rsync -avz source/ user@host:/path/ # Remote
rsync -av --exclude "*.log" src/ dst/
rsync -av --dry-run src/ dst/
```

---

