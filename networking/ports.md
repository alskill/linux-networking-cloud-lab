# Networking Ports

## Objective

Understand network ports, common port numbers, TCP and UDP, listening ports, and Linux commands used to check network connections.

---

## 1. What is a Port?

A **port** is a logical communication endpoint used by applications and services to communicate over a network.

An IP address identifies the **server**, while a port identifies the **service or application** running on that server.

Example:

```text
192.168.1.10:80
```

Here:

* `192.168.1.10` → IP address
* `80` → Port number

Simple understanding:

```text
IP Address → Identifies the server
Port       → Identifies the service
```

---

## 2. Why are Ports Needed?

A single server can run multiple services.

For example:

```text
Server: 192.168.1.10

Port 22  → SSH
Port 80  → HTTP
Port 443 → HTTPS
Port 3306 → MySQL
```

The port number helps the operating system deliver network traffic to the correct application.

---

## 3. Port Number Range

TCP and UDP ports range from:

```text
0 - 65535
```

They are commonly divided into:

| Range         | Name                  | Description                          |
| ------------- | --------------------- | ------------------------------------ |
| `0–1023`      | Well-known ports      | Common system and standard services  |
| `1024–49151`  | Registered ports      | Common application services          |
| `49152–65535` | Dynamic/private ports | Often used for temporary connections |

---

## 4. Common Ports

|   Port | Protocol | Service                            |
| -----: | -------- | ---------------------------------- |
|   `20` | TCP      | FTP Data                           |
|   `21` | TCP      | FTP Control                        |
|   `22` | TCP      | SSH                                |
|   `23` | TCP      | Telnet                             |
|   `25` | TCP      | SMTP                               |
|   `53` | TCP/UDP  | DNS                                |
|   `80` | TCP      | HTTP                               |
|  `110` | TCP      | POP3                               |
|  `143` | TCP      | IMAP                               |
|  `443` | TCP      | HTTPS                              |
| `3306` | TCP      | MySQL                              |
| `5432` | TCP      | PostgreSQL                         |
| `6379` | TCP      | Redis                              |
| `8080` | TCP      | Common application/web port        |
| `9090` | TCP      | Common monitoring/application port |

---

## 5. SSH Port 22

SSH stands for **Secure Shell**.

SSH is commonly used to remotely connect to Linux servers.

```text
Client
   |
   | SSH
   | Port 22
   v
Linux Server
```

Example:

```bash
ssh user@192.168.1.10
```

The default SSH port is:

```text
22
```

---

## 6. HTTP Port 80

HTTP stands for **Hypertext Transfer Protocol**.

HTTP commonly uses port:

```text
80
```

Example:

```text
http://example.com
```

Simple flow:

```text
Browser
   |
   | HTTP :80
   v
Web Server
```

---

## 7. HTTPS Port 443

HTTPS is the secure version of HTTP.

HTTPS commonly uses:

```text
443
```

Example:

```text
https://example.com
```

Simple flow:

```text
Browser
   |
   | HTTPS :443
   v
Web Server
```

HTTPS encrypts communication using TLS.

---

## 8. DNS Port 53

DNS commonly uses:

```text
53
```

DNS can use both:

```text
TCP
UDP
```

UDP is commonly used for normal DNS queries, while TCP can be used for situations such as larger responses and zone transfers.

---

## 9. TCP

TCP stands for **Transmission Control Protocol**.

TCP provides reliable, connection-oriented communication.

Important characteristics:

* Connection-oriented
* Reliable delivery
* Ordered data
* Error checking
* Retransmission of lost data

Common TCP-based services include:

```text
SSH    → 22
HTTP   → 80
HTTPS  → 443
MySQL  → 3306
```

---

## 10. UDP

UDP stands for **User Datagram Protocol**.

UDP is connectionless and has less overhead than TCP.

Important characteristics:

* Connectionless
* Faster/lower overhead
* No guaranteed delivery
* No guaranteed ordering

Common examples include:

```text
DNS
DHCP
Streaming
Real-time applications
```

---

## 11. TCP vs UDP

| TCP                                | UDP                                       |
| ---------------------------------- | ----------------------------------------- |
| Connection-oriented                | Connectionless                            |
| Reliable delivery                  | No guaranteed delivery                    |
| Ordered data                       | No guaranteed ordering                    |
| More overhead                      | Lower overhead                            |
| Used when reliability is important | Used when speed/low overhead is important |

Simple understanding:

```text
TCP → Reliable communication
UDP → Fast, lightweight communication
```

---

## 12. Listening Port

A **listening port** is a port where a service is waiting for incoming network connections.

For example:

```text
Nginx
   |
   v
Listening on port 80
```

Check listening ports in Linux:

```bash
sudo ss -tuln
```

---

## 13. Understanding `ss -tuln`

Command:

```bash
sudo ss -tuln
```

Options:

| Option | Meaning                          |
| ------ | -------------------------------- |
| `-t`   | TCP                              |
| `-u`   | UDP                              |
| `-l`   | Listening                        |
| `-n`   | Show numeric addresses and ports |

Example output:

```text
Netid State  Local Address:Port
tcp   LISTEN 0.0.0.0:22
tcp   LISTEN 0.0.0.0:80
```

This means services are listening on ports `22` and `80`.

---

## 14. Check a Specific Port

Use `ss` with `grep`:

