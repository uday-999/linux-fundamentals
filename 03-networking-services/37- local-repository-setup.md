## 📌 Overview

A Local Repository allows Linux systems to install packages from a local source instead of downloading them from online repositories.

Local repositories are commonly used in:

- Enterprise environments
- Offline servers
- Secure internal networks
- Controlled package distribution

---

# 📦 1️⃣ Install Required Tool

To create a repository, install the `createrepo` package.

### RHEL / CentOS / Rocky / AlmaLinux

```bash
yum install createrepo

or

dnf install createrepo


---

📁 2️⃣ Create Repository Directory

Create a directory where RPM packages will be stored.

mkdir /localrepo

Copy RPM packages into the directory.

Example:

cp *.rpm /localrepo


---

🔧 3️⃣ Generate Repository Metadata

Create repository metadata using createrepo.

createrepo /localrepo

This generates a repodata directory required for repository functionality.


---

📝 4️⃣ Create Repository Configuration File

Create a repository configuration file.

vi /etc/yum.repos.d/local.repo

Add the following configuration:

[localrepo]
name=Local Repository
baseurl=file:///localrepo
enabled=1
gpgcheck=0

Explanation:

name → Repository name

baseurl → Path to repository directory

enabled → Enable the repository

gpgcheck → Disable GPG verification



---

🔄 5️⃣ Refresh Repository Cache

Refresh repository metadata.

yum clean all
yum repolist

or

dnf clean all
dnf repolist


---

📦 6️⃣ Install Package From Local Repository

Example:

yum install package-name

or

dnf install package-name

The package will now be installed from the local repository.


---

🧠 What I Learned

How to create a local package repository

How repository metadata works

How to configure custom repo files

How to install packages from a local source



---

🔥 Why Local Repositories Are Important

Enables offline package installation

Reduces dependency on internet repositories

Improves package management control

Used widely in enterprise infrastructure
