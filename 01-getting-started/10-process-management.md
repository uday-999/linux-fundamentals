# 🐧 Day 10 - Linux Process Management ⚙️

> **Mastering Linux Processes:** Learn how to monitor, control, and manage running programs efficiently.

Process management is essential for:

- System stability
- Troubleshooting performance issues
- Managing background services
- Server administration tasks

---

## 📊 1) What is a Linux Process?

A **process** is a running instance of a program.

| Component | Description |
|------------|------------|
| Program | Static code stored on disk (e.g., `/bin/bash`) |
| Process | Running program loaded into memory |
| PID | Unique Process ID assigned by the kernel |

### Example:
```bash
sleep 300
```

This creates a process that runs for 5 minutes.

---

## 🔍 2) Monitoring Processes

### 🔹 Using `ps`

```bash
ps                # Current terminal processes
ps aux            # All processes with details
ps auxf           # Tree view (parent-child structure)
```

### Custom view (Top memory usage)

```bash
ps -eo pid,comm,%mem --sort=-%mem | head -10
```

### Understanding `ps aux` columns:

```
USER   PID  %CPU  %MEM   VSZ   RSS  TTY   STAT  START  TIME  COMMAND
```

- **PID** → Process ID  
- **%CPU** → CPU usage  
- **%MEM** → Memory usage  
- **STAT** → Process state (R, S, Z, etc.)  
- **VSZ** → Virtual memory size  
- **RSS** → Physical memory usage  

---

## 📈 3) Real-Time Monitoring with `top`

```bash
top
```

Inside `top`:

- `P` → Sort by CPU  
- `M` → Sort by Memory  
- `k` → Kill a process  
- `q` → Quit  

Optional (Amazon Linux):
```bash
sudo yum install htop
htop
```

---

## 🎯 4) Finding Processes

### Using `pgrep`

```bash
pgrep nginx
pgrep -l nginx
pgrep -a nginx
```

### Find process using a port

```bash
sudo lsof -i :80
```

---

## ⚡ 5) Job Control (Background & Foreground)

### Run in background
```bash
sleep 500 &
```

### View background jobs
```bash
jobs -l
```

### Bring job to foreground
```bash
fg %1
```

### Suspend process
Press:
```
Ctrl + Z
```

Then resume in background:
```bash
bg
```

---

## 🛑 6) Terminating Processes

### Common Signals

| Signal | Number | Purpose |
|--------|--------|----------|
| SIGTERM | 15 | Graceful termination (default) |
| SIGKILL | 9 | Force kill |
| SIGSTOP | 19 | Pause process |

### Examples:

```bash
kill 1234          # Graceful stop
kill -9 1234       # Force kill
pkill nginx        # Kill by name
killall httpd      # Kill all httpd processes
```

⚠️ Always try `kill` before using `kill -9`.

---

## 🏃 7) Persistent Processes with `nohup`

To keep a process running after logout:

```bash
nohup python app.py &
```

Redirect output:

```bash
nohup ./server.sh > server.log 2>&1 &
```

---

## 🚨 8) Troubleshooting

### Find High CPU Usage
```bash
ps aux --sort=-%cpu | head -5
```

### Find High Memory Usage
```bash
ps aux --sort=-%mem | head -5
```

### Monitor continuously
```bash
watch -n 1 'ps aux --sort=-%mem | head -10'
```

### Find Zombie Processes
```bash
ps aux | grep 'Z'
```

---

## 🧪 Hands-On Practice

```bash
sleep 300 &
pgrep sleep
kill PID
```

Start a simple web server:

```bash
nohup python -m http.server 8000 > web.log 2>&1 &
pgrep -f http.server
kill PID
```

---

## 📋 Quick Cheat Sheet

```bash
ps aux
top
pgrep -l nginx
kill PID
kill -9 PID
jobs -l
fg %1
bg
nohup command &
```

---

## 🎯 Key Takeaways

✅ Every command becomes a process  
✅ Each process has a unique PID  
✅ Monitor using `ps` and `top`  
✅ Use `kill` carefully  
✅ Manage background jobs  
✅ Use `nohup` for persistent processes  

---

## 🚀 Next Step

With core Linux fundamentals completed (Day 01–10),  
tomorrow begins the **Linux System Administration phase**.
