# Day 05 – Linux Troubleshooting Drill: CPU, Memory, and Logs

## Target Service / Process
Service selected: **SSH (sshd)**

Purpose: Perform a quick troubleshooting drill to check system health and inspect the SSH service using Linux diagnostic commands.

---

# 1. Environment Basics

### Command 1
```bash
uname -a
```

Example Output
```
Linux ubuntu 6.8.0-31-generic #31-Ubuntu SMP x86_64 GNU/Linux
```

Observation  
Shows kernel version, architecture, and system details.

---

### Command 2
```bash
cat /etc/os-release
```

Example Output
```
NAME="Ubuntu"
VERSION="24.04 LTS"
ID=ubuntu
```

Observation  
Confirms OS version and distribution details.

---

# 2. Filesystem Sanity Check

### Command 3
```bash
mkdir /tmp/runbook-demo
```

Observation  
Created a temporary directory for testing file operations.

---

### Command 4
```bash
cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
```

Example Output
```
-rw-r--r-- 1 root root 300 hosts-copy
```

Observation  
Successfully copied a system file to the test directory.

---

# 3. CPU & Memory Snapshot

### Command 5
```bash
top
```

Example Output
```
top - 19:45:12 up 2:03, 1 user, load average: 0.15, 0.10, 0.05
Tasks: 100 total, 1 running, 99 sleeping
%Cpu(s): 2.0 us, 1.0 sy
```

Observation  
CPU usage is low and system load is normal.

---

### Command 6
```bash
free -h
```

Example Output
```
              total        used        free
Mem:          2.0Gi       600Mi       1.2Gi
Swap:         1.0Gi       0.0Gi       1.0Gi
```

Observation  
Memory usage is normal and sufficient free memory is available.

---

# 4. Disk & IO Snapshot

### Command 7
```bash
df -h
```

Example Output
```
Filesystem      Size  Used Avail Use%
/dev/sda1        20G  6.2G   12G  35%
```

Observation  
Disk usage is healthy with plenty of free space.

---

### Command 8
```bash
du -sh /var/log
```

Example Output
```
120M /var/log
```

Observation  
Log directory size is moderate and not consuming excessive disk space.

---

# 5. Network Snapshot

### Command 9
```bash
ss -tulpn
```

Example Output
```
tcp LISTEN 0 128 0.0.0.0:22 users:(("sshd",pid=1234))
```

Observation  
SSH service is listening on port **22**.

---

### Command 10
```bash
curl -I http://localhost
```

Example Output
```
HTTP/1.1 200 OK
```

Observation  
Network connectivity to the local service is working.

---

# 6. Logs Reviewed

### Command 11
```bash
journalctl -u ssh -n 50
```

Example Output
```
Mar 14 19:30:11 ubuntu sshd[1050]: Accepted password for ubuntu
Mar 14 19:32:15 ubuntu sshd[1052]: Connection closed
```

Observation  
Recent SSH login activity recorded. No critical errors found.

---

### Command 12
```bash
tail -n 50 /var/log/syslog
```

Example Output
```
Mar 14 19:40:22 ubuntu systemd[1]: Started Session 3 of user ubuntu.
```

Observation  
System logs show normal operations without warnings.

---

# Quick Findings

- CPU usage is low and system load is stable.
- Memory usage is within safe limits.
- Disk space is available and logs are not oversized.
- SSH service is running and listening on port 22.
- No major errors found in recent system logs.

---

# If This Worsens (Next Steps)

1. Restart the affected service
```
sudo systemctl restart ssh
```

2. Increase logging and inspect deeper logs
```
journalctl -u ssh -f
```

3. Trace system calls or inspect process behavior
```
strace -p <pid>
```

---

# Summary

This troubleshooting drill captured a quick health snapshot of CPU, memory, disk, network, and logs.  
The SSH service was inspected and confirmed to be running normally.  
This runbook provides a repeatable checklist for diagnosing Linux service issues quickly.
