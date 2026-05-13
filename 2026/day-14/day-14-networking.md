# Day 14 – Networking Fundamentals & Hands-on Checks

# Objective

Today I practiced Linux networking fundamentals and troubleshooting commands used in real DevOps environments.

I learned:
- OSI vs TCP/IP models
- Network troubleshooting basics
- DNS, HTTP, TCP/IP flow
- Connectivity and port checks

Target Host Used:
```bash
google.com
```

---

# OSI vs TCP/IP Models

## OSI Model (7 Layers)

| Layer | Purpose |
|-------|----------|
| L7 – Application | HTTP, HTTPS, DNS |
| L6 – Presentation | Encryption, formatting |
| L5 – Session | Session management |
| L4 – Transport | TCP / UDP |
| L3 – Network | IP addressing & routing |
| L2 – Data Link | MAC address & switching |
| L1 – Physical | Cables & hardware |

---

## TCP/IP Model

| Layer | Example |
|-------|----------|
| Application | HTTP, HTTPS, DNS |
| Transport | TCP, UDP |
| Internet | IP |
| Link | Ethernet, Wi-Fi |

---

# Quick Concepts

## Where protocols sit

- IP → Internet/Network layer
- TCP/UDP → Transport layer
- HTTP/HTTPS → Application layer
- DNS → Application layer

---

## Real Example

```bash
curl https://example.com
```

Meaning:
- Application Layer → HTTP/HTTPS
- Transport Layer → TCP
- Internet Layer → IP routing

---

# Hands-on Networking Checks

---

# 1. Identity Check

## Input
```bash
hostname -I
```

## Output
```bash
192.168.1.12
```

### Observation
- Displays local system IP address.
- Useful for identifying the machine on a network.

---

# 2. Reachability Check

## Input
```bash
ping -c 4 google.com
```

## Output
```bash
4 packets transmitted, 4 received, 0% packet loss
time=28ms
```

### Observation
- Internet connectivity working properly.
- Low latency and no packet loss observed.

---

# 3. Path Check

## Input
```bash
tracepath google.com
```

## Output
```bash
1?: [LOCALHOST] pmtu 1500
1: 192.168.1.1
2: isp-gateway
3: google-network
```

### Observation
- Packets successfully routed through ISP network.
- No major delays or failures found.

---

# 4. Check Listening Ports

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
- SSH service listening on port 22.
- Nginx web server listening on port 80.

---

# 5. Name Resolution Check

## Input
```bash
nslookup google.com
```

## Output
```bash
Address: 142.250.182.14
```

### Observation
- DNS resolution working successfully.

---

# 6. HTTP Header Check

## Input
```bash
curl -I https://google.com
```

## Output
```bash
HTTP/2 200
```

### Observation
- Website reachable successfully.
- HTTP status code 200 means request succeeded.

---

# 7. Connections Snapshot

## Input
```bash
netstat -an | head
```

## Output
```bash
tcp 0 0 127.0.0.1:22 LISTEN
tcp 0 0 192.168.1.12:443 ESTABLISHED
```

### Observation
- Both listening and active connections visible.
- Useful during troubleshooting.

---

# Mini Task – Port Probe & Interpretation

## Step 1 – Identify Listening Port

### Port Found
```bash
22 (SSH)
```

---

## Step 2 – Test Port Reachability

### Input
```bash
nc -zv localhost 22
```

### Output
```bash
Connection to localhost 22 port [tcp/ssh] succeeded!
```

### Observation
- SSH service reachable successfully.

---

## If Port Was Not Reachable

Next checks:
```bash
systemctl status ssh
sudo ufw status
```

---

# Reflection

## Which command gives the fastest signal when something is broken?

### My Answer
```bash
ping
```

Reason:
- Quickly checks network connectivity and packet loss.

---

## If DNS fails, which layer would you inspect?

### Answer
- Application Layer (DNS service)
- Internet Layer (IP routing)

---

## If HTTP 500 error appears, which layer would you inspect?

### Answer
- Application Layer
- Web server logs and backend service status

---

## Two follow-up checks during a real incident

### Check Logs
```bash
journalctl -u nginx
```

### Check Service Status
```bash
systemctl status nginx
```

---

# Commands Used

```bash
hostname -I
ping
tracepath
ss -tulpn
nslookup
curl -I
netstat
nc
```

---

# What I Learned

- Networking troubleshooting starts with connectivity checks.
- DNS and HTTP issues happen at different layers.
- `ping`, `curl`, and `ss` are essential troubleshooting commands.
- Ports help identify running services.
- Understanding OSI/TCP-IP improves debugging skills.

---

# Final Summary

Today I practiced:
- Linux networking fundamentals
- Connectivity troubleshooting
- DNS and HTTP testing
- Port inspection and probing
- OSI & TCP/IP concepts

This challenge improved my confidence in debugging network-related issues in Linux and DevOps environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
