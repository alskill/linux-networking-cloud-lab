# Linux File Permissions

## Objective

Learn how Linux controls access to files and directories using **permissions** and **ownership**.

---

## 1. What are Linux Permissions?

Linux permissions control who can:

* Read a file
* Modify a file
* Execute a file
* Access a directory

Permissions are mainly assigned to:

1. **Owner** — the user who owns the file
2. **Group** — users belonging to the file's group
3. **Others** — all other users

---

## 2. Check File Permissions

Use `ls -l`:

```bash
ls -l
```

Example output:

```text
-rwxr-xr-- 1 ubuntu developers 120 Sep 12 10:00 script.sh
```

The permission section is:

```text
-rwxr-xr--
```

It can be understood as:

```text
- rwx r-x r--
  --- --- ---
  Owner Group Others
```

---

## 3. Permission Types

| Permission | Symbol | Meaning          | Numeric Value |
| ---------- | ------ | ---------------- | ------------- |
| Read       | `r`    | Read the file    | `4`           |
| Write      | `w`    | Modify the file  | `2`           |
| Execute    | `x`    | Execute the file | `1`           |

### Read

Allows a user to view the contents of a file.

### Write

Allows a user to modify the contents of a file.

### Execute

Allows a user to execute a file or enter a directory when the appropriate directory permissions are present.

---

## 4. Permission Groups

Linux permissions are divided into three groups:

```text
Owner
Group
Others
```

For example:

```text
-rwxr-xr--
```

Means:

```text
Owner  → rwx
Group  → r-x
Others → r--
```

---

## 5. Numeric Permissions

Linux permissions can also be represented using numbers.

```text
r = 4
w = 2
x = 1
```

Add the values together:

```text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2 = 6
r-x = 4 + 1 = 5
r-- = 4
```

### Common Permission Values

| Number | Permission |
| ------ | ---------- |
| `7`    | `rwx`      |
| `6`    | `rw-`      |
| `5`    | `r-x`      |
| `4`    | `r--`      |
| `0`    | `---`      |

---

## 6. Common Permission Examples

### `755`

```text
Owner  → rwx
Group  → r-x
Others → r-x
```

Therefore:

```text
755 = rwxr-xr-x
```

### `644`

```text
Owner  → rw-
Group  → r--
Others → r--
```

Therefore:

```text
644 = rw-r--r--
```

### `700`

```text
Owner  → rwx
Group  → ---
Others → ---
```

Therefore:

```text
700 = rwx------
```

---

## 7. Change Permissions with chmod

`chmod` means **change mode**.

It is used to change file and directory permissions.

### Example

Create a file:

```bash
touch script.sh
```

Check its permissions:

```bash
ls -l script.sh
```

Give the owner execute permission:

```bash
chmod u+x script.sh
```

Check again:

```bash
ls -l script.sh
```

---

## 8. Numeric chmod

Set permissions to `755`:

```bash
chmod 755 script.sh
```

Set permissions to `644`:

```bash
chmod 644 script.sh
```

Set permissions to `700`:

```bash
chmod 700 script.sh
```

Check the result:

```bash
ls -l script.sh
```

---

## 9. Symbolic chmod

Linux also allows permissions to be changed using symbols.

### Owner

`u` = user/owner

### Group

`g` = group

### Others

`o` = others

### All

`a` = all users

---

### Add execute permission for owner

```bash
chmod u+x script.sh
```

### Remove write permission from group

```bash
chmod g-w script.sh
```

### Add read permission for others

```bash
chmod o+r script.sh
```

### Add execute permission for everyone

```bash
chmod a+x script.sh
```

---

## 10. Change File Ownership

`chown` means **change owner**.

Check current ownership:

```bash
ls -l script.sh
```

Change the owner:

```bash
sudo chown devops script.sh
```

Check again:

```bash
ls -l script.sh
```

---

## 11. Change Owner and Group

You can change both the owner and group:

```bash
sudo chown devops:developers script.sh
```

Verify:

```bash
ls -l script.sh
```

---

## 12. Change Group Ownership

Use `chgrp` to change the group.

```bash
sudo chgrp developers script.sh
```

Check:

```bash
ls -l script.sh
```

---

## 13. Directory Permissions

Permissions on directories have slightly different meanings.

| Permission | Directory Meaning                 |
| ---------- | --------------------------------- |
| `r`        | List directory contents           |
| `w`        | Create, delete, or rename entries |
| `x`        | Enter/access the directory        |

For example:

```bash
mkdir project
```

Check permissions:

```bash
ls -ld project
```

---

## 14. Execute Permission for Scripts

Create a script:

```bash
echo '#!/bin/bash' > hello.sh
echo 'echo "Hello DevOps"' >> hello.sh
```

Try to execute it:

```bash
./hello.sh
```

You may receive:

```text
Permission denied
```

Give execute permission:

```bash
chmod +x hello.sh
```

Run it again:

```bash
./hello.sh
```

Expected output:

```text
Hello DevOps
```

---

## 15. Default Permissions and umask

`umask` controls the default permissions assigned when new files and directories are created.

Check the current umask:

```bash
umask
```

Example:

```text
0022
```

The exact default permissions depend on the system and umask configuration.

---

## 16. Check Ownership and Permissions

Use:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 ubuntu developers 120 Sep 12 10:00 app.conf
```

Important information includes:

```text
-rw-r--r--
ubuntu
developers
```

Where:

* `-rw-r--r--` → permissions
* `ubuntu` → owner
* `developers` → group

---

## 17. Practice

Create a practice directory:

```bash
mkdir permissions-lab
cd permissions-lab
```

Create a file:

```bash
touch app.txt
```

Check its permissions:

```bash
ls -l app.txt
```

Set permissions to `644`:

```bash
chmod 644 app.txt
```

Verify:

```bash
ls -l app.txt
```

Create a script:

```bash
echo '#!/bin/bash' > script.sh
echo 'echo "DevOps Permission Lab"' >> script.sh
```

Check permissions:

```bash
ls -l script.sh
```

Give execute permission:

```bash
chmod +x script.sh
```

Run the script:

```bash
./script.sh
```

Expected output:

```text
DevOps Permission Lab
```

---

## 18. Verification

Run:

```bash
ls -l
```

Check the script:

```bash
ls -l script.sh
```

Check the file:

```bash
ls -l app.txt
```

Check the current user:

```bash
whoami
```

Check the current group information:

```bash
id
```

---

## Key Takeaways

| Command | Purpose                         |
| ------- | ------------------------------- |
| `ls -l` | Shows permissions and ownership |
| `chmod` | Changes file permissions        |
| `chown` | Changes file owner              |
| `chgrp` | Changes group ownership         |
| `umask` | Shows default permission mask   |
| `r`     | Read permission                 |
| `w`     | Write permission                |
| `x`     | Execute permission              |
| `u`     | Owner                           |
| `g`     | Group                           |
| `o`     | Others                          |
| `a`     | All users                       |

---

## Common Permission Values

| Permission | Numeric |
| ---------- | ------- |
| `rwx`      | `7`     |
| `rw-`      | `6`     |
| `r-x`      | `5`     |
| `r--`      | `4`     |
| `---`      | `0`     |

Common combinations:

```text
755 → rwxr-xr-x
644 → rw-r--r--
700 → rwx------
600 → rw-------
```

---

## Conclusion

Linux permissions are an important part of **server security and access control**.

Understanding `chmod`, `chown`, ownership, and permission values helps DevOps engineers securely manage application files, configuration files, scripts, logs, and Linux servers.
