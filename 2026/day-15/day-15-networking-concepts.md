# Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

## Objective
Understand the core networking concepts that every DevOps engineer should know, including DNS resolution, IP addressing, subnetting, and service ports.

---

# Task 1 – DNS: How Names Become IPs

### What happens when you type `google.com` in a browser?

1. The browser asks the system’s DNS resolver to find the IP address of `google.com`.
2. The resolver queries DNS servers starting from root servers to authoritative servers.
3. The authoritative DNS server returns the IP address associated with the domain.
4. The browser then connects to that IP address using HTTP or HTTPS.

---

### DNS Record Types

| Record | Description |
|------|-------------|
| A | Maps a domain name to an IPv4 address |
| AAAA | Maps a domain name to an IPv6 address |
| CNAME | Alias that points one domain name to another |
| MX | Specifies mail servers responsible for receiving emails |
| NS | Indicates authoritative name servers for the domain |

---

### DNS Lookup

Command:

```bash
dig google.com
```

Example Output

```
google.com.  300 IN A 142.250.182.14
```

Observation

- **A record:** 142.250.182.14
- **TTL:** 300 seconds

TTL determines how long DNS results are cached.

---

# Task 2 – IP Addressing

### What is an IPv4 address?

An IPv4 address is a **32-bit numeric identifier** assigned to a device on a network.  
It is written in **four octets separated by dots**, for example:

```
192.168.1.10
```

Each octet ranges from **0–255**.

---

### Public vs Private IP

| Type | Description | Example |
|-----|-------------|---------|
| Public IP | Accessible from the internet | 8.8.8.8 |
| Private IP | Used within internal networks | 192.168.1.10 |

---

### Private IP Ranges

```
10.0.0.0 – 10.255.255.255
172.16.0.0 – 172.31.255.255
192.168.0.0 – 192.168.255.255
```

---

### Check Local IP Address

Command:

```bash
ip addr show
```

Example Output

```
inet 192.168.1.10/24 brd 192.168.1.255 scope global
```

Observation

This IP is within **192.168.x.x**, which is a private IP range.

---

# Task 3 – CIDR & Subnetting

### What does `/24` mean?

`192.168.1.0/24` means **24 bits are used for the network portion** of the address.  
The remaining bits are available for hosts.

---

### Number of Hosts

| CIDR | Subnet Mask | Total IPs | Usable Hosts |
|------|-------------|-----------|--------------|
| /24 | 255.255.255.0 | 256 | 254 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /28 | 255.255.255.240 | 16 | 14 |

---

### Why Do We Subnet?

Subnetting helps:

- Organize networks into smaller segments
- Improve network performance
- Increase security and traffic isolation
- Efficiently allocate IP addresses

---

# Task 4 – Ports: The Doors to Services

### What is a Port?

A port is a **logical communication endpoint** used by applications to send and receive network traffic.

Ports allow multiple services to run on the same machine.

Example:

```
IP Address = device
Port = specific service
```

---

### Common Ports

| Port | Service |
|------|--------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 53 | DNS |
| 3306 | MySQL |
| 6379 | Redis |
| 27017 | MongoDB |

---

### Check Listening Ports

Command:

```bash
ss -tulpn
```

Example Output

```
tcp LISTEN 0 128 0.0.0.0:22 users:(("sshd",pid=756))
tcp LISTEN 0 128 0.0.0.0:80 users:(("nginx",pid=1021))
```

Observation

- Port **22** → SSH service
- Port **80** → Nginx web server

---

# Task 5 – Putting It Together

### Scenario 1

Command:

```
curl http://myapp.com:8080
```

Networking concepts involved:

- DNS resolves **myapp.com → IP address**
- TCP establishes a connection
- Port **8080** identifies the application service
- HTTP request is sent to the web server

---

### Scenario 2

Database connection issue:

```
10.0.1.50:3306
```

Checks to perform:

1. Verify database service is running
2. Check port accessibility using:

```bash
ss -tulpn
```

3. Check firewall or security group rules
4. Verify network connectivity using:

```bash
ping 10.0.1.50
```

---

# Commands Used

```
dig
ip addr show
ss -tulpn
ping
curl
```

---

# What I Learned

1. DNS translates human-friendly domain names into machine-readable IP addresses.
2. IP addressing and subnetting help organize and manage networks efficiently.
3. Ports allow multiple services to run on the same server and handle different types of traffic.

---

# Summary

In this exercise, I explored key networking concepts such as DNS resolution, IPv4 addressing, subnetting with CIDR notation, and service ports. Understanding these concepts helps DevOps engineers troubleshoot connectivity issues and manage networked applications effectively.
