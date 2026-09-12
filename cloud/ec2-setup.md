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