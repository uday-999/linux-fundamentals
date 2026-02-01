# 🐧 Day 12 - Sudoers Deep Dive & Privilege Escalation Control

> **Least Privilege in Practice** - Controlling who can do what, when, and how on your Linux systems.

**User management gives identity. Sudo management gives authority.**  
This is where theoretical security meets practical system administration.

---

## 🎯 Why Sudo Matters - The Security Foundation

### The Problem
Without proper sudo control, you're one mistake away from:
- **Data destruction** (`rm -rf /`)
- **Service disruption** (restarting critical services)
- **Security breaches** (adding unauthorized users)
- **Compliance violations** (unlogged admin actions)

### The Solution
Proper sudo implementation provides:
- ✅ **Least Privilege**: Only necessary permissions
- ✅ **Accountability**: Who did what and when
- ✅ **Auditability**: Complete action trail
- ✅ **Containment**: Damage limitation boundaries

---

## 🔐 1. Understanding sudo - More Than Just Root

### What sudo Really Means
```
sudo = "superuser do" + security model + audit trail
```

**Key Distinctions:**
```bash
# Running as root
sudo command

# Running as specific user
sudo -u apache /bin/bash

# Running as specific user with their environment
sudo -i -u jenkins
```

### How sudo Works Under the Hood
1. Checks `/etc/sudoers` and `/etc/sudoers.d/*`
2. Validates user credentials (or cached auth)
3. Executes command with elevated privileges
4. Logs the action to syslog/journald

---

## 📂 2. The Sudoers File - Anatomy & Best Practices

### Primary Configuration Files
```bash
/etc/sudoers                    # Main configuration
/etc/sudoers.d/                 # Modular configuration directory
```

### The Golden Rule: Always Use `visudo`
```bash
# NEVER do this:
sudo nano /etc/sudoers

# ALWAYS do this:
sudo visudo

# For files in sudoers.d:
sudo visudo -f /etc/sudoers.d/web-admins
```

**Why?** `visudo` performs:
- Syntax validation before saving
- Lock file prevention
- Atomic file writing
- Backup creation

---

## 🧩 3. Sudoers Syntax - The Complete Reference

### Basic Structure
```
User_Host=(Runas_User:Runas_Group) NOPASSWD: Command_List
```

### Practical Examples with Explanations

**Example 1: Full Admin Access**
```bash
# user    host   = (run-as-user)   commands
jdoe      ALL    = (ALL)           ALL
# ↑          ↑          ↑              ↑
# User     Any host   As any user   Any command
```

**Example 2: Service-Specific Access**
```bash
# Allow restarting specific services only
webserver ALL = (root) /bin/systemctl restart nginx, \
                      /bin/systemctl restart php-fpm
```

**Example 3: Database Admin with Constraints**
```bash
# Allow DB operations but prevent shell access
dbadmin ALL = (postgres) /usr/bin/psql, \
               (root) /bin/systemctl restart postgresql
```

**Example 4: Time-Restricted Access**
```bash
# Allow backups only during maintenance window
backupops ALL = (root) /usr/local/bin/run-backup
# Requires additional PAM configuration
```

---

## 👥 4. Group-Based Management - The Enterprise Way

### The Wheel Group (RHEL/CentOS/Amazon Linux)
```bash
# Check wheel configuration
sudo grep -i wheel /etc/sudoers

# Add user to wheel group
sudo usermod -aG wheel username

# Verify membership
groups username
id username
```

### Creating Custom Admin Groups
```bash
# 1. Create admin groups
sudo groupadd web-admins
sudo groupadd db-admins
sudo groupadd backup-admins

# 2. Add users to groups
sudo usermod -aG web-admins alice
sudo usermod -aG db-admins bob

# 3. Configure sudoers
%web-admins ALL = (root) /bin/systemctl * httpd, /bin/systemctl * nginx
%db-admins ALL = (postgres) /usr/bin/psql, /usr/bin/pg_dump
```

### LDAP/AD Integration
```bash
# For centralized management
%DOMAIN\\Linux-Admins ALL=(ALL) ALL
```

---

## 🛡 5. Least Privilege Implementation - Real World Examples

### Scenario 1: Web Developer
```bash
# Allow: Managing web content, viewing logs
# Deny: Installing packages, user management

Cmnd_Alias WEB_CONTENT = /bin/cp /var/www/*, /bin/chown apache /var/www/*
Cmnd_Alias VIEW_LOGS = /bin/tail -f /var/log/httpd/*, /usr/bin/less /var/log/nginx/*

webdev ALL = (apache) WEB_CONTENT, \
             (root) VIEW_LOGS
```

