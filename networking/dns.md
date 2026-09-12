# DNS (Domain Name System)

## Objective

Understand how DNS works, why it is used, common DNS records, and Linux commands used to troubleshoot DNS.

---

## 1. What is DNS?

DNS stands for **Domain Name System**.

DNS converts human-readable domain names into IP addresses.

For example:

```text
google.com
     ↓
IP Address
```

Instead of remembering an IP address, users can access a service using a domain name.

---

## 2. Why Do We Need DNS?

Computers communicate using IP addresses, but IP addresses can be difficult to remember.

For example:

```text
https://example.com
```

is easier to remember than an IP address.

DNS provides the translation:

```text
Domain Name
     ↓
DNS
     ↓
IP Address
     ↓
Web Server
```

---

## 3. Simple DNS Flow

When a user enters:

```text
www.example.com
```

the following process can happen:

```text
User
  |
  v
DNS Resolver
  |
  v
DNS Server
  |
  v
IP Address
  |
  v
Web Server
```

The browser can then connect to the server using the resolved IP address.

---

## 4. DNS Resolver

A **DNS resolver** receives a DNS query from a client and finds the IP address associated with a domain name.

For example:

```text
Client
   |
   | What is the IP of example.com?
   v
DNS Resolver
   |
   v
IP Address
```

---

## 5. DNS Server

A DNS server stores or retrieves DNS information.

It helps answer questions such as:

```text
What IP address belongs to example.com?
```

DNS servers can be:

* Recursive resolvers
* Authoritative DNS servers
* Root DNS servers
* TLD DNS servers

---

## 6. DNS Resolution

A simplified DNS lookup can involve:

```text
Client
  |
  v
Recursive Resolver
  |
  v
Root DNS Server
  |
  v
TLD DNS Server
  |
  v
Authoritative DNS Server
  |
  v
IP Address
```

For example:

```text
www.example.com
        |
        v
     DNS Lookup
        |
        v
    IP Address
```

---

## 7. Common DNS Records

DNS uses different record types for different purposes.

| Record  | Purpose                                  |
| ------- | ---------------------------------------- |
| `A`     | Maps a domain name to an IPv4 address    |
| `AAAA`  | Maps a domain name to an IPv6 address    |
| `CNAME` | Creates an alias for another domain name |
| `MX`    | Defines mail servers                     |
| `TXT`   | Stores text information                  |
| `NS`    | Defines authoritative name servers       |
| `PTR`   | Used for reverse DNS lookup              |

---

## 8. A Record

An `A` record maps a domain name to an IPv4 address.

Example:

```text
example.com → 93.184.216.34
```

Conceptually:

```text
example.com
     |
     v
A Record
     |
     v
93.184.216.34
```

---

## 9. AAAA Record

An `AAAA` record maps a domain name to an IPv6 address.

Example:

```text
example.com → 2001:db8::1
```

---

## 10. CNAME Record

A `CNAME` record creates an alias for another domain name.

Example:

```text
www.example.com
        |
        v
example.com
```

A CNAME points to another hostname rather than directly storing an IP address.

---

## 11. MX Record

An `MX` record specifies the mail servers responsible for receiving email for a domain.

Example:

```text
example.com
     |
     v
MX Record
     |
     v
mail.example.com
```

---

## 12. TXT Record

A `TXT` record stores text information associated with a domain.

TXT records are commonly used for:

* Domain verification
* SPF
* Email security
* Other configuration information

Example:

```text
example.com
     |
     v
TXT Record
```

---

## 13. NS Record

`NS` stands for **Name Server**.

An `NS` record identifies the authoritative DNS servers for a domain.

Example:

```text
example.com
     |
     v
NS Record
     |
     +---- ns1.example-dns.com
     |
     +---- ns2.example-dns.com
```

---

## 14. PTR Record

A `PTR` record is used for **reverse DNS lookup**.

Normal DNS:

```text
Domain
   ↓
IP Address
```

Reverse DNS:

```text
IP Address
   ↓
Domain Name
```

---

## 15. Forward DNS Lookup

A forward lookup finds an IP address from a domain name.

Example:

```text
example.com
     ↓
IP Address
```

Use:

```bash id="3wsg44"
nslookup example.com
```

---

## 16. Reverse DNS Lookup

A reverse lookup finds a hostname from an IP address.

Use:

```bash id="50c70q"
nslookup 8.8.8.8
```

The result depends on whether a PTR record exists.

---

## 17. nslookup

`nslookup` is a command-line tool used to query DNS information.

Example:

