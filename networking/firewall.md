# Firewall

## Objective

Understand what a firewall is, how inbound and outbound traffic work, how to use UFW on Linux, and how AWS Security Groups work together with a Linux firewall.

---

## 1. What is a Firewall?

A **firewall** controls network traffic entering or leaving a system.

It can:

- Allow trusted traffic
- Block unwanted traffic
- Control access to specific ports
- Restrict access to specific IP addresses

Example:

```text
Internet
   |
   v
Firewall
   |
   +---- Allow SSH : 22
   |
   +---- Allow HTTP : 80
   |
   +---- Block unwanted traffic
```
   ## 2. Inbound and Outbound Traffic

### Inbound Traffic

Inbound traffic is network traffic coming **into** the server.

Example:

```text
Your Laptop ---> EC2 Server
             SSH : 22

```             
## 3. Types of Firewalls

In this lab, we work with two firewall layers:

## 1. AWS Security Group

AWS Security Group controls network traffic to and from an EC2 instance.

It works at the AWS infrastructure level.

Example:
```text
Internet
   |
   v
AWS Security Group
   |
   v
   EC2        
```
## 2. UFW

## UFW = Uncomplicated Firewall

UFW is a simple firewall management tool commonly used on Ubuntu.

It controls traffic at the Linux host level.

Example:     
```text
AWS Security Group
        |
        v
      Ubuntu
        |
        v
       UFW
        |
        v
     Nginx
```

## 4. Check UFW Status

Check whether UFW is active:
```text
sudo ufw status
```
For more details:
```text
sudo ufw status verbose
``` 
Example:
```text
Status: inactive
```
UFW may be disabled by default.
-----------
## 5. Important SSH Warning

If you are connected to a remote EC2 server through SSH, make sure SSH is allowed before enabling UFW.

Otherwise, you may lock yourself out of the server.

Allow SSH:
```text
sudo ufw allow 22/tcp

```
Or use the application name:
```text
sudo ufw allow OpenSSH
```
Then check:
```text
sudo ufw status
```

## 6. Allow HTTP

Nginx normally uses port 80 for HTTP.

Allow HTTP:
```text
sudo ufw allow 80/tcp
```
Check the rule:
```text 
sudo ufw status
```
Expected:
```text
22/tcp    ALLOW
80/tcp    ALLOW
```
7. Allow HTTPS

HTTPS normally uses port 443.
```text
sudo ufw allow 443/tcp
```
8. Allow SSH Only From a Specific IP

For better security, SSH can be restricted to a trusted IP address.

Example:
```text
sudo ufw allow from <YOUR-IP> to any port 22 proto tcp
```
Example:
```text
sudo ufw allow from 203.0.113.10 to any port 22 proto tcp
```
Replace the example IP with your actual trusted public IP.

This is safer than allowing SSH from everyone.

9. Deny a Port

Example: block Telnet port 23.
```text
sudo ufw deny 23/tcp    
```
Check:
```text
sudo ufw status
```
## 10. Delete a Firewall Rule

List numbered rules:
```txt
sudo ufw status numbered
```
Example:
```txt
[ 1] 22/tcp ALLOW
[ 2] 80/tcp ALLOW
[ 3] 23/tcp DENY
```
Delete rule number 3:
```txt
sudo ufw delete 3
```
Check again:
```txt
sudo ufw status numbered
```
## 11. Default Firewall Policies

A common server configuration is:

Deny incoming traffic by default
Allow outgoing traffic by default

Commands:
```txt
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
Important: Make sure required ports such as SSH are allowed before enabling these policies on a remote server.

## 12. Enable UFW

First make sure SSH is allowed:
```txt
sudo ufw allow OpenSSH
```
Allow HTTP if using Nginx:
```txt
sudo ufw allow 80/tcp
```
Then enable UFW:
```txt
sudo ufw enable
```
Confirm:
```txt
sudo ufw status verbose
```
Example:
```txt
Status: active
```
```txt
22/tcp    ALLOW
80/tcp    ALLOW
```
## 13. Disable UFW

To temporarily disable UFW:
```txt
sudo ufw disable
```
Check:
```txt
sudo ufw status
```
## 14. Reload UFW

After changing rules:
```txt
sudo ufw reload
```
Check:
```txt
sudo ufw status verbose
```
## 15. Reset UFW

To remove UFW rules and return to the default configuration:
```txt
sudo ufw reset
```
Warning: This removes existing UFW rules.

Do not use this on a production server without understanding the impact.

## 16. UFW and Nginx

If Nginx is running on port 80, UFW must allow port 80.

Check Nginx listening ports:
```txt
sudo ss -tuln
```
Allow HTTP:
```txt
sudo ufw allow 80/tcp
```
Test locally:
```rxt
curl -I http://localhost
```
Expected response:
```txt
HTTP/1.1 200 OK
```
## 17. Check Listening Ports

Use:
```txt
sudo ss -tuln
```
For processes:
```txt
sudo ss -tulpn
```
Example:
```txt
LISTEN 0 511 0.0.0.0:80
```
This means a service is listening on port 80.

Both can work together.

Example:

Internet
   |
   v
AWS Security Group
   |
   | Allow TCP 22
   | Allow TCP 80
   v
Ubuntu EC2
   |
   v
UFW
   |
   | Allow TCP 22
   | Allow TCP 80
   v
Services

## 18. AWS Security Group vs UFW

For traffic to reach the service, both layers must allow it.

| Feature | AWS Security Group | UFW |
|---|---|---|
| Location | AWS infrastructure | Linux server |
| Applies to | EC2 network interface | Linux host |
| Controls | Network traffic | Host traffic |
| Managed through | AWS Console, CLI, Terraform | Linux commands |
| Example | Allow TCP 22 | Allow TCP 22 |
| Layer | Cloud/network level | Operating-system level |
| Purpose | Controls traffic to/from EC2 | Controls traffic on the Linux server |

### How They Work Together

Both firewalls can work together to provide multiple layers of protection.

```text
Internet
   |
   v
