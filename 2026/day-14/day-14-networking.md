# Day 14 – Networking Fundamentals & Hands-on Checks

## Objective
Understand basic networking concepts and practice Linux networking commands used during troubleshooting.

---

# Networking Concepts

## OSI Model vs TCP/IP Model

### OSI Model (7 Layers)

| Layer | Name | Example |
|------|------|------|
| L7 | Application | HTTP, HTTPS, DNS |
| L6 | Presentation | Encryption (SSL/TLS) |
| L5 | Session | Session management |
| L4 | Transport | TCP, UDP |
| L3 | Network | IP |
| L2 | Data Link | Ethernet |
| L1 | Physical | Cables, signals |

### TCP/IP Model

| Layer | Example Protocols |
|------|------|
| Application | HTTP, HTTPS, DNS |
| Transport | TCP, UDP |
| Internet | IP |
| Network Access | Ethernet |

---

## Where Protocols Sit

| Protocol | Layer |
|------|------|
| HTTP / HTTPS | Application Layer |
| DNS | Application Layer |
| TCP / UDP | Transport Layer |
| IP | Network Layer |

Example:

```
curl https://example.com
```

This request goes through:

```
Application (HTTP)
Transport (TCP)
Internet (IP)
Network Access (Ethernet)
```

---

# Hands-on Networking Checks

Target host used for testing:

```
google.com
```

---

# Identity Check

Command:

```bash
hostname -I
```

Example Output

```
192.168.1.10
```

Observation  
Shows the system's local IP address.

Alternative command:

```bash
ip addr show
```

---

# Reachability Test

Command:

```bash
ping google.com
```

Example Output

```
64 bytes from google.com: icmp_seq=1 ttl=118 time=20.3 ms
64 bytes from google.com: icmp_seq=2 ttl=118 time=19.8 ms
```

Observation  
Host is reachable with low latency and no packet loss.

---

# Network Path

Command:

```bash
traceroute google.com
```

or

```bash
tracepath google.com
```

Observation  
Shows the route packets take to reach the destination server.

This helps identify slow or failing network hops.

---

# Listening Ports

Command:

```bash
ss -tulpn
```

Example Output

```
tcp LISTEN 0 128 0.0.0.0:22 users:(("sshd",pid=756))
```

Observation  
SSH service is listening on port **22**.

Alternative command:

```bash
netstat -tulpn
```

---

# Name Resolution

Command:

```bash
dig google.com
```

or

```bash
nslookup google.com
```

Example Output

```
google.com.    300 IN A 142.250.182.14
```

Observation  
DNS successfully resolved domain name to an IP address.

---

# HTTP Check

Command:

```bash
curl -I https://google.com
```

Example Output

```
HTTP/2 200
content-type: text/html
```

Observation  
HTTP response **200 OK** indicates the server is reachable and responding.

---

# Connections Snapshot

Command:

```bash
netstat -an | head
```

Example Output

```
tcp 0 0 127.0.0.1:631 LISTEN
tcp 0 0 0.0.0.0:22 LISTEN
tcp 0 0 192.168.1.10:22 ESTABLISHED
```

Observation  
Shows active connections and listening ports.

---

# Mini Task – Port Probe

Identify listening port:

From `ss -tulpn`:

```
SSH running on port 22
```

Test connectivity:

```bash
nc -zv localhost 22
```

Example Output

```
Connection to localhost 22 port [tcp/ssh] succeeded!
```

Observation  
Port is reachable.

Next check if unreachable:

```
systemctl status ssh
sudo ufw status
```

---

# Reflection

### Which command gives the fastest signal when something breaks?

`ping` and `curl` provide quick feedback to check connectivity and service availability.

---

### If DNS fails, which layer should you inspect?

Application layer (DNS service).

Commands to troubleshoot:

```
dig domain.com
nslookup domain.com
```

---

### If HTTP 500 error appears?

Check application and server logs.

Example:

```
journalctl -u nginx
tail -f /var/log/nginx/error.log
```

---

# Follow-up Checks in Real Incidents

1. Check service status

```bash
systemctl status nginx
```

2. Check firewall rules

```bash
sudo ufw status
```

3. Inspect logs

```bash
journalctl -xe
```

---

# Commands Used

```
hostname -I
ip addr
ping
traceroute
tracepath
ss
netstat
dig
nslookup
curl
nc
```

---

# What I Learned

1. Networking troubleshooting often starts with simple commands like **ping and curl**.
2. Tools like **ss and netstat** help identify listening services and open ports.
3. DNS resolution is a critical step before any HTTP communication happens.

---

# Summary

In this exercise, I explored networking fundamentals and practiced essential troubleshooting commands. Understanding networking layers and using commands like ping, traceroute, curl, and ss helps diagnose connectivity and service issues quickly in real-world DevOps environments.