```bash
sudo ss -tuln | grep :80
```

Check SSH:

```bash
sudo ss -tuln | grep :22
```

Check port 443:

```bash
sudo ss -tuln | grep :443
```

---

## 15. Check Which Process Uses a Port

Use:

```bash
sudo ss -tulpn
```

The `-p` option shows the process using the socket.

Example:

```text
tcp LISTEN 0 511 0.0.0.0:80 0.0.0.0:* users:(("nginx",pid=1234))
```

This shows that Nginx is listening on port `80`.

---

## 16. Test a Port with curl

`curl` can be used to test HTTP services.

Test port 80:

```bash
curl http://localhost:80
```

Test a specific website:

```bash
curl http://example.com
```

Check only the HTTP headers:

```bash
curl -I http://example.com
```

For HTTPS:

```bash
curl -I https://example.com
```

---

## 17. Test Port Connectivity with nc

`nc` stands for **Netcat**.

It can be used to test whether a TCP port is reachable.

Example:

```bash
nc -zv 127.0.0.1 80
```

Test SSH:

```bash
nc -zv 127.0.0.1 22
```

The exact output depends on whether the port is listening and whether network access is allowed.

---

## 18. Port vs IP Address

| IP Address                         | Port                                 |
| ---------------------------------- | ------------------------------------ |
| Identifies a host/network endpoint | Identifies an application endpoint   |
| Example: `10.0.1.10`               | Example: `80`                        |
| Used for routing                   | Used to deliver traffic to a service |

Together:

```text
10.0.1.10:80
```

means:

```text
Server IP : Port
```

---

## 19. Port and Protocol

A port number by itself does not define everything about a network connection.

The protocol also matters.

For example:

```text
TCP : 80
UDP : 80
```

These are different transport-layer endpoints.

Common examples:

```text
TCP : 22  → SSH
TCP : 80  → HTTP
TCP : 443 → HTTPS
UDP : 53  → DNS
```

---

## 20. Ports in AWS

AWS Security Groups control inbound and outbound traffic using rules that can specify:

* Protocol
* Port
* Source or destination

Example web server Security Group:

```text
Inbound
   |
   +-- TCP 22  → SSH
   |
   +-- TCP 80  → HTTP
   |
   +-- TCP 443 → HTTPS
```

For a public web server:

```text
Internet
    |
    v
Security Group
    |
    +---- Port 80 ----> Nginx
    |
    +---- Port 443 ---> HTTPS
```

SSH should generally be restricted to trusted source IP addresses rather than being unnecessarily open to the entire internet.

---

## 21. Ports in Kubernetes

Kubernetes also uses ports when exposing applications.

For example:

```text
Pod
 |
 | containerPort: 8080
 v
Application
```

A Kubernetes Service can expose the application:

```text
Service Port: 80
      |
      v
Target Port: 8080
      |
      v
Pod
```

This allows users to access the service through one port while the application may listen on another port.

---

## 22. Port Troubleshooting

If an application cannot be accessed, check whether the expected port is listening.

### Step 1: Check listening ports

```bash
sudo ss -tuln
```

### Step 2: Check the specific port

```bash
sudo ss -tuln | grep :80
```

### Step 3: Check the service

```bash
sudo systemctl status nginx
```

### Step 4: Test locally

```bash
curl http://localhost:80
```

### Step 5: Check firewall rules

```bash
sudo ufw status
```

### Step 6: If using AWS, check the Security Group

Make sure the required port is allowed in the EC2 Security Group.

---

## 23. Practice

### Step 1: Check listening ports

```bash
sudo ss -tuln
```

### Step 2: Check SSH

```bash
sudo ss -tuln | grep :22
```

### Step 3: Check HTTP

```bash
sudo ss -tuln | grep :80
```

### Step 4: Check HTTPS

```bash
sudo ss -tuln | grep :443
```

### Step 5: Test HTTP locally

```bash
curl -I http://localhost
```

### Step 6: Check the process using ports

```bash
sudo ss -tulpn
```

---

## 24. Verification

Run:

```bash
sudo ss -tuln
```

Then:

```bash
sudo ss -tulpn
```

If Nginx is installed and running, you should normally see a listening entry for port `80`.

You can verify Nginx:

```bash
sudo systemctl status nginx
```

And test it:

```bash
curl -I http://localhost
```

---

## Key Takeaways

| Concept   | Meaning                                |
| --------- | -------------------------------------- |
| Port      | Logical communication endpoint         |
| TCP       | Reliable, connection-oriented protocol |
| UDP       | Connectionless, lightweight protocol   |
| Port 22   | SSH                                    |
| Port 53   | DNS                                    |
| Port 80   | HTTP                                   |
| Port 443  | HTTPS                                  |
| Port 3306 | MySQL                                  |
| Port 5432 | PostgreSQL                             |
| Port 6379 | Redis                                  |
| `ss`      | Checks network sockets and ports       |
| `curl`    | Tests HTTP/HTTPS communication         |
| `nc`      | Tests network connectivity to ports    |

---

## Conclusion

Understanding ports is essential for DevOps and cloud infrastructure.

Ports help applications communicate over networks, while tools such as `ss`, `curl`, and `nc` help troubleshoot connectivity problems.

In AWS, ports are also important when configuring **Security Groups**. In Kubernetes, ports are used to connect **Services, Pods, and applications**.
