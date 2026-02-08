# 🐧 Day 19 – SSH & Secure Remote Access 🔐
## Hardening Remote Access for Production Linux Servers

![Lab](https://img.shields.io/badge/Lab-Complete-green)
![Level](https://img.shields.io/badge/Level-Intermediate-orange)
![Focus](https://img.shields.io/badge/Focus-Security-blue)

> **System Admin Rule:** If SSH is insecure, your entire server is insecure.  
> Secure remote access is not optional — it’s foundational.

---

## 📊 Quick Navigation

- [🎯 Why SSH Security Matters](#-why-ssh-security-matters)
- [🔑 SSH Key-Based Authentication](#-1-ssh-key-based-authentication)
- [⚙️ SSH Server Configuration](#️-2-ssh-server-configuration-hardening)
- [🛡 Production Hardening Checklist](#-3-production-hardening-checklist)
- [📁 SSH Client Configuration](#-4-ssh-client-configuration)
- [🚇 SSH Tunneling Basics](#-5-ssh-tunneling-basics)
- [🧪 Hands-On Lab](#-6-production-lab)
- [📚 Quick Reference](#-quick-reference-cheat-sheet)

---

# 🎯 Why SSH Security Matters

SSH provides:

- Remote command execution
- File transfers (SCP/SFTP)
- Port forwarding
- Secure tunneling

If misconfigured, it becomes:

- A brute-force target
- A root access entry point
- A pivot for lateral movement

---

# 🔑 1. SSH Key-Based Authentication

## Generate SSH Key Pair

```bash
ssh-keygen -t ed25519 -C "my-secure-key"
````

Recommended:

* Use `ed25519` (modern & secure)
* Set a strong passphrase

Keys are stored in:

```
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

---

## Copy Public Key to Server

```bash
ssh-copy-id user@server_ip
```

Manual method:

```bash
cat ~/.ssh/id_ed25519.pub | ssh user@server_ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

---

## Test Key Login

```bash
ssh user@server_ip
```

Confirm:

* No password prompt
* Login works successfully

⚠️ Always verify key login works **before disabling passwords**

---

# ⚙️ 2. SSH Server Configuration Hardening

Edit configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

## 🔐 Recommended Secure Settings

```bash
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
MaxAuthTries 3
LoginGraceTime 30
AllowUsers ec2-user
```

---

## Restart SSH Safely

```bash
sudo systemctl restart sshd
```

Before closing your current session:

* Open a new terminal
* Test login again

Never lock yourself out.

---

# 🛡 3. Production Hardening Checklist

| Security Control      | Purpose                      |
| --------------------- | ---------------------------- |
| Disable root login    | Prevent direct root access   |
| Disable password auth | Prevent brute force          |
| Use SSH keys only     | Strong authentication        |
| Change default port   | Reduce automated scans       |
| Limit users           | Reduce attack surface        |
| Fail2Ban              | Block brute force attempts   |
| Firewall rules        | Restrict SSH to specific IPs |

---

## Firewall Restriction Example (Amazon Linux / RHEL)

```bash
sudo firewall-cmd --add-port=2222/tcp --permanent
sudo firewall-cmd --reload
```

Restrict to specific IP:

```bash
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="YOUR_IP" port port="2222" protocol="tcp" accept' --permanent
```

---

# 📁 4. SSH Client Configuration

Instead of typing long commands:

Edit:

```bash
nano ~/.ssh/config
```

Example:

```
Host prod-server
    HostName 192.168.1.100
    User ec2-user
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
```

Now connect with:

```bash
ssh prod-server
```

Clean. Professional. Efficient.

---

# 🚇 5. SSH Tunneling Basics

## Local Port Forwarding

Access remote DB securely:

```bash
ssh -L 3306:localhost:3306 user@server_ip
```

Now:

* Connect to `localhost:3306`
* Traffic is encrypted via SSH

---

## Remote Port Forwarding

```bash
ssh -R 8080:localhost:80 user@server_ip
```

---

## Dynamic Port Forwarding (SOCKS Proxy)

```bash
ssh -D 1080 user@server_ip
```

Used for:

* Secure browsing
* Pen-testing labs
* Secure remote access

---

# 🧪 6. Production Lab

## Lab 1 – Secure SSH Setup

1. Generate SSH key
2. Copy to server
3. Disable password login
4. Change port to 2222
5. Restrict root login
6. Restart SSH
7. Verify connection

---

## Lab 2 – Brute Force Protection

Install Fail2Ban:

```bash
sudo yum install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

Check status:

```bash
sudo fail2ban-client status sshd
```

---

## Lab 3 – Audit SSH Access

Check login history:

```bash
last -a
```

Check failed attempts:

```bash
sudo grep "Failed password" /var/log/secure
```

Check SSH logs (systemd):

```bash
sudo journalctl -u sshd --since "1 hour ago"
```

---

# 📚 Quick Reference Cheat Sheet

```bash
# Generate SSH key
ssh-keygen -t ed25519

# Copy key to server
ssh-copy-id user@server

# Test login
ssh user@server

# Restart SSH
sudo systemctl restart sshd

# Check SSH service
sudo systemctl status sshd

# View open SSH port
ss -tlnp | grep ssh

# Check failed logins
sudo grep "Failed password" /var/log/secure
```

---

# ⚠️ Common Mistakes

❌ Disabling password authentication before testing key login
❌ Restarting SSH over remote session without fallback
❌ Forgetting firewall rule for new port
❌ Leaving root login enabled

---

# 🏆 Achievement Unlocked

You can now:

* Secure SSH for production systems
* Implement key-based authentication
* Restrict users and root access
* Configure SSH client profiles
* Use SSH tunneling professionally
* Audit and monitor remote access

---

# 🚀 Next Up – Day 20

## Log Management & System Auditing

* `journalctl`
* Log rotation
* Centralized logging
* Monitoring authentication events

---

> Security is not about reacting to breaches.
> It’s about preventing them.

---

**Lab Verified On:** Amazon Linux 2
**Time Required:** 30–45 minutes
**Part of:** Linux System Administration Journey

```

---

If you want, I can now give:

- 🔥 Matching LinkedIn post (Day 19)
- 📄 Short version for resume/project section
- 🛡️ Advanced SSH hardening (Ciphers, MACs, Kex algorithms)
- 🚀 Day 20 full GitHub content

Your system admin journey is becoming very strong now.
```
