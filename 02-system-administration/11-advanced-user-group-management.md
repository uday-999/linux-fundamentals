# 🐧 Day 11 - Advanced User & Group Management: The SysAdmin's Toolkit

> Transitioning from Linux fundamentals to real System Administration with enterprise-grade user management techniques.

User and group management is one of the **core responsibilities of a Linux System Administrator**.  
Today we go deeper than basic `useradd` — we understand how Linux stores and manages user data internally, and learn professional administration patterns.

---

## 🎯 Why This Matters

In real-world environments:
- **Multiple developers** access the same server with different privilege levels
- **Services** run under dedicated system accounts for security isolation
- **Access control** must be structured using groups for maintainability
- **Sudo privileges** must be carefully controlled and audited
- **Compliance** requires proper user lifecycle management

A professional SysAdmin manages this cleanly, securely, and auditably.

---

# 📂 1️⃣ Understanding Critical System Files

## 🔍 The User Database Hierarchy

```ascii
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   /etc/passwd   │    │   /etc/shadow   │    │   /etc/group    │
│  User accounts  │    │  Password data  │    │   Group info    │
│  (World-read)   │◄───┤  (Root-only)    │    │  (World-read)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                        │                        │
         └────────────┬───────────┘                        │
                      │                                    │
               ┌─────────────┐                     ┌─────────────┐
               │  /etc/gshadow │                     │  /etc/login.defs │
               │ Group passwds │                     │  Default values  │
               └─────────────┘                     └─────────────┘
```

---

## 🔎 `/etc/passwd` - User Account Database

```bash
cat /etc/passwd
```

### Anatomy of an Entry:
```
ec2-user:x:1000:1000:EC2 User:/home/ec2-user:/bin/bash
└─────┬────┘ ┬ └─┬─┘ └──┬────┘ └────┬─────┘ └──┬────┘
   Username  │  UID  GID   GECOS    Home Dir   Shell
         Password
        (x = shadowed)
```

### Field Breakdown Table:
| Position | Field | Example | Description |
|----------|-------|---------|-------------|
| 1 | Username | `ec2-user` | Login name (1-32 chars) |
| 2 | Password | `x` | `x` = password in `/etc/shadow` |
| 3 | UID | `1000` | User ID (0=root, 1-999=system, 1000+=regular) |
| 4 | GID | `1000` | Primary Group ID |
| 5 | GECOS | `EC2 User` | Comment/Full name (comma-separated fields) |
| 6 | Home | `/home/ec2-user` | Home directory path |
| 7 | Shell | `/bin/bash` | Login shell (`/sbin/nologin` for service accounts) |

---

## 🔐 `/etc/shadow` - Secure Password Database

```bash
sudo cat /etc/shadow
```

### Example Entry:
```
ec2-user:$6$rAnDoMhAsH$MoReHaSh...:19876:0:99999:7:::
```

### Field-by-Field Explanation:
```
ec2-user:$6$rAnDoMhAsH$MoReHaSh...:19876:0:99999:7:::
│         │                    │      │   │    │  │││
│         │                    │      │   │    │  ││└─ Reserved
│         │                    │      │   │    │  │└── Inactive days
│         │                    │      │   │    │  └── Expiration warning
│         │                    │      │   │    └───── Max password age
│         │                    │      │   └────────── Min password age
│         │                    │      └────────────── Last changed (days since 1970-01-01)
│         │                    └───────────────────── Encrypted password
│         └────────────────────────────────────────── Password hash algorithm
│                                                     ($1=MD5, $5=SHA-256, $6=SHA-512)
└──────────────────────────────────────────────────── Username
```

---

## 👥 `/etc/group` - Group Definitions

```bash
cat /etc/group
```

### Format:
```
developers:x:1001:alice,bob,charlie
│          │   │    └──────────────┘
│          │   │        Members
│          │   └───────────── Group ID (GID)
│          └───────────────── Password placeholder
└──────────────────────────── Group name
```

---

# 👤 2️⃣ Professional User Creation

## 🎯 User Types & UID Ranges

```bash
# Check your system's UID/GID ranges
grep -E "^(UID|GID)_" /etc/login.defs
```

| User Type | UID Range | Purpose | Example Command |
|-----------|-----------|---------|-----------------|
| **Root** | 0 | Superuser | N/A |
| **System** | 1-999 | Daemons & services | `useradd -r -s /sbin/nologin mysql` |
| **Regular** | 1000+ | Human users | `useradd -m -u 1001 devuser` |
| **Custom Range** | Any | Application-specific | `useradd -u 5000 appuser` |

