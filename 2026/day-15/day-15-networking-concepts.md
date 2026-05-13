# Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

# Objective

Today I learned the core networking concepts every DevOps engineer should understand:
- DNS resolution
- IP addressing
- CIDR & subnetting
- Ports and services

This helped me understand how systems communicate across networks.

---

# Task 1 – DNS: How Names Become IPs

## 1. What happens when we type google.com in a browser?

- The browser first checks local DNS cache.
- If not found, it sends a DNS request to a DNS server.
- DNS resolves `google.com` into an IP address.
- The browser then connects to that IP using TCP/IP and HTTP/HTTPS.

---

# DNS Record Types

| Record | Purpose |
|--------|----------|
| A | Maps domain name to IPv4 address |
| AAAA | Maps domain name to IPv6 address |
| CNAME | Alias from one domain to another |
| MX | Mail server record |
| NS | Name server record |

---

# Run dig Command

## Input
```bash
dig google.com
```

## Output
```bash
google.com.    300    IN    A    142.250.182.14
```

### Observation
- A Record → `142.250.182.14`
- TTL → `300`

---

# Task 2 – IP Addressing

## What is an IPv4 Address?

- IPv4 is a 32-bit address used to identify devices on a network.
- It is written in dotted decimal format.

Example:
```bash
192.168.1.10
```

---

# Public vs Private IP

| Type | Example | Purpose |
|------|----------|----------|
| Public IP | 8.8.8.8 | Internet communication |
| Private IP | 192.168.1.10 | Internal/local network |

---

# Private IP Ranges

```bash
10.0.0.0 – 10.255.255.255
172.16.0.0 – 172.31.255.255
192.168.0.0 – 192.168.255.255
```

---

# Check IP Addresses

## Input
```bash
ip addr show
```

## Output
```bash
inet 192.168.1.12/24
inet 127.0.0.1/8
```

### Observation
- `192.168.1.12` is a private IP address.
- `127.0.0.1` is loopback/localhost.

---

# Task 3 – CIDR & Subnetting

## What does /24 mean?

```bash
192.168.1.0/24
```

- `/24` means first 24 bits are network bits.
- Remaining 8 bits are for hosts.

---

# Usable Hosts

| CIDR | Total IPs | Usable Hosts |
|------|------------|--------------|
| /24 | 256 | 254 |
| /16 | 65,536 | 65,534 |
| /28 | 16 | 14 |

---

# Why do we subnet?

- Subnetting divides networks into smaller sections.
- Improves security and traffic management.
- Helps organize large infrastructures efficiently.

---

# CIDR Table

| CIDR | Subnet Mask | Total IPs | Usable Hosts |
|------|-------------|-----------|--------------|
| /24 | 255.255.255.0 | 256 | 254 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /28 | 255.255.255.240 | 16 | 14 |

---

# Task 4 – Ports: The Doors to Services

## What is a Port?

- A port identifies a specific service running on a device.
- Ports allow multiple services to use the same IP address.

---

# Common Ports

| Port | Service |
|------|----------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 53 | DNS |
| 3306 | MySQL |
| 6379 | Redis |
| 27017 | MongoDB |

---

# Check Listening Ports

## Input
```bash
ss -tulpn
```

## Output
```bash
tcp LISTEN 0 128 0.0.0.0:22
tcp LISTEN 0 511 0.0.0.0:80
```

### Observation
- Port 22 → SSH service
- Port 80 → Nginx web server

---

# Task 5 – Putting It Together

## Question 1

### Command
```bash
curl http://myapp.com:8080
```

### Networking Concepts Involved
- DNS resolves `myapp.com`
- TCP connection established
- IP routing sends packets
- Port `8080` identifies the application service
- HTTP protocol transfers data

---

## Question 2

### Database Connection Problem
```bash
10.0.1.50:3306
```

### First Checks
- Verify database service is running
- Check network connectivity using `ping`
- Verify port 3306 is open
- Check firewall/security group rules

---

# Commands Used

```bash
dig
ip addr show
ss -tulpn
curl
ping
```

---

# What I Learned

- DNS converts domain names into IP addresses.
- CIDR helps divide networks efficiently.
- Ports identify specific services running on servers.
- Public and private IPs have different purposes.
- Networking concepts are critical for DevOps troubleshooting.

---

# Final Summary

Today I learned:
- DNS resolution flow
- IPv4 addressing basics
- CIDR and subnetting concepts
- Common networking ports
- How services communicate over networks

This challenge improved my understanding of networking fundamentals used daily in DevOps and cloud environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
