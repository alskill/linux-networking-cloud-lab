# AWS EC2 Setup

## Objective

Learn how to create, configure, connect to, and safely clean up an AWS EC2 Linux server.

In this lab, we will:

- Create an AWS EC2 instance
- Select an Ubuntu Linux AMI
- Choose an instance type
- Create or select a key pair
- Configure a Security Group
- Launch the EC2 instance
- Connect to the server using SSH
- Check the Linux server
- Update the server
- Verify the server
- Understand EC2 cleanup

---

## 1. What is Amazon EC2?

**EC2 = Elastic Compute Cloud**

Amazon EC2 provides virtual servers in the AWS cloud.

An EC2 instance is a virtual machine that can be used to:

- Host websites
- Run applications
- Run Docker containers
- Run databases
- Run DevOps tools
- Perform development and testing

Example:

```text
Your Computer
      |
      | Internet
      v
     AWS
      |
      v
   EC2 Instance
      |
      v
 Ubuntu Linux
 ```
## 2. EC2 Components

Important EC2 components include:

## 3. Choose an AWS Region

AWS has different geographical regions.

Example:

Mumbai Region
ap-south-1

For this lab, use:
```txt
Region: ap-south-1
```
Choose a region close to your users or workload when appropriate.

## 4. Open the EC2 Console

Open the AWS Management Console and go to:
```bash
EC2
```
Then select:
```bash
Instances
```
Click:
```bash
Launch instance
```
## 5. Choose an Instance Name

Give the instance a meaningful name.

Example:
```txt
linux-networking-lab
```
A useful name helps identify the server when working with multiple EC2 instances.

## 6. Choose an AMI

AMI = Amazon Machine Image

An AMI provides the operating system and initial software configuration for the EC2 instance.

For this lab, select:
```txt
Ubuntu Server
```
Example:
```txt
Ubuntu Server 24.04 LTS
```
Make sure the AMI architecture matches the selected instance type.
## 7. Choose an Instance Type

The instance type determines the compute resources available to the server.

It defines resources such as:
```txt
CPU
Memory
Network performance
```
For a small learning lab, select an eligible low-cost or free-tier option available to your AWS account and region.

Example:
```txt
Instance type: t3.micro
```
The exact free-tier eligibility depends on your AWS account, region, and current AWS pricing rules.

Always verify the current pricing before launching.

## 8. Create or Select a Key Pair

A key pair is used to securely connect to the EC2 instance.

For Linux EC2 instances, SSH commonly uses the private key.

Example key pair name:
```txt
linux-lab-key
```
When creating a key pair:

Key pair type: ED25519

or use the key type recommended by your AWS setup.

Download the private key file and store it securely.

Example:
```txt
linux-lab-key.pem
```
Important

Never upload your private key to GitHub.

Do not share:
```txt
*.pem
```
Add the private key to .gitignore if it is inside your project directory.

Example:
```txt
*.pem
```
## 9. Configure Network Settings

EC2 networking includes:
```txt
VPC
Subnet
Public IP
Security Group
```
For a basic lab, use the default VPC and a suitable public subnet if available.

Make sure the instance can receive SSH traffic from your trusted IP.

## 10. Configure Security Group

Create or select a Security Group.

For this lab, allow:
```bash
SSH
Port: 22
Source: My IP
```
Later, when Nginx is installed, allow:
```exe
HTTP
Port: 80
Source: Anywhere IPv4
```
Recommended setup:
```txt
SSH  : TCP 22  -> Your IP
HTTP : TCP 80  -> 0.0.0.0/0
```
Avoid opening SSH to:
```txt
0.0.0.0/0
```
unless there is a specific reason and appropriate security controls.

## 11. Configure Storage

EC2 uses EBS volumes for persistent block storage.

For a small Linux learning lab, the default root volume is usually sufficient.

Example:
```txt
Root volume
Type: gp3
Size: 8 GiB
```
The exact available minimum and default can vary by AMI and AWS configuration.

## 12. Launch the Instance

Review the configuration.

Check:
```txt
Instance name
AMI
Instance type
Key pair
VPC
Subnet
Security Group
Storage
```
Then click:
```txt
Launch instance
```
Wait for the instance state to become:

Running

## 13. Find the Instance Details

Select the EC2 instance.

Important information includes:
```txt
Instance ID
Instance state
Public IPv4 address
Private IPv4 address
Public IPv4 DNS
Availability Zone
Security Group
Subnet ID
VPC ID
```
Example:

Instance state: Running
- Public IPv4: 203.0.113.10
- Private IPv4: 10.0.1.10

The IP addresses above are examples only.

## 14. Connect to the EC2 Instance

For Ubuntu, the default SSH username is commonly:

ubuntu

Example SSH command:
```txt
ssh -i "linux-lab-key.pem" ubuntu@<PUBLIC-IP>
```
Example:
```txt
ssh -i "linux-lab-key.pem" ubuntu@203.0.113.10
```
Replace the example IP with the public IPv4 address of your EC2 instance.

## 15. Set Private Key Permissions

On Linux or Git Bash, SSH may require the private key to have restricted permissions.

Example:
```txt
chmod 400 linux-lab-key.pem
```
Then connect:
```txt
ssh -i "linux-lab-key.pem" ubuntu@<PUBLIC-IP>
```
## 16. Verify the Connection

After connecting, check the current user:
```txt
whoami
```
Expected:
```txt
ubuntu
```
Check the hostname:
```txt
hostname
```
Check the operating system:
```txt
cat /etc/os-release
```
Check the kernel:
```txt
uname -a
```
## 17. Check System Resources