---

## 🛠 Advanced `useradd` Options

### Create with Full Profile
```bash
sudo useradd -m \
  -c "John Doe,Developer,555-1234" \
  -s /bin/bash \
  -k /etc/skel \
  -e 2024-12-31 \
  -f 7 \
  johndoe
```

### Breakdown:
- `-m` → Create home directory from `/etc/skel`
- `-c` → GECOS field (comma-separated: Full name,Room,Work,Home,Other)
- `-s` → Login shell
- `-k` → Skeleton directory template
- `-e` → Account expiry date (YYYY-MM-DD)
- `-f` → Days after password expiry until account is disabled

### Create Service Account (Daemon)
```bash
sudo useradd -r \
  -s /usr/sbin/nologin \
  -d /var/lib/nginx \
  -c "Nginx web server" \
  nginx
```

---

## 🔄 User Modification Patterns

### Change Multiple Properties
```bash
sudo usermod \
  -d /home/john_new \  # New home directory
  -m \                 # Move contents
  -s /bin/zsh \        # New shell
  -l jdoe \            # New login name
  -c "John Doe Sr." \  # New comment
  johndoe
```

### Add to Supplementary Groups
```bash
sudo usermod -aG wheel,developers,admins johndoe
```

### Set Primary Group
```bash
sudo usermod -g developers johndoe
```

---

# 👥 3️⃣ Group Management Mastery

## 🏗 Creating & Managing Groups

### Create with Specific GID
```bash
sudo groupadd -g 2001 project-alpha
```

### Modify Group
```bash
sudo groupmod -n alpha-team project-alpha  # Rename
sudo groupmod -g 2002 alpha-team           # Change GID
```

### Add Multiple Users
```bash
# Method 1: Using gpasswd
sudo gpasswd -a alice alpha-team
sudo gpasswd -a bob alpha-team
sudo gpasswd -a charlie alpha-team

# Method 2: Set entire member list
sudo gpasswd -M "alice,bob,charlie" alpha-team
```

### Remove User from Group
```bash
sudo gpasswd -d alice alpha-team
```

---

## 🔍 Advanced Group Verification

```bash
# Show all groups a user belongs to
id alice

# Show group members
getent group alpha-team

# Find which groups have specific user
groups alice

# Check if user is in group (exit code shows result)
getent group alpha-team | grep -q "\balice\b" && echo "Member" || echo "Not member"
```

---

# 🛡 4️⃣ Security & Account Controls

## 🔐 Password Management

### Set Password Non-Interactively
```bash
echo "MySecurePass123" | sudo passwd --stdin johndoe
```

### Force Password Change on First Login
```bash
sudo passwd -e johndoe  # Expire password immediately
```

### Check Password Status
```bash
sudo passwd -S johndoe
# Output: johndoe P 05/15/2024 0 99999 7 -1
# P = Password set, L = Locked, NP = No password
```

### Lock/Unlock Accounts
```bash
sudo usermod -L johndoe      # Lock
sudo usermod -U johndoe      # Unlock
sudo passwd -l johndoe       # Alternative lock
sudo passwd -u johndoe       # Alternative unlock
```

---

## ⏳ Account Expiry Management

### Set Expiry Date
```bash
sudo usermod -e 2024-12-31 johndoe
```

### Check Expiry
```bash
sudo chage -l johndoe
```

### Set Password Aging Policy
```bash
sudo chage \
  -M 90 \      # Max password age (days)
  -m 7 \       # Min password age
  -W 14 \      # Warning period
  -I 30 \      # Inactive days after expiry
  johndoe
```

---

# 🧪 Hands-On Enterprise Lab

## 🔹 Lab 1: Web Server Environment Setup

```bash
# Create service groups
sudo groupadd -g 2001 web-admins
sudo groupadd -g 2002 web-developers

# Create service account for nginx
sudo useradd -r -s /sbin/nologin -u 201 -g nginx nginx

# Create developer accounts with expiry
sudo useradd -m -e 2025-06-30 -c "Alice Developer" -G web-developers alice
sudo useradd -m -e 2025-06-30 -c "Bob Admin" -G web-admins,web-developers bob

# Set different passwords
echo "AlicePass123" | sudo passwd --stdin alice
echo "BobPass456" | sudo passwd --stdin bob

# Force password change on first login for Alice
sudo passwd -e alice
```

