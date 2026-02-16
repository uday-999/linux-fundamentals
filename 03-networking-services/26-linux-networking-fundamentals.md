# 🌐 Day 26 – Linux Networking Fundamentals

## 📌 Introduction

For a Linux server to communicate with other systems, it must be properly configured with:

* IP Address
* Subnet Mask
* Default Gateway
* DNS

Networking is the backbone of system administration.

---

# 🌍 1️⃣ What is an IP Address?

An IP Address uniquely identifies a device on a network.

Two types:

* **IPv4** → 32-bit (Example: 192.168.1.10)
* **IPv6** → 128-bit (Example: 2001:db8::1)

Check IP address:

```bash
ip a
```

---

# 🧮 2️⃣ Subnet Mask

A Subnet Mask divides IP into:

* Network portion
* Host portion

Example:

IP: 192.168.1.10
Subnet: 255.255.255.0 (/24)

---

# 🚪 3️⃣ Default Gateway

The gateway is the exit point of your network.

Check routing table:

```bash
ip route show
```

Add default route manually:

```bash
sudo ip route add default via 192.168.1.1
```

---

# 🌐 4️⃣ DNS Configuration

DNS translates domain names into IP addresses.

File:

```bash
/etc/resolv.conf
```

Example:

```bash
nameserver 8.8.8.8
```

---

# 🔌 5️⃣ Network Interfaces

Check interfaces:

```bash
ip link show
```

Bring interface up:

```bash
sudo ip link set eth0 up
```

Bring interface down:

```bash
sudo ip link set eth0 down
```

---

# 🛠 6️⃣ Important Networking Commands

### 🔍 ping

```bash
ping google.com
```

Used to test connectivity.

---

### 📡 ss (Modern replacement of netstat)

```bash
ss -tulnp
```

Shows listening ports.

---

### 📦 tcpdump

```bash
sudo tcpdump -i eth0
```

Capture network packets.

---

# 🧠 How Linux Joins a Network

1. NIC is enabled
2. IP assigned (Static/DHCP)
3. Subnet applied
4. Gateway configured
5. DNS configured
6. Network service started

Once done → System can communicate.

---

# 🧪 Mini Lab

1. Check your IP using `ip a`
2. Check gateway using `ip route`
3. Ping google.com
4. Bring interface down and up
5. Edit `/etc/resolv.conf` and test DNS

---

# 🧠 Interview Questions

1. What is difference between IPv4 and IPv6?
2. What is subnet mask?
3. What is default gateway?
4. How do you check open ports?
5. Difference between netstat and ss?

---

# 🏁 Conclusion

Today we built foundation of Linux networking:

IP
Subnet
Gateway
DNS
Interfaces
Connectivity testing

Networking knowledge is mandatory for:

* SSH access
* Web servers
* Firewalls
* Cloud systems
* DevOps pipelines

---

🔥 Day 26 Completed
📅 Linux Advanced Track Started
👨‍💻 #Linux #Networking #SysAdmin #DevOps

---
