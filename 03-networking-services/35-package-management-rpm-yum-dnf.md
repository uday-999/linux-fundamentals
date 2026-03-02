Perfect 🔥
Here is your proper, clean, structured, portfolio-level GitHub code for

🚀 Day 34 – Package Management (RPM, YUM & DNF)

Create this file:

section-06-networking-services/09-package-management-rpm-yum-dnf.md


---

📄 09-package-management-rpm-yum-dnf.md

# 🚀 Day 34 – Linux Package Management (RPM, YUM & DNF)

## 📌 Overview

Package management in Linux is responsible for installing, updating, removing, and verifying software packages.

In this section, I explored:

- RPM (Red Hat Package Manager)
- YUM (Yellowdog Updater Modified)
- DNF (Dandified YUM)

Understanding package management is essential for maintaining secure and stable Linux systems.

---

# 📦 1️⃣ RPM – Red Hat Package Manager

RPM manages `.rpm` packages directly without resolving dependencies automatically.

## Install a Package

```bash
rpm -ivh package-name.rpm

Options:

-i → Install

-v → Verbose

-h → Show progress



---

Remove a Package

rpm -e package-name


---

Query Installed Package

rpm -q package-name


---

List All Installed Packages

rpm -qa


---

View Package Information

rpm -qi package-name


---

List Files Installed by a Package

rpm -ql package-name


---

Verify Package Integrity

rpm -V package-name

This checks if files were modified after installation.


---

🔄 2️⃣ YUM – Dependency-Based Package Manager

YUM automatically resolves dependencies while installing packages.

Install a Package

yum install package-name


---

Remove a Package

yum remove package-name


---

Update All Packages

yum update


---

Check Available Updates

yum check-update


---

View Enabled Repositories

yum repolist


---

⚡ 3️⃣ DNF – Modern Replacement for YUM

DNF is the next-generation version of YUM with improved performance and dependency handling.

Install a Package

dnf install package-name


---

Remove a Package

dnf remove package-name


---

Update System

dnf update


---

List Repositories

dnf repolist


---

Check Package Dependencies

dnf repoquery --requires package-name


---

🔐 Security & Best Practices

Regularly update system packages

Apply security patches promptly

Verify package integrity

Use official repositories only

Avoid installing unsigned packages



---

🧠 What I Learned

Difference between RPM, YUM, and DNF

How dependency resolution works

Installing and removing packages safely

Checking repositories and updates

Verifying installed packages



---

🔥 Why Package Management Matters

Maintains system security

Ensures software compatibility

Prevents dependency conflicts

Critical for production server stability



---

📌 Commit Message

Day 34 - Added detailed documentation on RPM, YUM, and DNF package management

---

Now this is:

✅ Clean  
✅ Professional  
✅ Structured  
✅ Recruiter-friendly  
✅ DevOps-ready  

---

Ready for **Day 35 – System Updates & Patch Management (Security Updates, Rollback, History)**? 🔥
