# ⏰ Day 23 – Cron Jobs & Automation in Linux

## 📌 Introduction

Automation is one of the most powerful skills in System Administration.

Instead of performing repetitive tasks manually, we use **cron jobs** to schedule tasks automatically.

Today we learned:

* What is cron?
* Cron syntax
* Managing crontab
* Scheduling jobs
* Automating backups
* Best practices

---

# 🔄 1️⃣ What is Cron?

Cron is a time-based job scheduler in Linux.

It allows you to:

* Run scripts automatically
* Schedule backups
* Clean logs
* Restart services
* Run monitoring tasks

Cron runs in the background as a daemon.

---

# ⚙️ 2️⃣ Cron Service Management

Check cron service status:

```bash
sudo systemctl status crond
```

Start cron service:

```bash
sudo systemctl start crond
```

Enable at boot:

```bash
sudo systemctl enable crond
```

---

# 📝 3️⃣ Managing Crontab

Edit crontab:

```bash
crontab -e
```

List cron jobs:

```bash
crontab -l
```

Remove cron jobs:

```bash
crontab -r
```

---

# 🧠 4️⃣ Cron Syntax Explained

```
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of Week (0–7)
│ │ │ └──── Month (1–12)
│ │ └────── Day of Month (1–31)
│ └──────── Hour (0–23)
└────────── Minute (0–59)
```

Example:

```
0 2 * * * /home/uday/backup.sh
```

➡ Runs every day at 2:00 AM

---

# 🛠 5️⃣ Real Practice Examples

### Run every minute

```
* * * * * echo "Hello World" >> /home/uday/test.txt
```

---

### Run daily at 3 AM

```
0 3 * * * /home/uday/script.sh
```

---

### Run every Sunday at 5 PM

```
0 17 * * 0 /home/uday/weekly.sh
```

---

# 📂 6️⃣ Automating Backup Script

Create backup script:

```bash
vi backup.sh
```

Example script:

```bash
#!/bin/bash
tar -czf /home/uday/backup_$(date +%F).tar.gz /home/uday/data
```

Give execute permission:

```bash
chmod +x backup.sh
```

Schedule it:

```
0 2 * * * /home/uday/backup.sh
```

---

# 📄 7️⃣ Cron Logs

Check cron logs:

```bash
sudo journalctl -u crond
```

Or (RHEL/CentOS):

```bash
sudo tail -f /var/log/cron
```

---

# ⚠️ 8️⃣ Best Practices

* Always use full path of commands
* Redirect output to log file
* Test script manually before scheduling
* Use proper permissions
* Avoid running heavy jobs at peak hours

Example with logging:

```
0 2 * * * /home/uday/backup.sh >> /home/uday/backup.log 2>&1
```

---

# 🧪 Mini Lab Task

1. Create a test script
2. Schedule it to run every minute
3. Verify output file
4. Check cron logs
5. Remove cron job

---

# 🧠 Interview Questions

1. What is cron?
2. Explain cron syntax.
3. Difference between cron and at?
4. Where are cron logs stored?
5. How do you debug a failed cron job?

---

# 🏁 Conclusion

Today we learned how to:

* Automate repetitive tasks
* Schedule backups
* Manage crontab
* Debug cron jobs

Automation reduces manual effort and increases system reliability.
