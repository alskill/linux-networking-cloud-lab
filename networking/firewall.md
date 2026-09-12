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