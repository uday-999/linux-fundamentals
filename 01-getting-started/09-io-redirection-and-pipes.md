# 🐧 Day 09: Input / Output Redirection & Pipes

Today I learned how Linux handles **input, output, and errors**, and how to connect
commands together using **redirection operators and pipes**.
This is where simple Linux commands turn into **powerful workflows**.

---

## 📥 1) Standard Streams in Linux

Every Linux command uses three streams:

| Stream | Description | Default |
|------|------------|---------|
| `stdin` (0) | Input | Keyboard |
| `stdout` (1) | Output | Terminal |
| `stderr` (2) | Errors | Terminal |

---

## 📤 2) Output Redirection (`>` and `>>`)

Redirect command output to a file.

```bash
echo "Hello Linux" > file.txt
echo "Another line" >> file.txt
````

* `>` overwrites the file
* `>>` appends to the file

---

## 📥 3) Input Redirection (`<`)

Read input from a file instead of the keyboard.

```bash
wc -l < file.txt
sort < names.txt
```

---

## 🚫 4) Error Redirection (`stderr`)

Redirect errors only:

```bash
ls nofile.txt 2> error.log
ls nofile.txt 2>> error.log
```

Discard errors:

```bash
ls nofile.txt 2> /dev/null
```

---

## 📦 5) Redirect Output and Errors Together

Redirect both `stdout` and `stderr` to the same file.

```bash
command > all.log 2>&1
```

Modern syntax:

```bash
command &> all.log
```

📌 **Order matters:**

```bash
command > output.log 2>&1   # correct
```

---

## 🔗 6) Pipes (`|`) – The Real Power

Pipes send the output of one command as input to another.

```bash
ls -l | less
ps aux | grep root
cat file.txt | wc -l
```

---

## ⚡ 7) Real-World Examples

### Live log filtering

```bash
tail -f app.log | grep ERROR
```

### Count errors in logs

```bash
grep ERROR app.log | wc -l
```

### Save filtered output

```bash
grep ERROR app.log > errors.txt
```

---

## 🧪 8) Mini Practice Task

```bash
touch demo.log
echo "INFO Server started" >> demo.log
echo "ERROR Database failed" >> demo.log
echo "INFO Retrying" >> demo.log

cat demo.log
grep ERROR demo.log
grep ERROR demo.log > error-only.txt
```

---

## 📚 Summary

✅ Learned `stdin`, `stdout`, and `stderr`
✅ Used output (`>`, `>>`) and input (`<`) redirection
✅ Handled errors using `2>`
✅ Connected commands using pipes (`|`)
✅ Built real log-filtering workflows

---

## 🔮 Next Day

**Day 10:** Process Management (`ps`, `top`, `kill`, `bg`, `fg`) 🔥