### Scenario 2: Backup Operator
```bash
Cmnd_Alias BACKUP_CMDS = /usr/bin/rsync, /bin/tar, /usr/bin/du
Cmnd_Alias STORAGE_CMDS = /bin/mount /backup*, /bin/umount /backup*

backupop ALL = (root) NOPASSWD: BACKUP_CMDS, \
               (root) STORAGE_CMDS
```

### Scenario 3: Docker Administrator
```bash
# Allow Docker management without full root
dockeradm ALL = (root) /usr/bin/docker, \
                (root) /usr/bin/docker-compose, \
                (root) /bin/systemctl * docker
```

---

## 📦 6. Advanced Configuration Techniques

### Using Aliases Effectively
```bash
# User Aliases
User_Alias WEB_ADMINS = alice, bob, charlie
User_Alias DB_ADMINS = david, eve

# Host Aliases
Host_Alias PRODUCTION = webserver1, dbserver1
Host_Alias STAGING = staging-web*, staging-db*

# Command Aliases
Cmnd_Alias SERVICES = /bin/systemctl start *, /bin/systemctl stop *, \
                      /bin/systemctl restart *, /bin/systemctl status *
Cmnd_Alias PACKAGE_MGMT = /usr/bin/yum install, /usr/bin/yum update, \
                          /usr/bin/yum remove

# Runas Aliases
Runas_Alias WEB_USERS = apache, nginx
Runas_Alias DB_USERS = postgres, mysql

# Putting it all together
WEB_ADMINS PRODUCTION = (WEB_USERS) SERVICES, \
                        (root) PACKAGE_MGMT httpd nginx php*
```

### Environment Variable Control
```bash
# Reset dangerous environment variables
Defaults env_reset
Defaults env_keep = "LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET"
Defaults env_delete = "PATH PYTHONPATH LD_LIBRARY_PATH"

# Allow preserving specific variables for certain commands
webdev ALL = (root) SETENV: /usr/local/bin/deploy-script
```

### Defaults Configuration
```bash
# Security defaults (add to top of sudoers)
Defaults timestamp_timeout=5            # Re-auth every 5 minutes
Defaults passwd_tries=3                 # Max 3 password attempts
Defaults logfile=/var/log/sudo.log      # Separate sudo log
Defaults iolog_dir=/var/log/sudo-io     # Command I/O logging
Defaults requiretty                      # Require TTY (security)
Defaults !lecture                       # Disable lecture message
Defaults mailto="security@company.com"  # Email alerts
Defaults mail_always                    # Email on every sudo use
Defaults use_pty                        # Run in pseudo-terminal
Defaults log_input, log_output          # Full command logging
```

---

## 🔍 7. Security Hardening & Audit

### Logging Configuration
```bash
# Enhanced logging in sudoers
Defaults log_host, log_year, log_input, log_output
Defaults iolog_dir=/var/log/sudo-io/%{user}
Defaults iolog_file=%{seq}

# Check sudo logs
sudo grep sudo /var/log/secure
sudo journalctl _COMM=sudo
sudo aureport --start today --input-logs | grep sudo
```

### Monitoring Sudo Usage
```bash
# Real-time monitoring
sudo tail -f /var/log/secure | grep sudo

# Daily report script
#!/bin/bash
echo "=== Sudo Usage Report for $(date) ==="
echo "Top sudo users:"
sudo grep "sudo:" /var/log/secure | cut -d: -f4 | sort | uniq -c | sort -rn
echo ""
echo "Commands run with sudo:"
sudo grep "COMMAND" /var/log/secure | cut -d= -f2 | sort | uniq -c | sort -rn
```

### Sudo Session Management
```bash
# Check active sudo sessions
sudo -l                    # Current user's privileges
sudo -U username -l       # Another user's privileges
sudo -k                   # Immediately expire cached credentials

# Session timeout control
Defaults timestamp_timeout=15     # 15 minutes idle timeout
Defaults passwd_timeout=1         # 1 minute password timeout
```

---

## 🧪 8. Hands-On Lab - Enterprise Scenario

### Lab: Implementing Role-Based Access Control

**Scenario:**  
Company needs three admin roles:
1. Web Admins - Manage web services only
2. DB Admins - Database operations only
3. Security Team - Read logs and audit

**Step 1: Create Groups and Users**
```bash
sudo groupadd web-admins
sudo groupadd db-admins
sudo groupadd security-auditors

sudo useradd -m -G web-admins webadmin1
sudo useradd -m -G db-admins dbadmin1
sudo useradd -m -G security-auditors auditor1
```

