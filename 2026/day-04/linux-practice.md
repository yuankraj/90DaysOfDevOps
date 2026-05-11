# Day 04 – Linux Practice: Processes and Services

## Process Checks

### 1. Check Running Processes

#### Input
```bash
ps aux | head
```

#### Output
```bash
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 169084 11320 ?        Ss   09:10   0:01 /sbin/init
root       632  0.0  0.2  50000 15000 ?        Ss   09:10   0:00 systemd-journald
user      1520  0.1  1.5 145000 32000 pts/0    S+   10:20   0:00 bash
```

#### Learning
- `ps aux` displays all running processes.
- Helps identify active programs and system tasks.

---

### 2. Monitor Live Processes

#### Input
```bash
top
```

#### Output
```bash
top - 10:30:11 up 1:20, 1 user, load average: 0.20, 0.15, 0.10
Tasks: 132 total,   1 running, 131 sleeping
%Cpu(s):  5.0 us,  2.0 sy, 93.0 id
MiB Mem :   3900 total,   1200 free
```

#### Learning
- `top` shows real-time CPU and memory usage.
- Useful for monitoring system performance.

---

# Service Checks

### 3. Check SSH Service Status

#### Input
```bash
systemctl status ssh
```

#### Output
```bash
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service)
     Active: active (running) since Mon 2026-05-11 09:10:01
```

#### Learning
- `systemctl status` checks service health.
- Confirms whether a service is running or stopped.

---

### 4. List Running Services

#### Input
```bash
systemctl list-units --type=service --state=running
```

#### Output
```bash
UNIT                         LOAD   ACTIVE SUB     DESCRIPTION
cron.service                 loaded active running Regular background program processing daemon
ssh.service                  loaded active running OpenBSD Secure Shell server
systemd-journald.service     loaded active running Journal Service
```

#### Learning
- Shows all active system services.
- Useful during troubleshooting.

---

# Log Checks

### 5. View SSH Service Logs

#### Input
```bash
journalctl -u ssh --no-pager | tail -n 10
```

#### Output
```bash
May 11 10:15:21 server sshd[1221]: Accepted password for user
May 11 10:15:23 server sshd[1221]: session opened for user
```

#### Learning
- `journalctl` displays logs collected by systemd.
- Helps analyze service-related issues.

---

### 6. Check System Logs

#### Input
```bash
tail -n 20 /var/log/syslog
```

#### Output
```bash
May 11 10:25:10 server CRON[2100]: pam_unix(cron:session): session opened
May 11 10:25:11 server CRON[2100]: session closed
```

#### Learning
- `tail` displays recent log entries.
- Useful for quick log inspection.

---

# Mini Troubleshooting Flow

## Problem
SSH service was not responding properly.

---

### Step 1 — Check Service Status

#### Input
```bash
systemctl status ssh
```

#### Output
```bash
Active: inactive (dead)
```

---

### Step 2 — Restart SSH Service

#### Input
```bash
sudo systemctl restart ssh
```

#### Output
```bash
(No output — service restarted successfully)
```

---

### Step 3 — Verify Service Again

#### Input
```bash
systemctl status ssh
```

#### Output
```bash
Active: active (running)
```

---

### Step 4 — Check Logs

#### Input
```bash
journalctl -u ssh --no-pager | tail
```

#### Output
```bash
May 11 10:35:20 server systemd[1]: Started OpenBSD Secure Shell server.
```

---

# Summary

Today I practiced:
- Linux process monitoring
- systemd service management
- Log inspection and troubleshooting

Commands practiced:
- ps
- top
- systemctl
- journalctl
- tail

This hands-on practice improved my Linux fundamentals for DevOps.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
