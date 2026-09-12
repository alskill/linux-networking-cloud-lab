# IP Addressing

## Objective

Understand IP addresses, IPv4, private and public IP addresses, subnetting basics, and common Linux commands used to check network configuration.

---

## 1. What is an IP Address?

An **IP address** is a unique address assigned to a device on a network.

It is used to identify a device and allow communication between systems.

Example:

```text
192.168.1.10
```

In cloud environments, servers such as AWS EC2 instances also use IP addresses for network communication.

---

## 2. IPv4 Address

IPv4 is the most commonly used IP addressing format.

An IPv4 address contains **32 bits** and is divided into four parts called octets.

Example:

```text
192.168.1.10
```

Each octet can have a value from:

```text
0 - 255
```

---

## 3. Public IP Address

A **public IP address** is reachable over the internet.

Example:

```text
13.234.120.50
```

Cloud servers may use public IP addresses so that users can access applications from the internet.

---

## 4. Private IP Address

A **private IP address** is used inside a private network.

Common private IPv4 ranges are:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

Examples:

```text
10.0.1.10
172.16.5.20
192.168.1.100
```

Private IP addresses are commonly used inside:

* Home networks
* Office networks
* AWS VPCs
* Kubernetes networks
* Internal cloud infrastructure

---

## 5. Public vs Private IP

| Public IP                       | Private IP                                  |
| ------------------------------- | ------------------------------------------- |
| Used for internet communication | Used inside private networks                |
| Globally reachable              | Not directly reachable from the internet    |
| Must be unique globally         | Can be reused in different private networks |
| Example: `13.234.120.50`        | Example: `10.0.1.10`                        |

---

## 6. Loopback Address

The loopback address refers to the local machine.

The most common IPv4 loopback address is:

```text
127.0.0.1
```

It is commonly called:

```text
localhost
```

Test it:

```bash
ping 127.0.0.1
```

Or:

```bash
ping localhost
```

---

## 7. Check IP Address in Linux

Use:

```bash
ip addr
```

A shorter command is:

```bash
ip a
```

This displays network interfaces and their IP addresses.

---

## 8. Check a Specific Network Interface

List network interfaces:

```bash
ip link
```

Example interfaces may include:

```text
lo
eth0
ens5
```

The exact interface name depends on the Linux system.

---

## 9. Check Routing Table

Use:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0
```

The routing table tells Linux where network traffic should be sent.

---

## 10. Default Gateway

A **default gateway** is the device that forwards traffic from the local network to other networks.

Check the default gateway:

```bash
ip route
```

Look for:

```text
default via <gateway-ip>
```

Example:

```text
default via 192.168.1.1 dev eth0
```

---

## 11. MAC Address

A **MAC address** is a hardware/network interface address.

Check it with:

```bash
ip link
```

Example:

```text
link/ether 00:11:22:33:44:55
```

---

## 12. Ping

`ping` is used to test network connectivity.

Test localhost:

```bash
ping 127.0.0.1
```

Test another host:

```bash
ping 8.8.8.8
```

Stop the command with:

```text
Ctrl + C
```

---

## 13. Check Connectivity to a Domain

You can test a domain name:

```bash
ping google.com
```

If the domain resolves to an IP address and replies are received, it indicates that DNS resolution and network connectivity are working for that test.

> Some servers block ICMP traffic, so a failed `ping` does not always mean the server is unreachable.

---

## 14. CIDR Notation

CIDR stands for **Classless Inter-Domain Routing**.

It represents an IP network using an IP address and prefix length.

Example:

```text
192.168.1.0/24
```

The `/24` means the first 24 bits represent the network portion.

Another example:

```text
10.0.0.0/16
```

This is commonly used for larger private networks.

---

## 15. Subnet

A **subnet** is a smaller network created from a larger network.

For example:

```text
VPC
10.0.0.0/16
    |
    +-- Public Subnet
    |   10.0.1.0/24
    |
    +-- Private Subnet
        10.0.2.0/24
```

In AWS, subnets are created inside a VPC.

---

## 16. AWS Example

A simple AWS network could look like:

```text
VPC
10.0.0.0/16
       |
       +-------------------+
       |                   |
Public Subnet          Private Subnet
10.0.1.0/24            10.0.2.0/24
       |                   |
    EC2 Server          Application
```

The public subnet can contain resources that need internet access, while private subnets are commonly used for internal resources.

---

## 17. Private IP vs Public IP in AWS EC2

An EC2 instance can have:

### Private IP

Used for communication inside the VPC.

Example:

```text
10.0.1.25
```

### Public IP

Used for internet communication when assigned to the instance.

Example:

```text
13.234.120.50
```

The private IP generally remains associated with the network interface while the instance is running, whereas public IPv4 addressing can change depending on how the instance is configured.

---

## 18. Useful Networking Commands

### Show IP addresses

```bash
ip addr
```

### Show network interfaces

```bash
ip link
```

### Show routing table

```bash
ip route
```

### Test connectivity

```bash
ping 8.8.8.8
```

### Test DNS and connectivity

```bash
ping google.com
```

### Show hostname

```bash
hostname
```

### Show hostname with network information

```bash
hostname -I
```

---

## 19. Practice

### Step 1: Check your IP address

```bash
ip addr
```

### Step 2: Check network interfaces

```bash
ip link
```

### Step 3: Check the routing table

```bash
ip route
```

### Step 4: Check your hostname

```bash
hostname
```

### Step 5: Check local IP addresses

```bash
hostname -I
```

### Step 6: Test localhost

```bash
ping 127.0.0.1
```

Stop the command:

```text
Ctrl + C
```

### Step 7: Test internet connectivity

```bash
ping 8.8.8.8
```

Stop the command:

```text
Ctrl + C
```

---

## 20. Verification

Run:

```bash
ip addr
```

Then:

```bash
ip route
```

Then:

```bash
hostname -I
```

Finally:

```bash
ping -c 4 127.0.0.1
```

You should receive replies from the local system.

---

## Key Takeaways

| Concept     | Meaning                                      |
| ----------- | -------------------------------------------- |
| IP Address  | Identifies a device on a network             |
| IPv4        | 32-bit IP addressing system                  |
| Public IP   | Used for internet communication              |
| Private IP  | Used inside private networks                 |
| `127.0.0.1` | Localhost/loopback address                   |
| Gateway     | Forwards traffic to other networks           |
| MAC Address | Network interface hardware address           |
| CIDR        | Represents an IP network using prefix length |
| Subnet      | Smaller network inside a larger network      |
| `ip addr`   | Shows IP addresses                           |
| `ip route`  | Shows routing table                          |
| `ping`      | Tests network connectivity                   |

---

## Conclusion

IP addressing is a fundamental networking concept for DevOps engineers.

Understanding public and private IPs, subnets, gateways, CIDR notation, and basic Linux networking commands helps when working with **AWS VPCs, EC2 instances, Kubernetes clusters, firewalls, and cloud infrastructure**.
