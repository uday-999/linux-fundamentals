# 🗂 Day 24 – Log Rotation & System Maintenance

## 📌 Introduction

Logs are essential for monitoring, troubleshooting, and auditing systems.

However, if logs are not managed properly, they can:

* Fill up disk space
* Slow down the system
* Crash services
* Cause application failures

Today we learned how to manage logs efficiently using **logrotate**.

---

# 📁 1️⃣ Understanding Log Directory

Most logs are stored in:

```bash
/var/log
```

Check logs:

```bash
ls -lh /var/log
```

---

# 📊 2️⃣ Checking Disk Usage

Check disk usage:

```bash
df -h
```

Check folder size:

```bash
du -sh /var/log
```

Check specific file size:

```bash
du -sh /var/log/messages
```

---

# 🔄 3️⃣ What is Logrotate?

Logrotate is a Linux utility that:

* Automatically rotates logs
* Compresses old logs
* Deletes old logs
* Prevents disk overflow

Configuration files:

```bash
/etc/logrotate.conf
/etc/logrotate.d/
```

---

# ⚙️ 4️⃣ Main Logrotate Configuration

Open main config:

```bash
sudo vi /etc/logrotate.conf
```

Common options:

```bash
weekly
rotate 4
create
compress
missingok
notifempty
```

Meaning:

* `weekly` → Rotate logs weekly
* `rotate 4` → Keep 4 old logs
* `compress` → Compress old logs
* `create` → Create new log file after rotation
* `missingok` → Ignore missing logs
* `notifempty` → Skip empty logs

---

# 📦 5️⃣ Custom Log Rotation Example

Create custom config:

```bash
sudo vi /etc/logrotate.d/myapp
```

Example:

```bash
/home/uday/myapp.log {
    daily
    rotate 7
    compress
    missingok
    notifempty
}
```

This means:

* Rotate daily
* Keep 7 backups
* Compress old logs

---

# 🛠 6️⃣ Force Log Rotation

Test configuration:

```bash
sudo logrotate -d /etc/logrotate.conf
```

Force rotation:

```bash
sudo logrotate -f /etc/logrotate.conf
```

---

# 🧠 7️⃣ Maintenance Best Practices

✔ Monitor disk usage regularly
✔ Rotate logs weekly or daily
✔ Compress old logs
✔ Delete unnecessary logs
✔ Archive important logs
✔ Avoid storing logs in root partition

---

# 🔍 8️⃣ Troubleshooting Disk Full Issue

Steps:

1. Check disk space

   ```bash
   df -h
   ```

2. Find large directories

   ```bash
   du -sh /*
   ```

3. Find large files

   ```bash
   find / -type f -size +100M
   ```

4. Clean or rotate logs

---

# 🧪 Mini Lab Task

1. Check disk usage
2. Check `/var/log` size
3. Create custom logrotate config
4. Force rotation
5. Verify compressed log files

---

# 🧠 Interview Questions

1. What is logrotate?
2. Where are logrotate configuration files stored?
3. What does rotate 4 mean?
4. How do you troubleshoot disk full error?
5. How to manually rotate logs?

---

# 🏁 Conclusion

Today we learned:

* How to monitor disk space
* How log rotation works
* How to configure custom rotation
* How to prevent disk full issues

Log management is a critical responsibility of a System Administrator.

Proper maintenance prevents downtime.

---
