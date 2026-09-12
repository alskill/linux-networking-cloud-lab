# Linux Users and Groups

## Objective

Learn how Linux manages users and groups and how to create, modify, and manage user accounts.

---

## 1. What is a User in Linux?

A **user** is an account that can access and use a Linux system.

Each user can have:

* A username
* A user ID (UID)
* A home directory
* A login shell
* File permissions
* Group memberships

Linux uses users to control access to files, directories, applications, and system resources.

---

## 2. Types of Users

### Root User

The **root user** is the administrator of a Linux system.

Root has almost complete control over the system.

```bash
whoami
```

If the output is:

```text
root
```

you are logged in as the root user.

### Normal User

A normal user has limited permissions and cannot normally perform administrative operations without using `sudo`.

---

## 3. Check Current User

Use `whoami` to find the currently logged-in user.

```bash
whoami
```

Example output:

```text
ubuntu
```

---

## 4. Check User Information

Use the `id` command.

```bash
id
```

Example:

```text
uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),27(sudo)
```

### Important Terms

* **UID** → User ID
* **GID** → Group ID
* **groups** → Groups the user belongs to

---

## 5. List Users

Linux stores local user account information in:

```text
/etc/passwd
```

View the file:

```bash
cat /etc/passwd
```

Search for a specific user:

```bash
grep "ubuntu" /etc/passwd
```

---

## 6. Create a User

Use `useradd` to create a user.

```bash
sudo useradd devops
```

Create a user with a home directory:

```bash
sudo useradd -m devops
```

The `-m` option creates the user's home directory.

Example:

```text
/home/devops
```

---

## 7. Set a User Password

Use the `passwd` command.

```bash
sudo passwd devops
```

The system will ask you to enter and confirm the password.

---

## 8. Check User Home Directory

List the `/home` directory:

```bash
ls -la /home
```

You may see:

```text
/home/devops
```

---

## 9. Create a Group

A **group** is a collection of users.

Groups make it easier to manage permissions for multiple users.

Create a group:

```bash
sudo groupadd developers
```

Check the group:

```bash
grep "developers" /etc/group
```

---

## 10. Add a User to a Group

Add the `devops` user to the `developers` group:

```bash
sudo usermod -aG developers devops
```

### Meaning

* `usermod` → modifies a user
* `-a` → append
* `-G` → supplementary group

The `-aG` combination adds the user to the group without removing existing supplementary groups.

---

## 11. Check Group Membership

Check the groups of a user:

```bash
groups devops
```

Or:

```bash
id devops
```

Example:

```text
uid=1001(devops) gid=1001(devops) groups=1001(devops),1002(developers)
```

---

## 12. Change User Information

The `usermod` command can modify a user.

Change the user's shell:

```bash
sudo usermod -s /bin/bash devops
```

Change the user's home directory:

```bash
sudo usermod -d /home/devops devops
```

> Be careful when changing a user's home directory on a real server.

---

## 13. Remove a User

Remove a user:

```bash
sudo userdel devops
```

Remove a user and their home directory:

```bash
sudo userdel -r devops
```

> Use `userdel -r` carefully because it removes the user's home directory and its contents.

---

## 14. Remove a User from a Group

On Ubuntu, you can use:

```bash
sudo gpasswd -d devops developers
```

Verify:

```bash
groups devops
```

---

## 15. Switch to Another User

Use the `su` command:

```bash
su - devops
```

The `-` starts a login shell and loads the user's environment.

Return to the previous user:

```bash
exit
```

---

## 16. Using sudo

`sudo` allows an authorized normal user to execute commands with administrative privileges.

Example:

```bash
sudo apt update
```

Another example:

```bash
sudo systemctl status nginx
```

You may be asked for the current user's password.

---

## 17. Check Who Can Use sudo

On Ubuntu, members of the `sudo` group generally have administrative privileges.

Check your groups:

```bash
groups
```

Check whether the current user can use sudo:

```bash
sudo -l
```

---

## 18. Important Linux User Files

| File           | Purpose                               |
| -------------- | ------------------------------------- |
| `/etc/passwd`  | Stores basic user account information |
| `/etc/shadow`  | Stores password-related information   |
| `/etc/group`   | Stores group information              |
| `/etc/sudoers` | Defines sudo permissions              |

> Do not edit `/etc/sudoers` directly with a normal text editor. Use `visudo` when modifying sudo configuration.

---

## 19. Practice

### Step 1: Create a user

```bash
sudo useradd -m devops
```

### Step 2: Set a password

```bash
sudo passwd devops
```

### Step 3: Create a group

```bash
sudo groupadd developers
```

### Step 4: Add the user to the group

```bash
sudo usermod -aG developers devops
```

### Step 5: Check the user

```bash
id devops
```

### Step 6: Check group membership

```bash
groups devops
```

### Step 7: Check the home directory

```bash
ls -la /home
```

---

## 20. Verification

Run:

```bash
id devops
```

Then:

```bash
groups devops
```

You should see the `developers` group in the user's group membership.

Check the user's home directory:

```bash
ls -ld /home/devops
```

---

## Key Takeaways

| Command      | Purpose                                          |
| ------------ | ------------------------------------------------ |
| `whoami`     | Shows current user                               |
| `id`         | Shows UID, GID, and groups                       |
| `useradd`    | Creates a user                                   |
| `passwd`     | Sets or changes a password                       |
| `usermod`    | Modifies a user                                  |
| `userdel`    | Deletes a user                                   |
| `groupadd`   | Creates a group                                  |
| `groups`     | Shows group membership                           |
| `gpasswd -d` | Removes a user from a group                      |
| `su`         | Switches to another user                         |
| `sudo`       | Executes commands with administrative privileges |

---

## Conclusion

Linux users and groups are important for **security and access control**.

Users identify who can access the system, while groups make it easier to manage permissions for multiple users.

In DevOps, user and group management is commonly used when configuring Linux servers, application accounts, deployment users, and service accounts.