Check CPU and memory:
```txt
free -h
````
Check disk usage:
```txt
df -h
```
Check system uptime:
```txt
uptime
```
Check CPU information:
```txt
lscpu
```
## 18. Update the Ubuntu Server

After connecting to the server, update the package list:
```txt
sudo apt update
```
Upgrade installed packages:
```txt
sudo apt upgrade -y
```
Check whether packages need updates:
```txt
sudo apt list --upgradable
```
## 19. Install Basic Networking Tools

You can install useful networking tools for the lab.
```txt
sudo apt install -y curl wget net-tools dnsutils
```
Verify:
```txt
curl --version
```
```txt
nslookup google.com
```
Check network interfaces:
```txt
ip addr
```
Check routing:
```txt
ip route
```
## 20. Test Internet Connectivity

Test connectivity to a public IP:
```txt
ping -c 4 8.8.8.8
```
Test DNS resolution:
```txt
ping -c 4 google.com
```
Test HTTP connectivity:
```txt
curl -I https://example.com
```
## 21. Check Network Information

Check IP addresses:
```txt
ip addr
```
Check routes:
```txt
ip route
```
Check listening ports:
```txt
sudo ss -tuln
```
Check DNS configuration:
```txt
cat /etc/resolv.conf
```
## 22. Check the EC2 Metadata

EC2 instances can access instance metadata.

For IMDSv2, obtain a metadata token:
```txt
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
```
Check the instance ID:
```txt
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id
```
Check the private IP:
```txt
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/local-ipv4
```
Check the instance hostname:
```txt
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/hostname
```
## 23. Check Running Services

Check SSH:
```txt
sudo systemctl status ssh
```
Check all failed services:
```txt
systemctl --failed
```
Check running services:
```txt
systemctl list-units --type=service --state=running
```
## 24. Basic EC2 Troubleshooting
SSH Connection Refused

Check:
```txt
Security Group
    |
    +-- TCP 22 allowed?
    |
    +-- Source IP correct?
```
Check the EC2 instance state:
```txt
Running
```
Check that the correct public IP is being used.
```txt
SSH Timeout
```
Possible causes:

- EC2 instance is not running
- Security Group does not allow port 22
- Wrong public IP
- Network configuration problem
- Local network restrictions

Check:
```txt
ssh -i "linux-lab-key.pem" ubuntu@<PUBLIC-IP>
```
Permission Denied for Key

Set the key permissions:
```txt
chmod 400 linux-lab-key.pem
```
Then try again:
```txt
ssh -i "linux-lab-key.pem" ubuntu@<PUBLIC-IP>
```
Cannot Access the Internet

Check the route:
```txt
ip route
```
Check DNS:
```txt
nslookup google.com
```
Test connectivity:
```txt
ping -c 4 8.8.8.8
```
## 25. EC2 Architecture
```txt

The basic architecture for this lab is:
                    Internet
                       |
                       v
              AWS Security Group
                  |           |
                SSH 22      HTTP 80
                  |           |
                  v           v
              +-------------------+
              |    Ubuntu EC2     |
              |                   |
              |       UFW         |
              |         |         |
              |       Nginx       |
              +-------------------+

```

## 26. EC2 Security Best Practices

Follow these practices:

- Use SSH keys instead of passwords.
- Restrict SSH access to your trusted IP where possible.
- Do not expose private keys.
- Never upload .pem files to GitHub.
- Use Security Groups carefully.
- Keep the operating system updated.
- Use UFW or another host firewall where appropriate.
- Stop or terminate unused EC2 instances.
- Remove unused EBS volumes and Elastic IPs when they are no longer needed.
- Monitor AWS costs.
## 27. Stop vs Terminate
### Stop

Stopping an EC2 instance shuts down the compute instance but keeps the instance configuration and attached EBS storage.

Some resources can still incur charges.

### Terminate

Terminating an EC2 instance permanently deletes the instance.

The root EBS volume is commonly deleted on termination when its Delete on termination setting is enabled.

Always verify the storage configuration before terminating.

## 28. Lab Cleanup

When the lab is complete, terminate the EC2 instance if you no longer need it.

Before terminating, check:

- EC2 instance
- EBS volumes
- Elastic IPs
- Snapshots
- Load balancers
- NAT Gateways
- Other AWS resources

In the AWS Console:
```txt
EC2
  |
  v
Instances
  |
  v
Select instance
  |
  v
Instance state
  |
  v
Terminate instance
```
## 29. Important Cost Warning

EC2 is only one possible source of AWS charges.

Other resources can also generate costs, such as:

- EBS storage
- Elastic IP addresses
- NAT Gateways
- Load Balancers
- Snapshots
- Data transfer
- Other AWS services

After completing the lab, verify that unnecessary resources have been removed.       
## 30. Key Takeaways
- EC2 provides virtual servers in AWS.
- An AMI provides the operating system image.
- Instance type determines CPU and memory resources.
- Key pairs are used for secure SSH authentication.
- Security Groups control network traffic to and from EC2.
- EBS provides persistent block storage.
- A public IP can be used for internet communication.
- A private IP is used for communication inside the VPC.
- SSH commonly uses port 22.
- Ubuntu commonly uses the ubuntu user for SSH.
- Always protect your private key.
- Always clean up unused AWS resources to avoid unnecessary charges.
## Conclusion

In this section, we created and configured an AWS EC2 Linux server.

We learned how to:
```txt
Create EC2
    ↓
Choose Ubuntu
    ↓
Configure Key Pair
    ↓
Configure Security Group
    ↓
Launch Instance
    ↓
Connect using SSH
    ↓
Check Linux System
    ↓
Test Networking
    ↓
Update Server
```