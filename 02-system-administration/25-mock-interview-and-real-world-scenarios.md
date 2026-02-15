# 🎯 Day 25 – Mock Interview & Real-World Troubleshooting Scenarios

## 📌 Introduction

Being a System Administrator is not about memorizing commands.

It is about:

* Logical thinking
* Structured troubleshooting
* Understanding system behavior
* Staying calm under pressure

Today we simulate real-world production issues and solve them step-by-step.

---

# 🛑 Scenario 1: Disk Full Error

## Problem:

Server is slow. Applications are failing. Error shows: *No space left on device*.

## Step-by-Step Troubleshooting:

### 1️⃣ Check Disk Usage

```bash
df -h
```

### 2️⃣ Identify Large Directories

```bash
du -sh /*
```

### 3️⃣ Check Log Directory

```bash
du -sh /var/log
```

### 4️⃣ Find Large Files

```bash
find / -type f -size +100M
```

### 5️⃣ Rotate or Clean Logs

```bash
sudo logrotate -f /etc/logrotate.conf
```

---

# 🌐 Scenario 2: Website Not Accessible

## Problem:

Website is not opening.

## Troubleshooting Steps:

### 1️⃣ Check Service Status

```bash
systemctl status httpd
```

(or nginx)

```bash
systemctl status nginx
```

### 2️⃣ Check If Port Is Listening

```bash
ss -tulnp | grep 80
```

### 3️⃣ Check Firewall Rules

```bash
firewall-cmd --list-all
```

### 4️⃣ Check Logs

```bash
journalctl -xe
```

---

# 🔐 Scenario 3: SSH Not Working

## Troubleshooting Steps:

### 1️⃣ Check SSH Service

```bash
systemctl status sshd
```

### 2️⃣ Check SSH Port

```bash
ss -tulnp | grep ssh
```

### 3️⃣ Check Firewall

```bash
firewall-cmd --list-all
```

### 4️⃣ Verify Configuration

```bash
vi /etc/ssh/sshd_config
```

Check:

```
PermitRootLogin
Port
```

Restart after changes:

```bash
systemctl restart sshd
```

---

# ⚙ Scenario 4: High CPU Usage

## Step 1️⃣ Identify Process

```bash
top
```

or

```bash
htop
```

## Step 2️⃣ Check Process Details

```bash
ps aux | grep process_name
```

## Step 3️⃣ Kill Process (If Necessary)

```bash
kill -9 PID
```

---

# 🔥 Scenario 5: Service Failed to Start

## Step 1️⃣ Check Status

```bash
systemctl status service_name
```

## Step 2️⃣ Check Detailed Logs

```bash
journalctl -xe
```

## Step 3️⃣ Check Configuration File

```bash
vi /etc/service_name/config.conf
```

---

# 🧠 Troubleshooting Mindset

A Real SysAdmin Follows:

1. Identify the problem
2. Gather information
3. Analyze logs
4. Apply fix
5. Verify service
6. Monitor system

Never guess.
Always verify.

---

# 📝 Common Interview Questions

1. What will you do if disk space is 100% full?
2. How do you troubleshoot a failed service?
3. What tools do you use for monitoring CPU usage?
4. How do you check which port a service is running on?
5. How do you secure SSH in production?
6. How do you automate backups?
7. Difference between firewalld and iptables?
8. How do you check login history?

---

# 🏁 Final Conclusion – 25 Days Completed

Over the last 25 days, we covered:

* Linux Fundamentals
* File System
* User & Permission Management
* Process Management
* Service Management
* Firewall & Security
* Cron Jobs & Automation
* Log Rotation
* Troubleshooting

This journey built:

* Technical skills
* Production mindset
* Troubleshooting ability
* Discipline

Learning Linux is not about commands.
It is about thinking like a System Administrator.

---

🔥 25 Days Linux Challenge Completed
👨‍💻 From Basics → Real-World Scenarios
🚀 Ready for Advanced System Administration & Cloud

---
