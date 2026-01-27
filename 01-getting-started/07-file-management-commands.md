# 🗂️ Day 07: Essential Linux File Management Commands

Mastering file operations is fundamental to Linux administration. Today I focused on the commands you'll use daily for organizing, manipulating, and maintaining files and directories.

---

## 📊 **Command Overview & Use Cases**

| Command | Primary Use | Key Options | Risk Level |
|---------|------------|-------------|------------|
| `mkdir` | Create directories | `-p` (parents) | Low |
| `touch` | Create empty files | None | Low |
| `cp` | Copy files/directories | `-r` (recursive) | Medium |
| `mv` | Move/rename items | None | High |
| `rm` | Delete files/directories | `-r` (recursive), `-f` (force) | ⚠️ **DANGER** |
| `rmdir` | Delete empty directories | None | Low |

---

## 🛠️ **Detailed Commands with Practical Examples**

### 1. **Creating Directories (`mkdir`)**
```bash
# Single directory
mkdir projects

# Multiple directories at once
mkdir docs images scripts

# Nested directory structure (creates parents if missing)
mkdir -p projects/web/{css,js,images}

# Verify creation
ls -la projects/
```

**Pro Tip:** Use `-p` flag to avoid "directory exists" errors in scripts.

### 2. **Creating Files (`touch`)**
```bash
# Single file
touch notes.txt

# Multiple files
touch file1.txt file2.txt file3.log

# Update file timestamp (without modifying content)
touch notes.txt  # Updates access/modification time
```

### 3. **Copying Files & Directories (`cp`)**
```bash
# Basic file copy
cp source.txt destination.txt

# Copy with preservation (timestamps, permissions)
cp -p config.ini config.ini.backup

# Copy directory recursively (with all contents)
cp -r /var/log/nginx/ ~/nginx-backup/

# Interactive copy (prompts before overwrite)
cp -i important.txt backup/

# Verbose copy (shows what's being copied)
cp -rv source/ destination/
```

### 4. **Moving & Renaming (`mv`)**
```bash
# Rename a file
mv old_filename.txt new_filename.txt

# Move file to directory
mv document.pdf ~/Documents/

# Move multiple files
mv *.jpg ~/Pictures/

# Interactive move (prompt before overwrite)
mv -i important_data.csv backups/

# Rename directory
mv old_directory_name/ new_directory_name/
```

**Key Insight:** `mv` works the same for files and directories—no `-r` flag needed!

### 5. **Safe Deletion Practices**
```bash
# Delete single file (with confirmation prompt)
rm -i unwanted_file.log

# Delete multiple matching files
rm *.tmp

# Delete empty directory
rmdir empty_folder/

# Delete directory with contents (RECURSIVE - USE CAUTION!)
rm -r old_project/

# Force delete without prompts (EXTREME CAUTION!)
rm -rf node_modules/  # ⚠️ NO UNDO!
```

### 6. **Safety First: Aliases for Protection**
Add to `~/.bashrc` or `~/.bash_aliases`:
```bash
# Make rm interactive by default
alias rm='rm -i'

# Create trash function instead of permanent delete
trash() { mv "$@" ~/.trash/; }
alias del='trash'

# Confirm before removing recursively
alias rmr='rm -ri'
```

---

## 🎯 **Comprehensive Practice Lab**

### **Scenario:** Organize a messy project directory

```bash
# 1. Create a chaotic workspace
mkdir -p messy_project/
cd messy_project
touch {index,main,app}.{js,css,html}
touch {temp1,temp2,backup,old}.tmp
touch README.md LICENSE.txt

# 2. Create organized structure
mkdir -p {src/{js,css},docs,backup}

# 3. Move files to correct locations
mv *.js src/js/
mv *.css src/css/
mv *.html src/
mv *.tmp backup/
mv README.md LICENSE.txt docs/

# 4. Create proper documentation
touch docs/{CHANGELOG,CONTRIBUTING}.md

# 5. Make a backup
cp -r ../messy_project ../project_backup

# 6. Verify structure
tree .  # or: find . -type f | sort

# 7. Cleanup practice (when ready)
cd ..
rm -ri messy_project/  # Interactive delete
# OR keep backup: rm -rf messy_project/
```

### **Challenge Tasks:**
1. Create a directory structure for a web app with 3 levels of nesting
2. Copy all `.log` files to an `archive/` directory
3. Rename all `.txt` files to have `.md` extension
4. Safely remove all `.tmp` files with confirmation

---

## ⚠️ **Critical Safety Guidelines**

### **NEVER RUN THESE (Seriously!):**
```bash
rm -rf /              # Deletes ENTIRE system
rm -rf ~              # Deletes your home directory
rm -rf *              # Deletes everything in current directory
rm -rf .              # Deletes current directory and parents!
```

### **Safe Alternatives:**
1. **Use `trash-cli`** instead of `rm`:
   ```bash
   sudo apt install trash-cli    # Debian/Ubuntu
   sudo yum install trash-cli    # RHEL/CentOS
   trash-put file.txt            # Moves to trash
   trash-list                    # View trashed items
   trash-restore                 # Restore from trash
   ```

2. **Test with `echo` first:**
   ```bash
   echo rm -rf *.log   # Shows what WOULD be deleted
   ```

3. **Use `-i` (interactive) flag:**
   ```bash
   rm -i *.tmp         # Asks before each deletion
   ```

---

## 📚 **Summary & Best Practices**

✅ **Created** files (`touch`) and directories (`mkdir -p`)  
✅ **Copied** items using `cp` with `-r` for directories  
✅ **Moved/Renamed** with `mv` (same command for both)  
✅ **Deleted** safely with `rm -i` and avoided `rm -rf /`  
✅ **Practiced** organization with real-world scenario  

**Golden Rules:**  
1. Always double-check paths before deleting  
2. Use `-i` flag when learning or in important directories  
3. Regular backups are your best friend  
4. When in doubt, `mv` to a backup folder instead of `rm`

---

## 🔮 **Next Up: Day 08**
File viewing & searching: `cat`, `less`, `head`, `tail`, and `grep` for powerful text manipulation!

---

**💡 Remember:** With great power (`rm -rf`) comes great responsibility. Practice in safe environments!**

---
*Part of the #LinuxLearning journey. Always test commands in non-production environments first!*
