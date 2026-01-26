# 🐧 Day 06: Linux Users & Groups Management

Understanding user and group management is foundational for system administration, server security, and effective team collaboration in any Linux environment (including AWS EC2 / Amazon Linux).

---

## 📖 Core Concepts

- **Users (`/etc/passwd`)**: Unique accounts on the system. Each user has a UID.
- **Groups (`/etc/group`)**: Collections of users. Helps manage permissions easily.
- **Primary Group**: A user's default group.
- **Supplementary Groups**: Extra groups a user belongs to for additional access.

---

## 🛠️ Essential Commands & Examples

## 1) Check Current User & Identity

```bash
whoami
id
````

Example output:

```txt
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user),10(wheel),100(developers)
```

---

## 2) Explore System Files

```bash
cat /etc/passwd
cat /etc/group
```

📌 `/etc/passwd` format:

```txt
username:x:UID:GID:comment:home:shell
```

---

## 3) User Management

### Create a new user (with home directory)

```bash
sudo useradd -m devuser
```

### Set or change password

```bash
sudo passwd devuser
```

### Delete a user

```bash
sudo userdel devuser
```

Delete user + home directory:

```bash
sudo userdel -r devuser
```

---

## 4) Group Management

### Create a new group

```bash
sudo groupadd developers
```

### Add user to a supplementary group

```bash
sudo usermod -aG developers devuser
```

📌 Meaning:

* `-a` = append (don’t remove existing groups)
* `-G` = add supplementary group(s)

### Change a user's primary group

```bash
sudo usermod -g developers devuser
```

### Delete a group (must be empty)

```bash
sudo groupdel developers
```

---

## 5) Verification Commands

```bash
groups devuser
id devuser
getent group developers
```

---

## 🎯 Hands-On Practice Task

**Objective:** Create a user `appuser`, add them to the `webadmins` group, and verify.

```bash
# 1) Create the user with a home directory
sudo useradd -m appuser
sudo passwd appuser

# 2) Create the group
sudo groupadd webadmins

# 3) Add user to the group
sudo usermod -aG webadmins appuser

# 4) Verify
id appuser
groups appuser
getent group webadmins
```

---

## ⚠️ Important Notes & Best Practices

* Use `sudo` for commands requiring root privileges.
* Always use `usermod -aG` to append a user to groups (without removing existing groups).
* Before deleting a user, check running processes:

  ```bash
  ps -u username
  ```
* Use `visudo` to safely manage admin privileges via `/etc/sudoers`.

---

## 📚 Summary & Key Takeaways

✅ Checked user identity using `whoami` and `id`
✅ Explored system files `/etc/passwd` and `/etc/group`
✅ Created and managed users using `useradd`, `passwd`, `userdel`
✅ Created and managed groups using `groupadd`, `groupdel`
✅ Assigned group membership using `usermod -aG`
✅ Verified configurations using `groups`, `id`, and `getent`

---

## 🔮 What's Next?

**Day 07:** File manipulation commands (`cp`, `mv`, `rm`, `mkdir`, `rmdir`) and best practices 🚀
