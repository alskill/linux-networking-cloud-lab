# Nginx

## Objective

Learn how to install, configure, manage, and test Nginx on an AWS EC2 Ubuntu server.

By the end of this lab, you will be able to:

- Install Nginx
- Start and stop the Nginx service
- Check Nginx status
- Verify port 80
- Access Nginx from a browser
- Create a simple web page
- Check Nginx logs
- Troubleshoot common Nginx issues

---

## 1. What is Nginx?

**Nginx** is a web server and reverse proxy used to serve websites and applications.

It can handle:

- HTTP requests
- HTTPS requests
- Static websites
- Reverse proxying
- Load balancing
- Application traffic

Simple architecture:

```text
User Browser
     |
     | HTTP :80
     ↓
AWS Security Group
     |
     ↓
Ubuntu EC2
     |
     ↓
Nginx
     |
     ↓
Website / Application
```
## 2. Why Do We Use Nginx?

Nginx is commonly used because it is:

Lightweight
Fast
Reliable
Easy to configure
Suitable for high traffic
Commonly used with DevOps and cloud applications

Example:
```txt
Internet
   ↓
Nginx
   ↓
Application
```
Nginx can receive the request and forward it to an application running on another port.

## 3. Connect to the EC2 Server

First connect to your Ubuntu EC2 instance using SSH.
```txt
ssh -i "linux-lab-key.pem" ubuntu@<PUBLIC-IP>
```
Verify the connection:
```txt
whoami
```
Expected output:
```txt
ubuntu
```
## 4. Update the Package Repository

Before installing Nginx, update the Ubuntu package repository.
```txt
sudo apt update
```
Optional:
```txt
sudo apt upgrade -y
```
## 5. Install Nginx

Install Nginx using APT.
```txt
sudo apt install nginx -y
```
Verify the installation:
```txt
nginx -v
```
Example output:
```txt
nginx version: nginx/1.x.x
```
## 6. Check Nginx Status

Check whether Nginx is running.
```txt
sudo systemctl status nginx
```
You should see:

Active: active (running)

Press:
```txt
q
```
to exit the status screen.

## 7. Start Nginx

If Nginx is stopped, start it.
```txt
sudo systemctl start nginx
```
Check the status:
```txt
sudo systemctl status nginx
```
## 8. Stop Nginx

To stop the Nginx service:
```txt
sudo systemctl stop nginx
```
Check:
```txt
sudo systemctl status nginx
```
## 9. Restart Nginx

Restart Nginx:
```txt
sudo systemctl restart nginx
```
Check:
```txt
sudo systemctl status nginx
```
## 10. Reload Nginx

Reload configuration without completely stopping the service:
```txt
sudo systemctl reload nginx
```
This is commonly used after changing an Nginx configuration.

## 11. Enable Nginx at Boot

Enable Nginx so it starts automatically when the server boots.
```txt
sudo systemctl enable nginx
```
Check:
```txt
sudo systemctl is-enabled nginx
```
Expected:
```txt
enabled
```
## 12. Check Nginx Port

Nginx normally listens on HTTP port 80.

Check listening ports:
```txt
sudo ss -tuln
```
To specifically check port 80:
```txt
sudo ss -tuln | grep :80
```
Example:
```txt
LISTEN 0 511 0.0.0.0:80
```
This means Nginx is listening for HTTP traffic on port 80.

## 13. Test Nginx Locally

Test Nginx from inside the EC2 server:
```txt
curl -I http://localhost
```
Expected response contains something similar to:
```txt
HTTP/1.1 200 OK
Server: nginx
```
You can also use:
```txt
curl http://localhost
```
This should return HTML content.

## 14. Access Nginx from the Browser

Find the Public IPv4 address of your EC2 instance.

Example:
```txt
13.234.XX.XX
```
Open your browser and enter:
```txt
http://<PUBLIC-IP>
```
Example:
```txt
http://13.234.XX.XX
```
You should see the default Nginx welcome page.
## 15. Allow HTTP Port 80 in AWS Security Group

## 15. Allow HTTP Port 80 in AWS Security Group

If the browser cannot access your Nginx website, check the **AWS Security Group** attached to your EC2 instance.

The Security Group should allow HTTP traffic on port `80`.

### Required Rule

| Type | Protocol | Port | Source |
|---|---|---:|---|
| HTTP | TCP | 80 | 0.0.0.0/0 |

### Steps

1. Open the **AWS EC2 Console**.
2. Select your EC2 instance.
3. Go to the **Security** tab.
4. Click the attached **Security Group**.
5. Select **Inbound rules**.
6. Click **Edit inbound rules**.
7. Add the following rule:

```text
Type: HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0
```

The Security Group should allow:
Important:

Do not open SSH port 22 to 0.0.0.0/0 unless there is a specific reason.

## 16. Allow Port 80 in UFW

If UFW is enabled on Ubuntu, allow HTTP traffic.
```txt
sudo ufw allow 80/tcp
```
Check the firewall:
```txt
sudo ufw status
```
You should see a rule similar to:
```txt
80/tcp ALLOW
```
If UFW is disabled, AWS Security Group rules still control network access to the EC2 instance.

## 17. Nginx Default Website

The default Nginx website is usually stored under:
```txt
/var/www/html/
```
List the files:
```txt
ls -la /var/www/html/
```
You may see:
```txt
index.nginx-debian.html
```
## 18. Create a Simple Custom Web Page

You can replace the default page with a simple HTML page.

