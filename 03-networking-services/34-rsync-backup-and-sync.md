# 🚀 Day 34 – rsync (Backup & Synchronization Tool)

## 📌 Overview

Today I explored `rsync`, a powerful and efficient file synchronization and backup tool used widely in Linux system administration and DevOps environments.

`rsync` is commonly used for:
- Automated backups
- Server synchronization
- Deployment processes
- Remote file transfers

---

# 🔄 1️⃣ What is rsync?

rsync (Remote Sync) is a fast and versatile file copying tool.

Unlike normal copy commands, rsync:
- Transfers only changed portions of files
- Saves bandwidth
- Preserves permissions and ownership
- Works locally and remotely

---

# 📂 2️⃣ Basic Local File Synchronization

Syntax:

```bash
rsync source destination

Example:

rsync file.txt /backup/

This copies file.txt to the /backup/ directory.


---

📁 3️⃣ Synchronize an Entire Directory

rsync -av folder/ /backup/folder/

Common Options:

-a → Archive mode (preserves permissions, timestamps, ownership)

-v → Verbose (shows progress)



---

🌐 4️⃣ Remote Synchronization Using SSH

rsync can transfer files securely using SSH.

rsync -av folder username@server_ip:/destination/path

Example:

rsync -av project/ uday@192.168.1.20:/home/uday/


---

🔐 5️⃣ rsync with Explicit SSH Option

rsync -av -e ssh folder username@server_ip:/destination/path

If SSH key-based authentication is configured, the transfer will be passwordless.


---

🗑 6️⃣ Delete Extra Files in Destination

To keep both directories identical:

rsync -av --delete source/ destination/

The --delete option removes files in the destination that no longer exist in the source.


---

📊 7️⃣ Dry Run (Simulation Mode)

Before executing, you can test the command:

rsync -av --dry-run source/ destination/

This shows what would happen without making changes.


---

🧠 What I Learned

How rsync efficiently transfers data

Local and remote synchronization

Secure transfer using SSH

Maintaining identical backup directories

Using dry-run for safe execution



---

🔥 Why rsync is Important

Industry-standard backup tool

Used in production environments

Reduces bandwidth usage

Supports automation and cron jobs

Essential for DevOps workflows
