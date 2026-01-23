# Day 03 - Linux Permissions + chmod 🔐🐧

Today I learned how Linux permissions work and how to change them using `chmod`.

---

## ✅ 1) What are Linux Permissions?

Linux controls access to files and folders using 3 permissions:

- **r** = read  
- **w** = write  
- **x** = execute  

---

## ✅ 2) Permission Groups (Who gets permissions?)

Permissions are set for:

- **u** = user (owner)  
- **g** = group  
- **o** = others (everyone else)  

---

## ✅ 3) Checking Permissions with `ls -l`

```bash
ls -l
```

Example output:

```txt
-rw-r--r-- 1 ec2-user ec2-user 120 Jan 22 notes.txt
drwxr-xr-x 2 ec2-user ec2-user 4096 Jan 22 scripts
```

### Breakdown (Example: `-rw-r--r--`)

* First character:

  * `-` = file
  * `d` = directory

Then permissions are in 3 parts:

| Part  | Meaning            |
| ----- | ------------------ |
| `rw-` | user permissions   |
| `r--` | group permissions  |
| `r--` | others permissions |

---

## ✅ 4) Meaning of Common Permission Patterns

| Permission | Meaning                |
| ---------- | ---------------------- |
| `r--`      | read only              |
| `rw-`      | read + write           |
| `r-x`      | read + execute         |
| `rwx`      | read + write + execute |

---

## ✅ 5) Changing Permissions using `chmod` (Symbolic Method)

### ➕ Add execute permission to user

```bash
chmod u+x script.sh
```

### ➖ Remove write permission from group

```bash
chmod g-w file.txt
```

### ➕ Give read permission to others

```bash
chmod o+r file.txt
```

### ➕ Give execute permission to everyone

```bash
chmod a+x script.sh
```

---

## 🎯 Mini Task (Try on Amazon Linux)

### 1) Create a file named `run.sh`

```bash
touch run.sh
```

### 2) Give execute permission

```bash
chmod u+x run.sh
```

### 3) Verify permissions

```bash
ls -l run.sh
```

---

## ✅ Summary

✅ Learned permission groups (**user, group, others**)
✅ Learned permissions (**r, w, x**)
✅ Practiced `ls -l` and `chmod` symbolic method

📌 Next Day: Numeric permissions (**755, 644**) + real examples 🔥
