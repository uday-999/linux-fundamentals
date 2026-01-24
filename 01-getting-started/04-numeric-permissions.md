# Day 04 - Numeric Permissions (chmod 755 / 644) 🔢🔐🐧

Today I learned how **numeric permissions** work in Linux and how to use them with `chmod`
to set correct access for files and folders.

---

## ✅ 1) Why Numeric Permissions?

Numeric permissions are a faster way to set permissions instead of symbolic mode.

Example:

```bash
chmod 755 script.sh
````

This is easier than writing:

```bash
chmod u=rwx,g=rx,o=rx script.sh
```

---

## ✅ 2) Permission Values (r, w, x)

Each permission has a number:

| Permission | Meaning | Value |
| ---------- | ------- | ----- |
| r          | read    | 4     |
| w          | write   | 2     |
| x          | execute | 1     |

So we calculate permissions by adding values:

✅ `rwx` = 4 + 2 + 1 = **7**
✅ `rw-` = 4 + 2 + 0 = **6**
✅ `r-x` = 4 + 0 + 1 = **5**
✅ `r--` = 4 + 0 + 0 = **4**
✅ `---` = 0 + 0 + 0 = **0**

---

## ✅ 3) Understanding 3 Digits (Owner / Group / Others)

Numeric permissions have 3 digits:

```txt
chmod XYZ file
```

| Digit | Applies to   |
| ----- | ------------ |
| X     | user (owner) |
| Y     | group        |
| Z     | others       |

Example:

```bash
chmod 754 demo.txt
```

Means:

* user = 7 = rwx
* group = 5 = r-x
* others = 4 = r--

---

## ✅ 4) Most Common Permissions (Real Life)

### 🔹 644 (Files)

```bash
chmod 644 notes.txt
```

Meaning:

* user: rw-
* group: r--
* others: r--

✅ Used for: text files, config files, read-only sharing

---

### 🔹 755 (Folders / Scripts)

```bash
chmod 755 myfolder
```

Meaning:

* user: rwx
* group: r-x
* others: r-x

✅ Used for: directories, scripts, public readable folders

---

### 🔹 700 (Private)

```bash
chmod 700 secret.txt
```

Meaning:

* user: rwx
* group: ---
* others: ---

✅ Used for: private files, SSH keys, sensitive files

---

### 🔹 600 (Very Private File)

```bash
chmod 600 private.key
```

Meaning:

* user: rw-
* group: ---
* others: ---

✅ Used for: private keys, credentials

---

## ✅ 5) Quick Examples (Try on Amazon Linux)

### 1) Create a file

```bash
touch notes.txt
```

### 2) Set file permission to 644

```bash
chmod 644 notes.txt
```

### 3) Check permission

```bash
ls -l notes.txt
```

Expected output style:

```txt
-rw-r--r-- 1 ec2-user ec2-user 0 Jan 23 notes.txt
```

---

## ✅ 6) Mini Task (Hands-on Practice)

### 🔹 Step 1: Create a script

```bash
touch run.sh
```

### 🔹 Step 2: Add a simple command

```bash
echo "echo Hello from Linux" > run.sh
```

### 🔹 Step 3: Give execute permission using numeric method

```bash
chmod 755 run.sh
```

### 🔹 Step 4: Run it

```bash
./run.sh
```

---

## ✅ Summary

✅ Learned numeric permissions using values (**4, 2, 1**)
✅ Understood common permissions (**644, 755, 700, 600**)
✅ Practiced `chmod` numeric mode on Amazon Linux

📌 Next Day: Ownership & Groups (`chown`, `chgrp`) 🔥
