# Day 04 – Linux Practice: Processes and Services

## 1. Process Checks

### Command 1: View Running Processes
```bash
ps aux | head
```

Example Output:
```
USER       PID %CPU %MEM COMMAND
root         1  0.0  0.1 /sbin/init
root       234  0.0  0.2 /usr/lib/systemd/systemd-journald
ubuntu     521  0.1  0.3 /usr/bin/python3
```

Observation:
- `ps aux` lists all running processes.
- Shows process owner, PID, CPU usage, and command.

---

### Command 2: Monitor Processes in Real Time
```bash
top
```

Example Output:
```
top - 20:21:55 up 1:10,  1 user,  load average: 0.00, 0.01, 0.05
Tasks: 95 total,   1 running, 94 sleeping
%Cpu(s): 1.2 us, 0.3 sy
MiB Mem : 1985.6 total
```

Observation:
- `top` displays real-time system usage.
- Useful for monitoring CPU and memory usage.

---

## 2. Service Checks

Service selected for inspection: **SSH**

### Command 3: Check Service Status
```bash
systemctl status ssh
```

Example Output:
```
● ssh.service - OpenSSH server daemon
Loaded: loaded (/lib/systemd/system/ssh.service)
Active: active (running)
```

Observation:
- Confirms whether SSH service is active and running.

---

### Command 4: List Active Services
```bash
systemctl list-units --type=service
```

Example Output:
```
UNIT                 LOAD   ACTIVE SUB     DESCRIPTION
cron.service         loaded active running Regular background program processing daemon
ssh.service          loaded active running OpenBSD Secure Shell server
docker.service       loaded active running Docker Application Container Engine
```

Observation:
- Displays currently running system services.

---

## 3. Log Checks

### Command 5: View SSH Service Logs
```bash
journalctl -u ssh --no-pager | tail -n 10
```

Example Output:
```
Mar 14 19:10:12 ubuntu sshd[1023]: Server listening on port 22
Mar 14 19:11:01 ubuntu sshd[1034]: Accepted password for ubuntu
```

Observation:
- Shows recent logs related to the SSH service.

---

### Command 6: View System Logs
```bash
tail -n 20 /var/log/syslog
```

Example Output:
```
Mar 14 19:15:33 ubuntu systemd[1]: Started Session 2 of user ubuntu.
Mar 14 19:16:10 ubuntu CRON[1402]: (root) CMD (run-parts /etc/cron.hourly)
```

Observation:
- Displays recent system log entries for troubleshooting.

---

## 4. Mini Troubleshooting Flow

Example: Checking if the SSH service is running.

Step 1: Check service status
```bash
systemctl status ssh
```

Step 2: Start the service if it is stopped
```bash
sudo systemctl start ssh
```

Step 3: Enable service to start automatically on boot
```bash
sudo systemctl enable ssh
```

Step 4: Check logs if the service fails
```bash
journalctl -u ssh
```

---

## Commands Used

| Category | Commands |
|---------|----------|
| Process | ps, top |
| Service | systemctl status, systemctl list-units |
| Logs | journalctl, tail |

---

## Summary

In this practice, I checked running processes, inspected system services, and reviewed system logs. These commands help monitor system performance and troubleshoot service-related issues in Linux systems.
