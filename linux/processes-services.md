# Linux Processes and Services

## Objective

Learn how to view, monitor, manage, and troubleshoot **processes and services** in Linux.

---

## 1. What is a Process?

A **process** is a running instance of a program.

For example, when you start Nginx, a process is created to run the Nginx application.

Each process has a unique **PID (Process ID)**.

---

## 2. What is a PID?

PID stands for **Process ID**.

Every running process in Linux has a unique PID.

To view running processes:

```bash
ps
```

Example:

```text
PID TTY          TIME CMD
1234 pts/0    00:00:00 bash
1456 pts/0    00:00:00 ps
```

---

## 3. View All Running Processes

Use:

```bash
ps aux
```

This displays detailed information about running processes.

Important columns include:

* USER → User running the process
* PID → Process ID
* %CPU → CPU usage
* %MEM → Memory usage
* COMMAND → Command that started the process

---

## 4. Search for a Process

Use `grep` with `ps`:

```bash
ps aux | grep nginx
```

Another option is:

```bash
pgrep nginx
```

`pgrep` searches for processes by name.

---

## 5. Monitor Processes in Real Time

Use:

```bash
top
```

`top` continuously displays running processes and system resource usage.

It can show:

* CPU usage
* Memory usage
* Process IDs
* Running processes
* System load

Press:

```text
q
```

to exit `top`.

---

## 6. Check System Uptime

Use:

```bash
uptime
```

Example:

```text
10:30:15 up 2 days, 4:20, 1 user, load average: 0.10, 0.15, 0.20
```

This shows how long the system has been running and the system load.

---

## 7. Stop a Process

The `kill` command is used to send a signal to a process.

First find the PID:

```bash
ps aux | grep nginx
```

Then terminate the process:

```bash
sudo kill PID
```

Replace `PID` with the actual process ID.

Example:

```bash
sudo kill 1234
```

---

## 8. Force Stop a Process

If a process does not stop normally, you can use:

```bash
sudo kill -9 PID
```

Example:

```bash
sudo kill -9 1234
```

> Use `kill -9` carefully. It forcefully terminates the process and should generally be used only when a normal termination does not work.

---

## 9. What is a Service?

A **service** is a background program that performs a specific function on a Linux system.

Examples:

* Nginx
* SSH
* Docker
* Cron
* Network services

Most modern Linux distributions use **systemd** to manage services.

---

## 10. systemctl

`systemctl` is used to manage systemd services.

Check the status of a service:

```bash
sudo systemctl status nginx
```

Start a service:

```bash
sudo systemctl start nginx
```

Stop a service:

```bash
sudo systemctl stop nginx
```

Restart a service:

```bash
sudo systemctl restart nginx
```

Reload a service configuration:

```bash
sudo systemctl reload nginx
```

---

## 11. Enable and Disable Services

### Enable a service

Enabling a service makes it start automatically when the system boots.

```bash
sudo systemctl enable nginx
```

### Disable a service

```bash
sudo systemctl disable nginx
```

Check whether a service is enabled:

```bash
systemctl is-enabled nginx
```

---

## 12. Check if a Service is Running

Use:

```bash
systemctl is-active nginx
```

Example output:

```text
active
```

You can also use:

```bash
sudo systemctl status nginx
```

---

## 13. List Running Services

To list active services:

```bash
systemctl list-units --type=service --state=running
```

To list all service units:

```bash
systemctl list-units --type=service
```

---

## 14. Check SSH Service

SSH is commonly used to remotely access Linux servers.

Check SSH service status:

```bash
sudo systemctl status ssh
```

On some Linux distributions, the service may be named `sshd`:

```bash
sudo systemctl status sshd
```

Start SSH:

```bash
sudo systemctl start ssh
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

---

## 15. Check Nginx Service

After installing Nginx:

```bash
sudo systemctl status nginx
```

Start Nginx:

```bash
sudo systemctl start nginx
```

Enable Nginx at boot:

```bash
sudo systemctl enable nginx
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## 16. View Service Logs

`journalctl` is used to view logs collected by systemd.

View Nginx-related service logs:

```bash
sudo journalctl -u nginx
```

View recent logs:

```bash
sudo journalctl -u nginx -n 50
```

Follow logs in real time:

```bash
sudo journalctl -u nginx -f
```

Press:

```text
Ctrl + C
```

to stop following the logs.

---

## 17. Check Service Failures

List failed services:

```bash
systemctl --failed
```

This is useful when troubleshooting a Linux server.

---

## 18. Process vs Service

| Process                       | Service                                  |
| ----------------------------- | ---------------------------------------- |
| Running instance of a program | Background program managed by the system |
| Has a PID                     | Usually managed by systemd               |
| Can be managed with `kill`    | Can be managed with `systemctl`          |
| Example: Nginx worker process | Example: Nginx service                   |

### Simple Understanding

```text
Service
   ↓
Starts application
   ↓
Application creates process
   ↓
Process gets a PID
```

---

## 19. Useful Troubleshooting Commands

### Check whether Nginx is running

```bash
sudo systemctl status nginx
```

### Check Nginx processes

```bash
ps aux | grep nginx
```

### Check listening ports

```bash
sudo ss -tuln
```

### Check Nginx logs

```bash
sudo journalctl -u nginx -n 50
```

### Check failed services

```bash
systemctl --failed
```

---

## 20. Practice

### Step 1: Check running processes

```bash
ps aux
```

### Step 2: Check system resources

```bash
top
```

Press `q` to exit.

### Step 3: Check SSH service

```bash
sudo systemctl status ssh
```

If your distribution uses `sshd`:

```bash
sudo systemctl status sshd
```

### Step 4: Check running services

```bash
systemctl list-units --type=service --state=running
```

### Step 5: Check failed services

```bash
systemctl --failed
```

### Step 6: Check listening ports

```bash
sudo ss -tuln
```

---

## 21. Verification

Run:

```bash
ps aux | head
```

Check system uptime:

```bash
uptime
```

Check services:

```bash
systemctl --failed
```

Check listening ports:

```bash
sudo ss -tuln
```

If these commands execute successfully, the basic Linux process and service management practice is complete.

---

## Key Takeaways

| Command              | Purpose                          |
| -------------------- | -------------------------------- |
| `ps`                 | Shows processes                  |
| `ps aux`             | Shows detailed running processes |
| `pgrep`              | Finds processes by name          |
| `top`                | Monitors processes in real time  |
| `kill`               | Sends a signal to a process      |
| `kill -9`            | Forcefully terminates a process  |
| `systemctl status`   | Checks service status            |
| `systemctl start`    | Starts a service                 |
| `systemctl stop`     | Stops a service                  |
| `systemctl restart`  | Restarts a service               |
| `systemctl enable`   | Enables service at boot          |
| `systemctl disable`  | Disables service at boot         |
| `journalctl`         | Views systemd logs               |
| `systemctl --failed` | Shows failed services            |
| `ss -tuln`           | Shows listening ports            |

---

## Conclusion

Understanding processes and services is important for DevOps because applications must run reliably on Linux servers.

DevOps engineers use commands such as `ps`, `top`, `systemctl`, `journalctl`, and `ss` to monitor applications, manage services, investigate failures, and troubleshoot production systems.
