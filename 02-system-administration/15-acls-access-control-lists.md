# 🐧 Day 15 – Access Control Lists (ACLs)
## Fine-Grained Permission Management in Linux

> **TL;DR**: Traditional `rwx` permissions limit you to one owner, one group, and "others."  
> ACLs extend this model, allowing precise permission control for multiple users and groups — without changing ownership.

ACLs are widely used in enterprise Linux environments where flexibility and controlled collaboration are required.

---

# 🎯 Why ACLs Matter

### Traditional UNIX Permission Model
- 1 Owner  
- 1 Group  
- Others  

### Real-World Problems
- Grant access to one specific user without changing the file’s group
- Allow multiple teams different access levels
- Provide temporary access to contractors
- Avoid creating unnecessary groups

**ACLs solve these limitations cleanly.**

---

# 📦 1️⃣ Prerequisites & Installation (Amazon Linux)

### Verify ACL Utilities
```bash
rpm -qa | grep -i acl
```

### Install (if missing)
```bash
sudo yum install -y acl
```

### Verify Filesystem Supports ACL
```bash
mount | grep -E "(acl|user_xattr)"
```

### Enable ACL (Temporary)
```bash
sudo mount -o remount,acl /
```

### Enable ACL (Permanent – `/etc/fstab`)
```
/dev/xvda1 / ext4 defaults,acl 0 0
```

---

# 📄 2️⃣ Viewing ACLs – `getfacl`

```bash
getfacl file.txt
```

### Example Output
```
# file: report.pdf
# owner: ec2-user
# group: developers
user::rw-
user:alice:rwx
group::r--
group:contractors:r-x
mask::rwx
other::r--
```

### Key Components

| Component | Meaning |
|------------|----------|
| `user::` | File owner permissions |
| `user:alice:` | Named user ACL entry |
| `group::` | Owning group permissions |
| `group:contractors:` | Named group ACL entry |
| `mask::` | Maximum effective permission |
| `other::` | Everyone else |

---

# ✏️ 3️⃣ Setting ACLs – `setfacl`

### Grant User Access
```bash
setfacl -m u:alice:rwx file.txt
```

### Multiple Users
```bash
setfacl -m u:bob:rw,u:charlie:r file.txt
```

### Grant Group Access
```bash
setfacl -m g:developers:rwx /shared/
```

### Apply ACL from File
```bash
setfacl -M acl_entries.txt project/
```

---

# 📁 4️⃣ Directory ACLs & Inheritance

### Recursive Apply
```bash
setfacl -R -m g:developers:rwx /project/
```

### Default ACL (Inheritance)
```bash
setfacl -d -m u:alice:rwx /shared/
```

Verify:
```bash
getfacl /shared/ | grep default
```

Default ACL ensures new files inherit defined permissions.

---

# 🧹 5️⃣ Removing ACLs

### Remove Specific Entry
```bash
setfacl -x u:alice file.txt
```

### Remove All Extended ACLs
```bash
setfacl -b file.txt
```

### Remove Default ACL Only
```bash
setfacl -k directory/
```

---

# 🔐 6️⃣ Understanding the Mask

The **mask** defines the maximum effective permissions for:

- Named users
- Named groups
- Owning group

### Example
```bash
setfacl -m u:alice:rwx file.txt
setfacl -m m::r-- file.txt
```

Result:
Alice gets **r-- only**, even though `rwx` was granted.

Always verify mask when troubleshooting.

---

# 🧪 7️⃣ Practical Enterprise Lab

### Scenario
Directory: `/shared/project-alpha/`

Requirements:
- Developers → `rwx`
- QA users → `r-x`
- Contractor Alice → `rw-`
- All new files inherit ACLs

### Implementation
```bash
sudo mkdir -p /shared/project-alpha
sudo chown root:developers /shared/project-alpha
sudo chmod 775 /shared/project-alpha

sudo setfacl -m g:developers:rwx /shared/project-alpha
sudo setfacl -m u:alice:rw- /shared/project-alpha
sudo setfacl -m u:qa1:r-x,u:qa2:r-x /shared/project-alpha

sudo setfacl -d -m g:developers:rwx /shared/project-alpha
sudo setfacl -d -m u:alice:rw- /shared/project-alpha
```

Verify:
```bash
getfacl /shared/project-alpha
ls -ld /shared/project-alpha
```

Look for `+` in `ls -l` output:
```
drwxrwxr-x+
```

---

# 🏢 8️⃣ Enterprise Use Cases

| Scenario | ACL Solution |
|------------|----------------|
| Web App Deployment | App user `rwx`, web server `r-x` |
| Database Backup | Backup user `r-x` only |
| Shared Dev Directory | Multi-team permission separation |
| Temporary Contractor | Time-limited named user ACL |

---

# ⚠️ 9️⃣ Troubleshooting

### ACL Not Working?
```bash
mount | grep acl
```

### Permissions Not Applied?
Check mask:
```bash
getfacl file.txt | grep mask
```

### ACL Not Inherited?
Ensure default ACL exists:
```bash
getfacl dir/ | grep default
```

---

# 📊 10️⃣ ACL vs Traditional Permissions

| Feature | chmod | ACL |
|-----------|--------|------|
| Multiple users | ❌ | ✅ |
| Multiple groups | ❌ | ✅ |
| Fine-grained control | Basic | Advanced |
| Inheritance control | Limited | Full (default ACL) |
| Enterprise usage | Moderate | Very Common |

---

# 📌 Key Takeaways

- ACL extends traditional UNIX permissions
- `getfacl` inspects ACL entries
- `setfacl` modifies ACL entries
- Mask controls effective permissions
- Default ACL enables inheritance
- `+` in `ls -l` indicates ACL presence
- Regular audits prevent permission sprawl

---

# 🚀 Next

**Day 16 – Package Management (yum / dnf)**  
Managing software lifecycle in Amazon Linux production systems.

---
