# Security Groups

## Objective

Understand how AWS Security Groups control network traffic to and from an EC2 instance.

## What is a Security Group?

A Security Group is a **virtual firewall** attached to an AWS resource such as an EC2 instance.

It controls:

* **Inbound traffic** → traffic coming into the EC2 instance
* **Outbound traffic** → traffic going out from the EC2 instance

## Inbound Rules

Inbound rules define which traffic is allowed to reach the EC2 instance.

Example:

| Type  | Protocol | Port | Source    | Purpose       |
| ----- | -------- | ---: | --------- | ------------- |
| SSH   | TCP      |   22 | My IP     | SSH access    |
| HTTP  | TCP      |   80 | 0.0.0.0/0 | Access Nginx  |
| HTTPS | TCP      |  443 | 0.0.0.0/0 | HTTPS traffic |

## Outbound Rules

Outbound rules control traffic leaving the EC2 instance.

By default, AWS commonly allows all outbound traffic.

Example:

```text
EC2 → Internet
```

## Important Points

* Security Groups are **stateful**.
* Security Groups contain **allow rules only**.
* There is no explicit deny rule in a Security Group.
* If traffic is not allowed by an inbound rule, it is blocked.
* A Security Group can be attached to one or more EC2 instances.
* Security Groups work at the **instance/network-interface level**.

## Example

If the EC2 instance has:

```text
SSH  → Port 22  → My IP
HTTP → Port 80  → Anywhere
```

Then:

```text
My computer → Port 22 → EC2     ✅ Allowed
Internet    → Port 80 → EC2     ✅ Allowed
Internet    → Port 22 → EC2     ❌ Blocked
```

## Verification

Check the Security Group attached to the EC2 instance from:

```text
AWS Console
→ EC2
→ Instances
→ Select Instance
→ Security
→ Security Groups
```

Verify that the required inbound rules are present.

## Security Best Practice

Do not expose SSH to the entire internet when it is not necessary.

Prefer:

```text
SSH → Port 22 → My IP
```

instead of:

```text
SSH → Port 22 → 0.0.0.0/0
```

For a web server, HTTP port 80 can normally be publicly accessible when the website is intended to be public.

## Troubleshooting

### Cannot access Nginx

Check:

```text
Security Group → Port 80 → HTTP → Allowed
```

### Cannot SSH into EC2

Check:

```text
Security Group → Port 22 → SSH → Your IP
```

Also verify that the EC2 instance is running and that the SSH key is correct.
