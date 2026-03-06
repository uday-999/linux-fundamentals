Great 🔥 let’s continue your streak.

🚀 Day 37 – DNS Tools (nslookup & dig)

These tools are very important for networking troubleshooting and are used by System Administrators, DevOps engineers, and network engineers.


---

📂 GitHub File Name

section-06-networking-services/-dns-toGreat 🔥 let’s continue your streak.

🚀 Day 37 – DNS Tools (nslookup & dig)

These tools are very important for networking troubleshooting and are used by System Administrators, DevOps engineers, and network engineers.


---

📂 GitHub File Name

section-06-networking-services/12-dns-tools-nslookup-dig.md


---

📄 Proper GitHub Repo Code (Day 37)

# 🚀 Day 37 – DNS Tools (nslookup & dig)

## 📌 Overview

DNS (Domain Name System) converts domain names into IP addresses.

Example:

google.com → 142.250.183.110

Linux provides tools like **nslookup** and **dig** to troubleshoot DNS issues.

---

# 🌐 1️⃣ nslookup Command

nslookup is used to query DNS servers and resolve domain names.

## Basic DNS Lookup

```bash
nslookup google.com

Output shows:

DNS server

IP address of the domain



---

Query Specific DNS Server

nslookup google.com 8.8.8.8

This queries Google's public DNS server.


---

Reverse DNS Lookup

Find domain name from IP address.

nslookup 8.8.8.8


---

🔎 2️⃣ dig Command (Domain Information Groper)

dig provides detailed DNS information.

Basic Lookup

dig google.com

Shows:

DNS response

Query time

DNS server used

Answer section



---

Short Output

dig google.com +short

Returns only the IP address.


---

Query Specific Record Type

MX Record (Mail Server)

dig google.com MX

NS Record (Name Servers)

dig google.com NS

A Record (IPv4 Address)

dig google.com A


---

📡 Check Reverse Lookup Using dig

dig -x 8.8.8.8


---

🧠 What I Learned

How DNS resolves domain names

How to troubleshoot DNS issues

Using nslookup for quick DNS queries

Using dig for detailed DNS information

Checking DNS records like A, MX, and NS



---

🔥 Why DNS Tools Are Important

Diagnose network connectivity issues

Verify DNS records

Troubleshoot domain resolution

Validate server configurations



---

📌 Commit Message

Day 37 - Added DNS tools documentation (nslookup and dig)

---

# 💼 LinkedIn Post – Day 37

🚀 **Day 37 – DNS Tools (nslookup & dig)**

Today I explored how Linux systems resolve domain names using DNS.

### 📌 What I Practiced:

🌐 Resolving domain names using `nslookup`  
🔎 Performing reverse DNS lookup  
📡 Querying specific DNS servers  
⚡ Using `dig` for detailed DNS analysis  
📬 Checking DNS records like A, MX, and NS  

Understanding DNS troubleshooting is essential for networking and infrastructure management.

All commands and notes are documented in my GitHub repository.

🔗 GitHub:  
https://github.com/uday-999/linux-fundamentals  

#Linux #DevOps #SystemAdmin #Networking #DNS #100DaysOfCode #LearningInPublic  

---

Next (Day 38) we’ll cover:

⏰ **Time Synchronization – NTP & Chrony**

This is another **important system administration topic**.ols-nslookup-dig.md


---

📄 Proper GitHub Repo Code (Day 37)

# 🚀 Day 37 – DNS Tools (nslookup & dig)

## 📌 Overview

DNS (Domain Name System) converts domain names into IP addresses.

Example:

google.com → 142.250.183.110

Linux provides tools like **nslookup** and **dig** to troubleshoot DNS issues.

---

# 🌐 1️⃣ nslookup Command

nslookup is used to query DNS servers and resolve domain names.

## Basic DNS Lookup

```bash
nslookup google.com

Output shows:

DNS server

IP address of the domain



---

Query Specific DNS Server

nslookup google.com 8.8.8.8

This queries Google's public DNS server.


---

Reverse DNS Lookup

Find domain name from IP address.

nslookup 8.8.8.8


---

🔎 2️⃣ dig Command (Domain Information Groper)

dig provides detailed DNS information.

Basic Lookup

dig google.com

Shows:

DNS response

Query time

DNS server used

Answer section



---

Short Output

dig google.com +short

Returns only the IP address.


---

Query Specific Record Type

MX Record (Mail Server)

dig google.com MX

NS Record (Name Servers)

dig google.com NS

A Record (IPv4 Address)

dig google.com A


---

📡 Check Reverse Lookup Using dig

dig -x 8.8.8.8


---

🧠 What I Learned

How DNS resolves domain names

How to troubleshoot DNS issues

Using nslookup for quick DNS queries

Using dig for detailed DNS information

Checking DNS records like A, MX, and NS



---

🔥 Why DNS Tools Are Important

Diagnose network connectivity issues

Verify DNS records

Troubleshoot domain resolution

Validate server configurations



---

📌 Commit Message

Day 37 - Added DNS tools documentation (nslookup and dig)

---

# 💼 LinkedIn Post – Day 37

🚀 **Day 37 – DNS Tools (nslookup & dig)**

Today I explored how Linux systems resolve domain names using DNS.

### 📌 What I Practiced:

🌐 Resolving domain names using `nslookup`  
🔎 Performing reverse DNS lookup  
📡 Querying specific DNS servers  
⚡ Using `dig` for detailed DNS analysis  
📬 Checking DNS records like A, MX, and NS  

Understanding DNS troubleshooting is essential for networking and infrastructure management.

All commands and notes are documented in my GitHub repository.

🔗 GitHub:  
https://github.com/uday-999/linux-fundamentals  

#Linux #DevOps #SystemAdmin #Networking #DNS #100DaysOfCode #LearningInPublic  

---

Next (Day 38) we’ll cover:

⏰ **Time Synchronization – NTP & Chrony**

This is another **important system administration topic**.