First create a backup:
```txt
sudo cp /var/www/html/index.nginx-debian.html /var/www/html/index.nginx-debian.html.backup
```
Create a new page:
```txt
sudo nano /var/www/html/index.html
```
Add:

<!DOCTYPE```txt html>
<html>
<head>
    <title>My DevOps Lab</title>
</head>
<body>
    <h1>Welcome to My AWS EC2 Server</h1>
    <p>Nginx is running successfully.</p>
    <p>Linux + Networking + Cloud Infrastructure Lab</p>
</body>
</html>
```
Save the file.

In Nano:
```txt
CTRL + O
```
ENTER
```txt
CTRL + X
```
## 19. Test the Custom Website

Test locally:
```txt
curl http://localhost
```
You should see your HTML content.

Then open the EC2 public IP in your browser:
```txt
http://<PUBLIC-IP>
```
You should see:

Welcome to My AWS EC2 Server
## 20. Check Nginx Configuration

The main Nginx configuration file is:
```txt
/etc/nginx/nginx.conf
```
View it:
```txt
sudo cat /etc/nginx/nginx.conf
```
The default website configuration is commonly located at:
```txt
/etc/nginx/sites-available/default
```
View it:
```txt
sudo cat /etc/nginx/sites-available/default
```
## 21. Test Nginx Configuration

Before reloading Nginx after configuration changes, test the configuration.
```txt
sudo nginx -t
```
Successful output should contain:

syntax is ok

test is successful

Then reload:
```txt
sudo systemctl reload nginx
```
## 22. Check Nginx Logs

Nginx logs are useful for troubleshooting.

Access Log
```txt
sudo tail -f /var/log/nginx/access.log
```
This shows incoming HTTP requests.

Press:
```txt
CTRL + C
```
to stop.

Error Log
```txt
sudo tail -f /var/log/nginx/error.log
```
This shows Nginx errors.
## 23. Check Nginx Service Logs

You can also use journalctl.
```txt
sudo journalctl -u nginx
```
Show recent logs:
```txt
sudo journalctl -u nginx -n 50
```
Follow logs:
```txt
sudo journalctl -u nginx -f
```
Press:
```txt
CTRL + C
```
to stop.

## 24. Common Nginx Commands

## 25. Nginx Troubleshooting
### Problem 1: Nginx is not running

Check:
```txt
sudo systemctl status nginx
```
Start it:
```txt
sudo systemctl start nginx
```
### Problem 2: Browser cannot access the website

Check Nginx:
```txt
sudo systemctl status nginx
```
Check port 80:
```txt
sudo ss -tuln | grep :80
```
Check AWS Security Group.

Make sure HTTP port 80 is allowed.

Check UFW:
```txt
sudo ufw status
```
Allow HTTP if required:
```txt
sudo ufw allow 80/tcp
```
### Problem 3: Nginx configuration error

Run:
```txt
sudo nginx -t
```
If there is an error, check the configuration files.
```txt
sudo nano /etc/nginx/nginx.conf
```
After fixing the configuration:
```txt
sudo nginx -t
```
Then:
```txt
sudo systemctl reload nginx
```
### Problem 4: Port 80 is not listening

Check:
```txt
sudo ss -tuln | grep :80
```
If there is no output, check:
```txt
sudo systemctl status nginx
```
Then:
```txt
sudo systemctl restart nginx
```
###  Problem 5: Website returns an error

Check the error log:
```txt
sudo tail -n 50 /var/log/nginx/error.log
```
Check the access log:

sudo tail -n 50 /var/log/nginx/access.log
## 26. Nginx Architecture
```txt
Basic architecture:
                Internet
                   |
                   | HTTP :80
                   ↓
          AWS Security Group
                   |
                   ↓
              Ubuntu EC2
                   |
                   ↓
                 UFW
                   |
                   ↓
                Nginx
                   |
                   ↓
           /var/www/html
                   |
                   ↓
             index.html
```             

## 27. Nginx Configuration Locations

Important locations:
```txt
/etc/nginx/
```
Main configuration:
```txt
/etc/nginx/nginx.conf
```
Available sites:
```txt
/etc/nginx/sites-available/
```
Enabled sites:
```txt
/etc/nginx/sites-enabled/
```
Default website:
```txt
/var/www/html/
```
Logs:
```txt
/var/log/nginx/
```
# 30. Cleanup

If this is only a temporary learning lab, stop Nginx when finished:
```txt
sudo systemctl stop nginx
```
If you want to remove Nginx:
```txt
sudo apt remove nginx -y
```
You can also remove unused packages:
```txt
sudo apt autoremove -y
```
For AWS cost control, remember to terminate the EC2 instance when the complete lab is finished if you no longer need it.

Also check for other billable resources such as:

- Elastic IPs
- NAT Gateways
- EBS volumes
- Load Balancers
- Snapshots
## 31. Key Takeaways
- Nginx is a web server and reverse proxy.
- Nginx commonly listens on port 80 for HTTP.
- systemctl is used to manage the Nginx service.
- nginx -t checks Nginx configuration.
- curl can test the website from the server.
- AWS Security Groups control network access to the EC2 instance.
- UFW can provide an additional host-level firewall.
- Nginx logs are useful for troubleshooting.
- Website files are commonly stored in /var/www/html/.
## Conclusion

In this lab, we installed Nginx on an Ubuntu EC2 server, managed the Nginx service, configured HTTP access, tested port 80, created a custom web page, checked logs, and learned basic Nginx troubleshooting.

This provides a practical foundation for understanding how Linux, networking, AWS EC2, security groups, and web servers work together in a cloud environment.