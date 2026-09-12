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
