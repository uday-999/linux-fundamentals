# 🐧 Day 08: File Viewing & Searching (cat, less, head, tail, grep)

Today I learned how to **view, inspect, and search files efficiently** in Linux.
These commands are essential for **log analysis, debugging, and system monitoring**
on servers (Amazon Linux / AWS / production systems).

---

## 📄 1) Display File Content (`cat`)

### Basic usage
```bash
cat file.txt
cat -n file.txt        # show line numbers
cat -A file.txt        # show hidden characters
````

### Useful examples

```bash
cat file1.txt file2.txt        # concatenate files
cat file.txt | wc -l           # count lines
```

⚠️ **Warning:** Avoid using `cat` on very large files or logs.

---

## 📖 2) Interactive File Viewing (`less`)

```bash
less file.log
less +F file.log       # follow mode (like tail -f)
```

### Common shortcuts

* `Space` → next page
* `b` → previous page
* `/word` → search forward
* `n` → next match
* `q` → quit

📌 Best choice for **large files and logs**.

---

## 🔝 3) View Beginning of a File (`head`)

```bash
head file.txt
head -n 20 file.txt
head -c 500 file.txt
```

### Real use cases

```bash
head -n 1 data.csv                 # check header
head -20 /etc/nginx/nginx.conf     # verify config start
```

---

## 🔽 4) View End of a File (`tail`)

```bash
tail file.log
tail -n 50 file.log
```

### Live log monitoring

```bash
tail -f /var/log/messages
tail -F app.log        # handles log rotation
```

📌 Used heavily for **debugging running services**.

---

## 🔍 5) Search Inside Files (`grep`)

### Common patterns

```bash
grep "error" app.log
grep -i "timeout" app.log
grep -n "404" access.log
grep -v "INFO" app.log
grep -c "ERROR" app.log
```

### Recursive search

```bash
grep -R "DB_HOST" /etc
```

### Context & advanced usage

```bash
grep -n -B2 -A2 "error" app.log
grep -E "error|fail|critical" app.log
```

---

## ⚡ 6) Command Combinations (Real Power)

```bash
# Live monitoring with filtering
tail -f app.log | grep -i error

# Top 10 frequent errors
grep "ERROR" app.log | sort | uniq -c | sort -nr | head -10

# Search inside compressed logs
zgrep "exception" app.log.1.gz
```

---

## 🎯 Mini Practice Task

```bash
touch demo.log
echo "INFO App started" >> demo.log
echo "ERROR Database failed" >> demo.log
echo "INFO Retrying connection" >> demo.log

cat demo.log
grep ERROR demo.log
tail -f demo.log
```

---

## 📚 Summary

✅ Viewed files using `cat` and `less`

✅ Checked file start/end using `head` and `tail`

✅ Monitored logs using `tail -f`

✅ Searched content using `grep`

✅ Combined commands for real-world troubleshooting

---

## 🔮 Next Day

**Day 09:** Input/Output Redirection & Pipes
(`>`, `>>`, `<`, `2>`, `|`, `tee`) 🔥