**Step 2: Create Modular Sudoers Files**
```bash
# /etc/sudoers.d/web-admins
%web-admins ALL = (root) Cmnd_Alias WEB_SERVICES
Cmnd_Alias WEB_SERVICES = /bin/systemctl * apache*, \
                          /bin/systemctl * nginx*, \
                          /bin/systemctl * php*, \
                          /usr/bin/tail /var/log/httpd/*, \
                          /usr/bin/tail /var/log/nginx/*

# /etc/sudoers.d/db-admins
%db-admins ALL = (postgres) /usr/bin/psql, \
                 (mysql) /usr/bin/mysql, \
                 (root) /bin/systemctl * postgresql, \
                 (root) /bin/systemctl * mysqld

# /etc/sudoers.d/security-auditors
%security-auditors ALL = (root) NOPASSWD: /usr/bin/tail /var/log/*, \
                         /bin/cat /var/log/*, \
                         /usr/bin/journalctl
```

**Step 3: Test Each Role**
```bash
# Switch to web admin
sudo su - webadmin1
sudo systemctl status nginx     # Should work
sudo useradd testuser           # Should fail

# Switch to security auditor
sudo su - auditor1
sudo tail /var/log/secure       # Should work without password
sudo systemctl restart nginx    # Should fail
```

**Step 4: Implement Audit Trail**
```bash
# Add to main sudoers
Defaults:%web-admins log_input, log_output
Defaults:%db-admins log_input, log_output
Defaults:%security-auditors syslog=local1
```

---

## 🚨 9. Common Pitfalls & Anti-Patterns

### ❌ Dangerous Practices to Avoid
```bash
# 1. World-writable sudoers
sudo chmod 777 /etc/sudoers.d/

# 2. Wildcard abuse
username ALL = (ALL) /bin/systemctl *   # Too permissive!

# 3. Passwordless ALL
username ALL = (ALL) NOPASSWD: ALL      # Security disaster

# 4. PATH injection
Defaults !secure_path                    # Allows PATH manipulation
```

### ✅ Security Best Practices
1. **Use groups instead of individual users**
2. **Implement time-based restrictions where possible**
3. **Regularly audit sudo usage**
4. **Use `NOPASSWD:` sparingly and specifically**
5. **Maintain separate sudoers.d files per role**
6. **Version control your sudoers configuration**
7. **Test changes in staging first**
8. **Implement break-glass emergency accounts separately**

---

## 🔧 10. Troubleshooting Guide

### Common Issues and Solutions

**Issue:** "User is not in the sudoers file"
```bash
# Solution: Check group membership
groups username
sudo visudo -c
sudo grep username /etc/sudoers /etc/sudoers.d/*
```

**Issue:** Sudo asks for password every time
```bash
# Check timestamp settings
sudo grep timestamp /etc/sudoers

# Clear cached credentials
sudo -k
```

**Issue:** Command works in shell but not via sudo
```bash
# Check secure_path
sudo grep secure_path /etc/sudoers

# Check command path
which command
sudo which command
```

**Issue:** Environment variables missing in sudo
```bash
# Preserve specific variables
Defaults env_keep += "VAR1 VAR2"
```

---

## 📊 11. Compliance & Reporting

### PCI-DSS, HIPAA, SOX Requirements
```bash
# Generate compliance reports
sudo_report.sh > sudo-compliance-$(date +%Y%m%d).csv

# Sample report script
#!/bin/bash
echo "User,Host,Command,Timestamp"
sudo grep "sudo:" /var/log/secure | \
  awk '{print $1","$2","$3","$4","$5","$6","$7","$8","$9","$10}'
```

### Regular Audit Checklist
- [ ] Review sudoers file quarterly
- [ ] Remove inactive users from admin groups
- [ ] Verify no NOPASSWD: ALL entries exist
- [ ] Check sudo logs for anomalies
- [ ] Validate backup admin accounts
- [ ] Test emergency access procedures

---

## 📌 Key Takeaways

✅ **Sudo is a privilege, not a right** - implement accordingly  
✅ **Least privilege is the golden rule** - give only what's needed  
✅ **Auditability is non-negotiable** - log everything  
✅ **Groups over users** - for scalability  
✅ **Test before deploy** - use visudo and test accounts  

---

## 🚀 What's Next: Day 13

**User Activity Monitoring & Forensic Analysis**  
Learn to track, audit, and investigate user activities using:
- `who`, `w`, `last`, `lastlog`
- Process accounting with `psacct`
- Auditd for enterprise auditing
- Login anomaly detection
