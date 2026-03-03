
## 📌 Overview
System updates and patch management are critical responsibilities of a Linux System Administrator.

Regular updates help:
- Fix security vulnerabilities
- Improve system performance
- Maintain software compatibility
- Ensure production stability

---

# 🔎 1️⃣ Check Available Updates

## Using YUM

```bash
yum check-update

Using DNF

dnf check-update


---

⬆ 2️⃣ Update All Packages

YUM

yum update

DNF

dnf update


---

🔐 3️⃣ Install Security Updates Only

YUM

yum update --security

DNF

dnf update --security

This installs only security-related patches.


---

📜 4️⃣ View Update History

yum history

View details of a specific transaction:

yum history info <ID>


---

🔁 5️⃣ Rollback an Update

yum history undo <ID>

This reverts a specific update transaction.


---

🧩 6️⃣ Check Installed Kernel Version

uname -r

After a kernel update, reboot may be required:

reboot


---

⚠ Best Practices for Patch Management

Take backups before major updates

Test updates in staging environments

Apply security patches promptly

Reboot after kernel updates

Monitor system after applying updates



---

🧠 What I Learned

How to check for available updates

How to update the system safely

How to apply security-only patches

How to view update history

How to rollback updates if needed



---

🔥 Why Patch Management Matters

Prevents security breaches

Protects against vulnerabilities

Maintains compliance

Ensures long-term system stability
