# Day 05 – Linux Troubleshooting Drill: CPU, Memory, and Logs

# Target Service / Process

Target Service:
```bash
ssh.service
```

Purpose:
- Investigate system health and troubleshoot SSH service using Linux monitoring and logging tools.

---

# Environment Basics

## 1. Check Kernel and System Information

### Input
```bash
uname -a
```

### Output
```bash
Linux ubuntu 6.8.0-31-generic x86_64 GNU/Linux
```

### Observation
- System is running Ubuntu Linux with a 64-bit kernel.
- Kernel version looks stable and updated.

---

## 2. Check OS Information

### Input
```bash
cat /etc/os-release
```

### Output
```bash
NAME="Ubuntu"
VERSION="24.04 LTS"
ID=ubuntu
```

### Observation
- Confirmed Ubuntu 24.04 LTS environment.
- Useful during troubleshooting and compatibility checks.

---

# Filesystem Sanity Checks

## 3. Create Temporary Runbook Directory

### Input
```bash
mkdir /tmp/runbook-demo
```

### Output
```bash
Directory created successfully
```

### Observation
- Temporary directory created for practice and testing.

---

## 4. Copy and Verify File

### Input
```bash
cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
```

### Output
```bash
-rw-r--r-- 1 user user 240 May 11 11:10 hosts-copy
```

### Observation
- File copied successfully.
- Filesystem is writable and working normally.

---

# Snapshot: CPU & Memory

## 5. Monitor Running Processes

### Input
```bash
top
```

### Output
```bash
top - 11:15:01 up 2:10, 1 user, load average: 0.20, 0.18, 0.15
Tasks: 130 total, 1 running, 129 sleeping
%Cpu(s): 6.0 us, 2.0 sy, 92.0 id
```

### Observation
- CPU usage is low and system is stable.
- No abnormal resource spikes detected.

---

## 6. Check Memory Usage

### Input
```bash
free -h
```

### Output
```bash
Mem: 3.8Gi total, 1.2Gi used, 2.0Gi free
Swap: 2.0Gi total, 0B used
```

### Observation
- Sufficient free memory available.
- Swap usage is zero, indicating healthy memory state.

---

# Snapshot: Disk & IO

## 7. Check Disk Usage

### Input
```bash
df -h
```

### Output
```bash
Filesystem      Size  Used Avail Use%
/dev/sda1        50G   18G   30G  38%
```

### Observation
- Disk utilization is normal.
- Enough free storage available.

---

## 8. Check Log Directory Size

### Input
```bash
du -sh /var/log
```

### Output
```bash
220M    /var/log
```

### Observation
- Log directory size is manageable.
- No excessive log growth observed.

---

# Snapshot: Network

## 9. Check Listening Ports

### Input
```bash
ss -tulpn
```

### Output
```bash
tcp   LISTEN 0 128 0.0.0.0:22  0.0.0.0:* users:(("sshd",pid=722))
```

### Observation
- SSH service is actively listening on port 22.
- Network connectivity for SSH appears healthy.

---

## 10. Test Network Connectivity

### Input
```bash
ping -c 4 google.com
```

### Output
```bash
4 packets transmitted, 4 received, 0% packet loss
```

### Observation
- Internet connectivity is stable.
- No packet loss detected.

---

# Logs Reviewed

## 11. Review SSH Service Logs

### Input
```bash
journalctl -u ssh -n 20
```

### Output
```bash
May 11 11:20:10 systemd[1]: Started OpenBSD Secure Shell server.
```

### Observation
- SSH service started successfully.
- No recent service failures found.

---

## 12. Review System Logs

### Input
```bash
tail -n 20 /var/log/syslog
```

### Output
```bash
May 11 11:22:01 CRON[1220]: session opened for user root
```

### Observation
- System logs look normal.
- No critical warnings or errors identified.

---

# Quick Findings

- CPU and memory utilization are healthy.
- SSH service is active and listening properly.
- Disk space is sufficient.
- Network connectivity is stable.
- No major issues found in recent logs.

---

# If This Worsens (Next Steps)

## 1. Restart SSH Service Safely
```bash
sudo systemctl restart ssh
```
- Restart service if SSH becomes unresponsive.

---

## 2. Increase Log Monitoring
```bash
journalctl -u ssh -f
```
- Monitor logs live for authentication or crash issues.

---

## 3. Investigate Deep System Activity
```bash
strace -p <PID>
```
- Trace process-level system calls if the service hangs.

---

# Summary

Today I practiced:
- Linux troubleshooting workflow
- CPU and memory monitoring
- Disk and network inspection
- Service log analysis
- Building a basic incident runbook

Commands Practiced:
- uname
- cat /etc/os-release
- mkdir
- cp
- top
- free
- df
- du
- ss
- ping
- journalctl
- tail

This drill improved my confidence in handling Linux troubleshooting scenarios for DevOps environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
