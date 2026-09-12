# Linux Commands

## Objective

Learn basic Linux command-line commands used for navigation, file management, searching, and system information.

---

## 1. What is Linux CLI?

CLI stands for **Command Line Interface**.

It allows us to interact with a Linux system by entering commands in the terminal.

Linux CLI is commonly used in DevOps for:

* Managing servers
* Managing files and directories
* Installing software
* Checking system resources
* Managing services
* Troubleshooting applications
* Working with cloud servers

---

## 2. Navigation Commands

| Command  | Purpose                     | Example   |
| -------- | --------------------------- | --------- |
| `pwd`    | Shows current directory     | `pwd`     |
| `ls`     | Lists files and directories | `ls`      |
| `ls -l`  | Shows detailed listing      | `ls -l`   |
| `ls -la` | Shows hidden files also     | `ls -la`  |
| `cd`     | Changes directory           | `cd /etc` |
| `cd ..`  | Moves to parent directory   | `cd ..`   |
| `cd ~`   | Goes to home directory      | `cd ~`    |

### Examples

```bash
pwd
ls
cd /etc
pwd
cd ..
pwd
```

---

## 3. Create Files and Directories

### Create a directory

```bash
mkdir devops
```

### Create multiple directories

```bash
mkdir linux networking cloud
```

### Create nested directories

```bash
mkdir -p project/app/logs
```

### Create an empty file

```bash
touch notes.txt
```

### Create multiple files

```bash
touch file1.txt file2.txt file3.txt
```

---

## 4. View File Contents

### `cat`

Displays the complete contents of a file.

```bash
cat notes.txt
```

### `less`

Used to view large files page by page.

```bash
less notes.txt
```

Press `q` to exit.

### `head`

Shows the first lines of a file.

```bash
head notes.txt
```

### `tail`

Shows the last lines of a file.

```bash
tail notes.txt
```

### `tail -f`

Continuously watches a file for new entries.

```bash
tail -f app.log
```

This is commonly used for checking application logs.

---

## 5. File Management Commands

### Copy a file

```bash
cp notes.txt backup.txt
```

### Copy a directory

```bash
cp -r project project-backup
```

### Move or rename a file

```bash
mv notes.txt new-notes.txt
```

### Move a file to another directory

```bash
mv notes.txt /tmp/
```

### Remove a file

```bash
rm notes.txt
```

### Remove a directory

```bash
rm -r project
```

> Be careful with `rm` because deleted files may not be easily recoverable.

---

## 6. Write Data to Files

### Using `echo`

```bash
echo "Hello Linux" > notes.txt
```

`>` creates or overwrites the file.

### Append data

```bash
echo "DevOps Learning" >> notes.txt
```

`>>` adds data to the existing file.

### View the file

```bash
cat notes.txt
```

---

## 7. Search Commands

### `grep`

Searches for text inside files.

```bash
grep "Linux" notes.txt
```

### Case-insensitive search

```bash
grep -i "linux" notes.txt
```

### Search recursively inside a directory

```bash
grep -r "error" /var/log
```

### `find`

Searches for files and directories.

```bash
find . -name "notes.txt"
```

Search for all `.log` files:

```bash
find . -name "*.log"
```

---

## 8. Useful Text Commands

### `wc`

Counts lines, words, and characters.

```bash
wc notes.txt
```

Count only lines:

```bash
wc -l notes.txt
```

### `sort`

Sorts lines alphabetically.

```bash
sort names.txt
```

### `history`

Shows previously executed commands.

```bash
history
```

---

## 9. System Information Commands

### Check current user

```bash
whoami
```

### Show user and group information

```bash
id
```

### Show hostname

```bash
hostname
```

### Show Linux system information

```bash
uname -a
```

### Check disk space

```bash
df -h
```

### Check directory size

```bash
du -sh /var/log
```

### Check memory usage

```bash
free -h
```

### Check system uptime

```bash
uptime
```

---

## 10. Useful Commands for DevOps

### Check running processes

```bash
ps aux
```

### Check network listening ports

```bash
ss -tuln
```

### Check current environment variables

```bash
env
```

### Check a specific environment variable

```bash
echo $PATH
```

### Clear the terminal

```bash
clear
```

---

## 11. Practice

Create a practice directory:

```bash
mkdir linux-practice
cd linux-practice
```

Create files:

```bash
touch app.txt config.txt log.txt
```

Add some content:

```bash
echo "Linux DevOps Practice" > app.txt
echo "server configuration" > config.txt
echo "ERROR: application failed" > log.txt
```

View the files:

```bash
ls -l
cat app.txt
cat config.txt
cat log.txt
```

Search for `ERROR`:

```bash
grep "ERROR" log.txt
```

Check the current directory:

```bash
pwd
```

Check disk space:

```bash
df -h
```

Return to the previous directory:

```bash
cd ..
```

---

## 12. Verification

Run the following commands:

```bash
pwd
ls -la
whoami
uname -a
df -h
free -h
```

If these commands execute successfully, the basic Linux CLI practice is complete.

---

## Key Takeaways

| Command    | Purpose                                       |
| ---------- | --------------------------------------------- |
| `pwd`      | Shows current directory                       |
| `ls`       | Lists files and directories                   |
| `cd`       | Changes directory                             |
| `mkdir`    | Creates a directory                           |
| `touch`    | Creates a file                                |
| `cp`       | Copies files/directories                      |
| `mv`       | Moves or renames files                        |
| `rm`       | Removes files/directories                     |
| `cat`      | Displays file contents                        |
| `head`     | Shows beginning of a file                     |
| `tail`     | Shows end of a file                           |
| `grep`     | Searches text                                 |
| `find`     | Searches files/directories                    |
| `wc`       | Counts lines, words, and characters           |
| `df -h`    | Checks disk usage                             |
| `free -h`  | Checks memory                                 |
| `ps`       | Shows running processes                       |
| `ss`       | Shows network connections and listening ports |
| `history`  | Shows previous commands                       |
| `whoami`   | Shows current user                            |
| `uname -a` | Shows system information                      |

---

## Conclusion

Linux command-line skills are fundamental for DevOps engineers because most cloud servers and production environments are managed through the terminal.

The commands learned in this lab provide the foundation for working with Linux servers, troubleshooting applications, managing files, checking system resources, and operating cloud infrastructure.
