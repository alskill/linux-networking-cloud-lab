# SSH (Secure Shell)

## Objective

Learn how to securely connect to an AWS EC2 instance from a local computer using SSH.

## What is SSH?

SSH stands for **Secure Shell**.

It is a network protocol used to securely connect to and manage a remote Linux server.

Example:

```text
Local Computer → SSH → EC2 Linux Server
```

## SSH Port

SSH uses:

```text
Protocol: TCP
Port: 22
```

The EC2 Security Group must allow inbound TCP traffic on port **22**.

## Prerequisites

Before connecting to EC2, make sure you have:

* EC2 instance running
* Public IPv4 address
* SSH key pair (`.pem`)
* Security Group allowing SSH on port 22
* Correct username

For an Amazon Linux EC2 instance, the username is commonly:

```text
ec2-user
```

## Connect to EC2

From Git Bash, use:

```bash
ssh -i "key.pem" ec2-user@<PUBLIC-IP>
```

Example:

```bash
ssh -i "terraform-key.pem" ec2-user@13.201.10.20
```

Replace:

```text
terraform-key.pem → your key file
13.201.10.20       → your EC2 public IP
```

## First Connection

The first time you connect, SSH may ask:

```text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Enter:

```text
yes
```

If the connection is successful, you will get a shell on the EC2 server.

## Verify Connection

Run:

```bash
whoami
```

Check the server hostname:

```bash
hostname
```

Check the operating system:

```bash
cat /etc/os-release
```

Check the private IP:

```bash
hostname -I
```

## SSH Flow

```text
1. Local computer
       ↓
2. SSH request
       ↓
3. EC2 Public IP
       ↓
4. Security Group checks port 22
       ↓
5. SSH authentication using private key
       ↓
6. Linux EC2 server
```

## Troubleshooting

### Permission denied

Check:

* Username is correct
* `.pem` key is correct
* EC2 instance is running

### Connection timed out

Check:

* EC2 has a public IP
* Security Group allows TCP port 22
* Network connectivity is available

### Wrong key

Make sure you are using the private key that belongs to the EC2 key pair.

## Exit SSH

To disconnect from the EC2 server:

```bash
exit
```

## Security Best Practices

* Do not share your private `.pem` key.
* Do not upload the private key to GitHub.
* Allow SSH from **your IP** instead of `0.0.0.0/0` whenever possible.
* Use key-based authentication instead of passwords.

## Conclusion

SSH provides secure remote access to Linux EC2 instances. It is commonly used by DevOps engineers to manage and troubleshoot cloud servers.