### Verification Commands:
```bash
# Check user details
id alice
id bob
id nginx

# Check group memberships
getent group web-developers
getent group web-admins

# Check account expiry
sudo chage -l alice
sudo chage -l bob
```

---

## 🔹 Lab 2: Shared Project Directory with SGID

```bash
# Create project group
sudo groupadd -g 3001 project-x

# Add users to project
sudo usermod -aG project-x alice
sudo usermod -aG project-x bob

# Create shared directory with SGID bit
sudo mkdir -p /opt/project-x/shared
sudo chgrp project-x /opt/project-x/shared
sudo chmod 2775 /opt/project-x/shared  # SGID + rwx for group

# Verify
ls -ld /opt/project-x/shared
# Should show: drwxrwsr-x

# Test: Alice creates file
sudo su - alice -c "touch /opt/project-x/shared/alice-file.txt"
ls -l /opt/project-x/shared/alice-file.txt
# File should be owned by alice but group should be project-x
```

---

# 🚨 Troubleshooting Common Issues

## ❌ "useradd: group 'developers' exists"
```bash
# Check if group exists
getent group developers

# If it does, either use different name or 
# specify GID explicitly to avoid conflict
sudo useradd -m -g 1500 devuser2
```

## ❌ "usermod: user 'username' is currently logged in"
```bash
# Check who's logged in
who
w

# Force modify (use with caution)
sudo usermod -f -L username
# Or wait for user to logout, or kill sessions
sudo pkill -KILL -u username
```

## ❌ Can't delete user: "userdel: user bob is currently logged in"
```bash
# Force deletion
sudo userdel -f -r bob
```

## ❌ Group modifications not taking effect
```bash
# User needs to re-login or start new shell
# Or use newgrp temporarily
newgrp developers
```

---

# 📊 Quick Reference Cheat Sheet

## Daily Tasks
```bash
# Add user to group
sudo usermod -aG groupname username

# Remove user from group
sudo gpasswd -d username groupname

# Lock account
sudo usermod -L username

# Check user info
id username
finger username  # If installed

# Find all users in group
getent group groupname
```

## Audit Commands
```bash
# List all users
cut -d: -f1 /etc/passwd

# List all system users (UID < 1000)
awk -F: '$3 < 1000 {print $1}' /etc/passwd

# List users with shells
grep -v "/nologin\|/false" /etc/passwd | cut -d: -f1

# Find users without passwords
sudo awk -F: '($2 == "" || $2 == "!") {print $1}' /etc/shadow
```

---

# 📌 Key Concepts Learned

✅ **User ID ranges** and their significance  
✅ **Password hash algorithms** in `/etc/shadow`  
✅ **Service accounts** vs regular users  
✅ **Account expiry** and password aging  
✅ **SGID directories** for collaboration  
✅ **Troubleshooting** common user/group issues  
✅ **Audit commands** for security reviews  

---

# 🚀 What's Next?

**Day 12 – Sudoers Deep Dive & Privilege Escalation Control**  
> Mastering the sudoers file, least privilege principles, and audit logging for compliance.

---

## 🎯 Tomorrow's Preview:
- Understanding the `/etc/sudoers` syntax
- Creating granular sudo permissions
- Setting up command aliases and defaults
- Implementing sudo logging and alerts
- Best practices for production environments

---

## 💡 Pro Tip: Practice Daily
```bash
# Create a test environment
sudo useradd -m testuser
# Experiment with different options
sudo usermod -aG sudo testuser
# Clean up
sudo userdel -r testuser
```

> **Remember:** Always test changes in a non-production environment first. User management mistakes can lock you out of systems!

---

## ❓ FAQ Quick Answers

**Q: What's the difference between `-g` and `-G` in usermod?**  
A: `-g` sets primary group (changes GID in /etc/passwd), `-G` sets supplementary groups.

**Q: Why use `-aG` instead of just `-G`?**  
A: `-aG` appends to existing groups; `-G` alone replaces all supplementary groups.

**Q: What UID should I use for service accounts?**  
A: Typically below 1000. Check `/etc/login.defs` for your system's SYS_UID range.

**Q: How to see when a user last logged in?**  
A: Use `last username` or check `/var/log/auth.log` (location varies by distro).

---

**📚 Further Reading:**  
- `man useradd`, `man usermod`, `man groupadd`  
- Linux Foundation's "Linux System Administration" guide  
- Red Hat's "Enterprise Linux User Management" documentation
