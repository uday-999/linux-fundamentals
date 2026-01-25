# Day 05 - File Ownership (chown / chgrp) 👤👥🐧

Today I learned how Linux file ownership works and how to manage file permissions using **chown** and **chgrp** commands - essential skills for server administration (especially on AWS EC2 instances).

---

## ✅ 1) Understanding Linux File Ownership

Every file and directory in Linux has three key attributes:

- **Owner (User)** - The user who created/owns the file
- **Group** - A collection of users sharing access permissions
- **Others** - Everyone else on the system

You can view ownership details using:

```bash
ls -l
```

Example output with explanation:
```
-rwxr-xr-- 1 alice developers 4096 Jan 22 14:30 script.sh
↑          ↑   ↑      ↑              ↑        ↑
│          │   Owner  Group          Time     Filename
│          │   (User)                Stamp
│          │
│          Link Count
Permissions
```

**Key Fields:**
- `alice` = Owner (User)
- `developers` = Group
- `-rwxr-xr--` = Permissions (user:rwx, group:r-x, others:r--)

---

## ✅ 2) Why Ownership Matters in Real-World Scenarios

| Scenario | Why Ownership Matters |
|----------|----------------------|
| **Web Server** | Apache/Nginx needs `www-data` group access to serve files |
| **Multi-user Server** | Different teams (devs, ops) need controlled file access |
| **Security** | Prevent unauthorized users from reading sensitive configs |
| **AWS EC2** | EC2 instances often run services under specific users (like `ec2-user`) |

---

## ✅ 3) Change File Owner with `chown`

### 🔹 Basic Syntax
```bash
sudo chown [new-owner] [file]
```

### 🔹 Practical Examples
```bash
# Change owner of a single file
sudo chown root /etc/config.conf

# Change owner of multiple files
sudo chown ec2-user file1.txt file2.txt

# Change owner of a directory (but not its contents)
sudo chown alice /var/www/html/
```

---

## ✅ 4) Change Group Ownership with `chgrp`

### 🔹 Basic Syntax
```bash
sudo chgrp [new-group] [file]
```

### 🔹 Practical Examples
```bash
# Change group of a file
sudo chgrp developers app.js

# Change group of a directory
sudo chgrp www-data /var/www/

# Verify the change
ls -l app.js
```

---

## ✅ 5) Change Owner AND Group Simultaneously

**Two equivalent methods:**

**Method 1:** Using `chown` with colon notation
```bash
# Format: user:group
sudo chown ec2-user:developers deploy.sh

# Just change group (keep current owner)
sudo chown :developers deploy.sh

# Just change owner (keep current group)
sudo chown alice: deploy.sh
```

**Method 2:** Using `chgrp` after `chown`
```bash
sudo chown alice deploy.sh
sudo chgrp developers deploy.sh
```

---

## ✅ 6) Recursive Ownership Changes (`-R` Flag)

**⚠️ Important:** Use carefully! Affects ALL files and subdirectories.

```bash
# Change owner recursively
sudo chown -R ec2-user /home/project/

# Change both owner and group recursively
sudo chown -R alice:developers /home/project/

# What NOT to do (unless you're sure!)
sudo chown -R root:root /home/  # Could break user directories!
```

---

## ✅ 7) Special Cases & Advanced Examples

### 🔹 Copying Ownership from Another File
```bash
# Copy ownership from reference.txt to target.txt
sudo chown --reference=reference.txt target.txt
```

### 🔹 Using Numeric UID/GID
```bash
# Find user/group IDs
id alice  # Shows uid=1001, gid=1001

# Use numeric IDs instead of names
sudo chown 1001:1001 file.txt
```

### 🔹 Symbolic Link Handling
```bash
# Affects the link itself, not the target
sudo chown alice symlink

# Affects the target file, not the link
sudo chown -h alice symlink
```

---

## ✅ 8) Amazon Linux / EC2 Practical Scenarios

### Scenario 1: Web Server Setup
```bash
# Grant Apache (httpd) access to web files
sudo chown -R ec2-user:apache /var/www/html/
sudo chmod 750 /var/www/html/  # Owner: rwx, Group: r-x, Others: ---
```

### Scenario 2: Shared Project Folder
```bash
# Create shared directory for dev team
sudo mkdir /opt/shared-project
sudo chown alice:developers /opt/shared-project
sudo chmod 2775 /opt/shared-project  # Sets sticky bit for group inheritance
```

### Scenario 3: Fixing Permission Issues
```bash
# Common fix for "Permission denied" errors
sudo chown -R ec2-user:ec2-user ~/my-app/
```

---

## 🎯 Hands-On Lab Exercise

### Task: Create a Secure Project Structure
```bash
# 1. Create project structure
mkdir -p ~/project/{src,logs,config}
touch ~/project/src/app.py ~/project/logs/app.log ~/project/config/settings.conf

# 2. Create a new group
sudo groupadd project-team

# 3. Set different ownerships
sudo chown ec2-user:project-team ~/project/src/app.py
sudo chown ec2-user:admins ~/project/logs/
sudo chown root:root ~/project/config/settings.conf

# 4. Verify with detailed listing
ls -la ~/project/

# 5. Try accessing as different user (optional)
sudo -u otheruser cat ~/project/config/settings.conf  # Should fail
```

---

## 📋 Common Commands Cheat Sheet

| Command | Description |
|---------|-------------|
| `ls -l` | View ownership and permissions |
| `stat file` | Detailed file info including UID/GID |
| `id username` | Show user's UID and group memberships |
| `groups username` | Show user's groups |
| `sudo chown user file` | Change file owner |
| `sudo chown user:group file` | Change both owner and group |
| `sudo chown -R user:group dir/` | Recursive ownership change |
| `sudo chgrp group file` | Change group only |
| `chown --reference=ref file` | Copy ownership from reference file |

---

## ⚠️ Important Security Considerations

1. **Don't use `chown -R root:root`** on user directories
2. **Web files** should have restrictive permissions (e.g., `640` for configs)
3. **Log files** often need specific groups (like `adm` or `syslog`)
4. **Home directories** should be owned by the user, not root
5. **Use `setfacl`** for complex permission scenarios (beyond chown/chgrp)

---

## ✅ Summary

✅ **Ownership Model:** User-Group-Others triad
✅ **View Ownership:** `ls -l`, `stat`, `id` commands
✅ **Change Owner:** `chown` (supports `user:group` format)
✅ **Change Group:** `chgrp` or `chown :group`
✅ **Recursive Changes:** `-R` flag (use carefully!)
✅ **Real-world Use:** Web servers, shared projects, security hardening

📌 **Next Day:** User & Group Management (`useradd`, `groupadd`, `usermod`, `passwd`) 🔥

---

## 💡 Pro Tips

1. **Test first:** Use `-v` (verbose) flag to see what `chown` will change
2. **Dry run:** Some systems support `-n` or `--dry-run` for safety
3. **Document:** Keep notes of ownership changes in production
4. **Backup:** Backup important files before bulk permission changes

**Remember:** With great power (sudo chown) comes great responsibility! 🕷️
