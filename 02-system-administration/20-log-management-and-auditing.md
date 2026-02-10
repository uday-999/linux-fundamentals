# 🐧 Day 20 – Log Management & System Auditing

## Debugging, Monitoring & Investigating Linux Systems Like a Pro

> “If you don’t check logs, you’re not troubleshooting — you’re guessing.”

Logs are the **black box recorder** of Linux systems.
Every login, error, service crash, and permission issue leaves a trace.

Today we master:

* 📂 Where logs live
* 🔎 How to search them
* 📊 How to analyze them
* 🔄 How to rotate & manage them

---

## 📂 1️⃣ Understanding Linux Log Structure

Most logs are stored inside:

```bash
/var/log/
```

### Common Log Files

| File                | Purpose                                 |
| ------------------- | --------------------------------------- |
| `/var/log/messages` | General system logs (RHEL/Amazon Linux) |
| `/var/log/syslog`   | General logs (Debian/Ubuntu)            |
| `/var/log/secure`   | Authentication logs (RHEL-based)        |
| `/var/log/auth.log` | Authentication logs (Debian-based)      |
| `/var/log/dmesg`    | Kernel ring buffer                      |
| `/var/log/boot.log` | Boot messages                           |

---

## 🔎 2️⃣ Viewing & Searching Logs

### Basic Viewing

```bash
cat /var/log/messages
less /var/log/messages
tail -f /var/log/messages
```

### Real-Time Monitoring

```bash
tail -f /var/log/secure
```

### Search for Errors

```bash
grep -i error /var/log/messages
grep -i "failed password" /var/log/secure
```

### Count Failed SSH Attempts

```bash
grep "Failed password" /var/log/secure | wc -l
```

---

## 📘 3️⃣ journalctl – Modern Log Management (systemd)

Most modern systems use **systemd journal**.

### View All Logs

```bash
journalctl
```

### Show Logs from Today

```bash
journalctl --since today
```

### Filter by Service

```bash
journalctl -u sshd
journalctl -u nginx
```

### Filter by Time

```bash
journalctl --since "1 hour ago"
journalctl --since "2025-01-01" --until "2025-01-02"
```

### Show Only Errors

```bash
journalctl -p err
```

---

## 🔐 4️⃣ Auditing User Activity

### Who Logged In?

```bash
last
lastlog
```

### Current Users

```bash
who
w
```

### Monitor sudo Usage

```bash
journalctl _COMM=sudo
```

### Check Suspicious Activity

```bash
grep "authentication failure" /var/log/secure
```

---

## 🔄 5️⃣ Log Rotation (logrotate)

Logs grow fast.
Without rotation, disk space will fill up.

### Check Configuration

```bash
cat /etc/logrotate.conf
ls /etc/logrotate.d/
```

### Manual Test

```bash
sudo logrotate -d /etc/logrotate.conf
```

### Force Rotation

```bash
sudo logrotate -f /etc/logrotate.conf
```

### Sample Config

```bash
/var/log/myapp.log {
    weekly
    rotate 4
    compress
    missingok
    notifempty
}
```

This means:

* Rotate weekly
* Keep 4 old logs
* Compress old logs

---

## 🧪 6️⃣ Practical Troubleshooting Lab

### Scenario 1: SSH Login Failed

```bash
grep "Failed password" /var/log/secure
```

### Scenario 2: Service Not Starting

```bash
systemctl status nginx
journalctl -u nginx
```

### Scenario 3: Disk Full

```bash
du -sh /var/log/*
```

---

## 📊 7️⃣ Useful One-Liners

### Top IPs Trying to Login

```bash
grep "Failed password" /var/log/secure | awk '{print $11}' | sort | uniq -c | sort -nr | head
```

### Errors from Last Hour

```bash
journalctl --since "1 hour ago" -p err
```

### Largest Log Files

```bash
find /var/log -type f -exec du -h {} + | sort -rh | head -10
```

---

## 📌 Key Takeaways

* Logs are your **first stop** in troubleshooting.
* Use `journalctl` for modern systems.
* Monitor authentication logs for security.
* Always configure log rotation.
* Logs help reduce MTTR significantly.

---

## 🚀 What’s Next?

**Day 21 – Process Monitoring & Resource Analysis**

* top / htop
* nice & renice
* system resource tracking
* CPU & memory bottlenecks

---