```bash id="1u5b0s"
nslookup example.com
```

You can also query a specific DNS record:

```bash id="3s9vvi"
nslookup -type=MX example.com
```

Query a TXT record:

```bash id="f9u8jb"
nslookup -type=TXT example.com
```

---

## 18. dig

`dig` stands for **Domain Information Groper**.

It is commonly used by DevOps and system administrators for DNS troubleshooting.

Example:

```bash id="s3t7pi"
dig example.com
```

Query an A record:

```bash id="73n5x6"
dig example.com A
```

Query an MX record:

```bash id="0flg8w"
dig example.com MX
```

Query a TXT record:

```bash id="z1lq4x"
dig example.com TXT
```

---

## 19. Check DNS Resolution

Use:

```bash id="n24zq3"
getent hosts example.com
```

This can show the IP address returned by the system's configured name-resolution system.

---

## 20. DNS Configuration in Linux

Linux systems can contain DNS resolver configuration in:

```text
/etc/resolv.conf
```

View it using:

```bash id="w5d1y9"
cat /etc/resolv.conf
```

You may see entries such as:

```text
nameserver 8.8.8.8
```

The exact configuration depends on the Linux distribution and network management system.

---

## 21. DNS vs IP Address

| DNS                            | IP Address                     |
| ------------------------------ | ------------------------------ |
| Converts names to IP addresses | Identifies a network endpoint  |
| Human-friendly                 | Machine/network-friendly       |
| Example: `example.com`         | Example: `93.184.216.34`       |
| Uses DNS records               | Used for network communication |

---

## 22. DNS Troubleshooting

If a website or service cannot be reached, DNS may be one possible cause.

### Step 1: Check the domain

```bash id="yofm9c"
nslookup example.com
```

### Step 2: Use dig

```bash id="k7bq3r"
dig example.com
```

### Step 3: Check DNS configuration

```bash id="v6m0i5"
cat /etc/resolv.conf
```

### Step 4: Check connectivity to the resolved IP

```bash id="0pv6bz"
ping <IP_ADDRESS>
```

Replace `<IP_ADDRESS>` with the IP returned by the DNS lookup.

> A failed `ping` does not always mean DNS is broken because some systems block ICMP traffic.

---

## 23. DNS in AWS

AWS provides DNS-related services and features through services such as **Amazon Route 53**.

A common architecture is:

```text
User
  |
  v
Domain Name
  |
  v
Route 53
  |
  v
Load Balancer / Server
  |
  v
Application
```

For example:

```text
www.example.com
        |
        v
    Route 53
        |
        v
Application Load Balancer
        |
        v
EC2 / Application
```

---

## 24. Practice

### Step 1: Look up a domain

```bash id="l2bqbi"
nslookup example.com
```

### Step 2: Check the A record

```bash id="n6c4qa"
dig example.com A
```

### Step 3: Check MX records

```bash id="9z6e4c"
dig example.com MX
```

### Step 4: Check TXT records

```bash id="76e9g5"
dig example.com TXT
```

### Step 5: Check resolver configuration

```bash id="j89x3u"
cat /etc/resolv.conf
```

### Step 6: Test name resolution

```bash id="v2g1pu"
getent hosts example.com
```

---

## 25. Verification

Run:

```bash id="a1nq2y"
nslookup example.com
```

Then:

```bash id="54f8jq"
dig example.com
```

Then:

```bash id="7e1jms"
cat /etc/resolv.conf
```

If the domain resolves successfully, DNS resolution is working for that test.

---

## Key Takeaways

| Concept            | Meaning                                     |
| ------------------ | ------------------------------------------- |
| DNS                | Converts domain names to IP addresses       |
| DNS Resolver       | Finds DNS information for clients           |
| A                  | IPv4 address record                         |
| AAAA               | IPv6 address record                         |
| CNAME              | Alias for another hostname                  |
| MX                 | Mail server record                          |
| TXT                | Stores text/configuration information       |
| NS                 | Name server record                          |
| PTR                | Reverse DNS record                          |
| `nslookup`         | Queries DNS                                 |
| `dig`              | Detailed DNS troubleshooting tool           |
| `/etc/resolv.conf` | Contains resolver configuration information |

---

## Conclusion

DNS is an important part of networking and cloud infrastructure.

DevOps engineers use DNS when configuring **web applications, AWS Route 53, load balancers, domains, Kubernetes services, and internal infrastructure**.

Understanding DNS helps troubleshoot problems such as domain resolution failures, incorrect records, and services that cannot be reached using their hostname.
