# 🐧 Day 17 – Service Management with systemd
## Managing Services Like a Real Linux SysAdmin

> In modern Linux systems (including Amazon Linux, RHEL, CentOS, Ubuntu), **systemd** is the core service manager.  
If you can control services confidently, you can control the system.

---

## 🎯 Why systemd Matters

In production environments:
- Web servers must auto-start after reboot
- Database services must be monitored
- Failed services must restart automatically
- Background processes must run reliably
- Logs must be traceable

Systemd makes all of this possible.

---

# 🔎 1️⃣ What is systemd?

`systemd` is the **init system and service manager** in most modern Linux distributions.

It:
- Starts services at boot
- Manages background processes
- Tracks dependencies
- Logs service activity
- Handles restarts and failures

Check if systemd is running:

```bash
ps -p 1 -o comm=
```

If it shows:
```
systemd
```
You're using systemd.

---

# 📦 2️⃣ Understanding Service Units

Systemd manages **units**.

Common unit types:

| Unit Type | Purpose |
|------------|----------|
| `.service` | Service (nginx, httpd, sshd) |
| `.socket` | Socket activation |
| `.target` | Group of services |
| `.mount` | Mount points |
| `.timer` | Scheduled tasks (cron alternative) |

Example:
```
nginx.service
sshd.service
cron.service
```

---

# ▶️ 3️⃣ Basic Service Management Commands

### Start a Service
```bash
sudo systemctl start nginx
```

### Stop a Service
```bash
sudo systemctl stop nginx
```

### Restart a Service
```bash
sudo systemctl restart nginx
```

### Reload Configuration (without full restart)
```bash
sudo systemctl reload nginx
```

---

# 🔍 4️⃣ Checking Service Status

```bash
systemctl status nginx
```

Example output:
```
Active: active (running)
Main PID: 1234
```

Key states:
- active (running)
- inactive
- failed
- activating
- deactivating

---

# 🚀 5️⃣ Enable / Disable at Boot

Start automatically after reboot:

```bash
sudo systemctl enable nginx
```

Disable from auto-start:

```bash
sudo systemctl disable nginx
```

Check if enabled:

```bash
systemctl is-enabled nginx
```

---

# 📋 6️⃣ Listing Services

### List all services
```bash
systemctl list-units --type=service
```

### List failed services
```bash
systemctl --failed
```

### List all installed service files
```bash
systemctl list-unit-files --type=service
```

---

# 🧠 7️⃣ Understanding Targets (Runlevels)

Old Linux used runlevels (0–6).  
Systemd uses **targets**.

| Old Runlevel | systemd Target |
|--------------|----------------|
| 0 | poweroff.target |
| 1 | rescue.target |
| 3 | multi-user.target |
| 5 | graphical.target |
| 6 | reboot.target |

Check current target:

```bash
systemctl get-default
```

Change default target:

```bash
sudo systemctl set-default multi-user.target
```

---

# 🔁 8️⃣ Restart Policies & Failure Handling

Check why a service failed:

```bash
journalctl -u nginx
```

Restart failed service:

```bash
sudo systemctl restart nginx
```

Reset failed state:

```bash
sudo systemctl reset-failed nginx
```

---

# 🧾 9️⃣ Viewing Logs with journalctl

Systemd integrates with journald.

### View logs for a service
```bash
journalctl -u nginx
```

### Follow logs live
```bash
journalctl -u nginx -f
```

### View logs since today
```bash
journalctl --since today
```

---

# 🛠 10️⃣ Creating a Custom Service

Example: Python App Service

### Step 1 – Create service file
```bash
sudo nano /etc/systemd/system/myapp.service
```

### Step 2 – Add configuration

```ini
[Unit]
Description=My Python App
After=network.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user/app
ExecStart=/usr/bin/python3 app.py
Restart=always

[Install]
WantedBy=multi-user.target
```

### Step 3 – Reload systemd
```bash
sudo systemctl daemon-reload
```

### Step 4 – Start & Enable
```bash
sudo systemctl start myapp
sudo systemctl enable myapp
```

---

# 🔐 11️⃣ Security Best Practices

✅ Use dedicated service accounts  
✅ Avoid running services as root unless required  
✅ Use `Restart=always` for critical services  
✅ Monitor logs with journalctl  
✅ Restrict service permissions  

---

# 🧪 12️⃣ Hands-On Lab

### Lab: Manage nginx service (Amazon Linux)

```bash
sudo yum install nginx -y

sudo systemctl start nginx
sudo systemctl enable nginx

systemctl status nginx

sudo systemctl stop nginx
```

Check if port 80 is active:

```bash
sudo ss -tulnp | grep 80
```

---

# 📌 Key Takeaways

✅ systemd replaces traditional init systems  
✅ systemctl controls services  
✅ enable ≠ start  
✅ journalctl is your logging tool  
✅ Custom services allow automation  
✅ Targets replace runlevels  

---

# 🚀 What’s Next?

**Day 18 – Networking Fundamentals for SysAdmins**
- IP configuration
- Network interfaces
- ss / netstat
- Firewall basics
- Troubleshooting connectivity

---
