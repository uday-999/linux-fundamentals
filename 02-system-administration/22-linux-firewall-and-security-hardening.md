# 🛡️ Day 22 – Linux Firewall & Basic Security Hardening

## 📌 Introduction

In real-world system administration, securing your Linux server is one of the most important responsibilities.

Today we will learn:

* What is a firewall?
* How to manage `firewalld`
* How to open/close ports
* Basic SSH hardening
* Password policy configuration
* Disabling unused services

---

# 🔥 1️⃣ What is a Firewall?

A firewall controls incoming and outgoing network traffic based on defined security rules.

In Linux:

* 🔹 `firewalld` (RHEL / CentOS / Amazon Linux)
* 🔹 `ufw` (Ubuntu)
* 🔹 `iptables` (legacy)

---

# 🚀 2️⃣ Managing firewalld

## Check Status

```bash
sudo systemctl status firewalld
```

or

```bash
sudo firewall-cmd --state
```

---

## Start Firewall

```bash
sudo systemctl start firewalld
```

---

## Enable at Boot

```bash
sudo systemctl enable firewalld
```

---

# 🌍 3️⃣ View Firewall Rules

```bash
sudo firewall-cmd --list-all
```

Check active zones:

```bash
sudo firewall-cmd --get-active-zones
```

---

# 🚪 4️⃣ Open a Port

Example: Open HTTP (Port 80)

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```

Open SSH:

```bash
sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload
```

---

# ❌ 5️⃣ Remove a Port

```bash
sudo firewall-cmd --remove-port=80/tcp --permanent
sudo firewall-cmd --reload
```

---

# 🔎 6️⃣ Check Open Ports

```bash
ss -tulnp
```

or

```bash
netstat -tulnp
```

---

# 🔐 7️⃣ SSH Security Hardening

## Disable Root Login

Edit SSH config:

```bash
sudo vi /etc/ssh/sshd_config
```

Find and change:

```
PermitRootLogin no
```

Restart SSH:

```bash
sudo systemctl restart sshd
```

---

## Change SSH Port (Optional Advanced)

```
Port 2222
```

Then open port 2222 in firewall.

---

# 🔒 8️⃣ Password Policy Configuration

Edit:

```bash
sudo vi /etc/login.defs
```

Update values:

```
PASS_MAX_DAYS 90
PASS_MIN_DAYS 7
PASS_WARN_AGE 7
```

---

# ⚙️ 9️⃣ Disable Unused Services

List services:

```bash
systemctl list-units --type=service
```

Disable unused service:

```bash
sudo systemctl disable service_name
```

---

# 🧪 Mini Lab Practice

1. Install firewalld
2. Start and enable it
3. Open port 8080
4. Verify port using `ss -tulnp`
5. Remove port 8080
6. Disable root SSH login

---

# 🧠 Interview Questions

1. What is firewalld?
2. Difference between firewalld and iptables?
3. What is a zone in firewalld?
4. How do you permanently open a port?
5. How do you secure SSH in production?

---

# 🏁 Conclusion

Today we learned how to:

* Manage firewall rules
* Secure SSH access
* Implement password policies
* Disable unnecessary services

These are essential skills for any Linux System Administrator.