AWS Security Group
   |
   | Allow TCP 22
   | Allow TCP 80
   v
Ubuntu EC2
   |
   v
UFW
   |
   | Allow TCP 22
   | Allow TCP 80
   v
Application / Nginx
```

## 19. Example EC2 Firewall Configuration

For this lab:

SSH

Port:
```txt
22
```
Purpose:

Remote server administration
HTTP

Port:
```txt
80
```
Purpose:

Nginx web traffic

Recommended setup:

AWS Security Group
------------------
```txt
SSH 22  -> Your IP
HTTP 80 -> 0.0.0.0/0
```
UFW
------------------
```txt
22/tcp -> ALLOW
80/tcp -> ALLOW
```
## 20. Firewall Troubleshooting
Problem: SSH connection fails

Check AWS Security Group:
```txt
TCP 22 -> Your IP
```
Check UFW:
```txt
sudo ufw status
```
Make sure SSH is allowed:
```txt
sudo ufw allow OpenSSH
```
Problem: Nginx is not accessible

Check Nginx:
```txt
sudo systemctl status nginx
```
Check port 80:
```txt
sudo ss -tuln | grep :80
```
Check UFW:
```txt
sudo ufw status
```
Allow HTTP:
```txt
sudo ufw allow 80/tcp
```    
Also check the AWS Security Group:

TCP 80 -> Allowed
Problem: UFW is blocking traffic

Check:
```txt
sudo ufw status verbose
```
Check numbered rules:
```txt
sudo ufw status numbered
```
Add the required rule:
```txt
sudo ufw allow 80/tcp
```
Reload:
```txt
sudo ufw reload
```
## 21. Practice
Step 1: Check UFW
```txt
sudo ufw status verbose
```
Step 2: Allow SSH
```txt
sudo ufw allow OpenSSH
```
Step 3: Allow HTTP
```txt
sudo ufw allow 80/tcp
```
Step 4: Enable UFW
```txt
sudo ufw enable
```
Step 5: Verify
```txt
sudo ufw status numbered
```
Step 6: Check listening ports
```txt
sudo ss -tuln
```
Step 7: Test local HTTP
```txt
curl -I http://localhost
```
## 22. Useful UFW Commands

## 22. Useful UFW Commands

| Command | Purpose |
|---|---|
| `sudo ufw status` | Check UFW status |
| `sudo ufw status verbose` | Show detailed firewall status |
| `sudo ufw status numbered` | Show numbered firewall rules |
| `sudo ufw allow 22/tcp` | Allow SSH traffic |
| `sudo ufw allow 80/tcp` | Allow HTTP traffic |
| `sudo ufw allow 443/tcp` | Allow HTTPS traffic |
| `sudo ufw allow OpenSSH` | Allow SSH |
| `sudo ufw deny 23/tcp` | Block port 23 |
| `sudo ufw delete <number>` | Delete a firewall rule |
| `sudo ufw enable` | Enable UFW |
| `sudo ufw disable` | Disable UFW |
| `sudo ufw reload` | Reload UFW rules |
| `sudo ufw reset` | Reset UFW rules |
| `sudo ufw default deny incoming` | Deny incoming traffic by default |
| `sudo ufw default allow outgoing` | Allow outgoing traffic by default |

## 23. Key Takeaways
- A firewall controls network traffic.
- Inbound traffic comes into the server.
- Outbound traffic leaves the server.
- UFW is a simple Linux firewall management tool.
- AWS Security Groups are separate from UFW.
- SSH normally uses port 22.
- HTTP normally uses port 80.
- HTTPS normally uses port 443.
- Always allow SSH before enabling UFW on a remote EC2 server.
- For better security, restrict SSH to your trusted IP.
- Both AWS Security Groups and UFW can protect an EC2 server.
## Conclusion

In this section, we learned how firewalls control network traffic and how to configure UFW on Linux.

We also learned the difference between:

AWS Security Group
        +
      UFW
        =
Multiple layers of network protection

This prepares us for the next section where we create and configure an AWS EC2 Linux server.