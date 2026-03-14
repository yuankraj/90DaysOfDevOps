# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

## Part 1: Linux File System Hierarchy

### / (Root Directory)

**Purpose**  
The root directory is the starting point of the entire Linux filesystem.  
All directories and files originate from `/`.

**Command**
```bash
ls -l /
```

**Example Output**
```
bin   boot  dev  etc  home  lib  opt  root  tmp  usr  var
```

**Observation**  
This directory contains core system folders such as `/etc`, `/home`, `/usr`, and `/var`.

**I would use this when:**  
Navigating the system structure or locating system directories.

---

### /home

**Purpose**  
Contains personal directories for all users on the system.

**Command**
```bash
ls -l /home
```

**Example Output**
```
drwxr-xr-x  3 ubuntu ubuntu 4096 Mar 20 ubuntu
```

**Observation**  
Each user has their own directory under `/home`.

**I would use this when:**  
Managing user files or checking user environments.

---

### /root

**Purpose**  
Home directory of the root (administrator) user.

**Command**
```bash
ls -l /root
```

**Example Output**
```
-rw------- 1 root root  310 .bashrc
-rw------- 1 root root  161 .profile
```

**Observation**  
Contains configuration files specific to the root user.

**I would use this when:**  
Working with administrative scripts or root-specific configurations.

---

### /etc

**Purpose**  
Stores system-wide configuration files.

**Command**
```bash
ls -l /etc
```

**Example Output**
```
hostname
hosts
passwd
ssh
systemd
```

**Observation**  
Configuration files for services and the system are stored here.

**I would use this when:**  
Editing system or service configuration files.

---

### /var/log

**Purpose**  
Stores system and application log files.

**Command**
```bash
ls -l /var/log
```

**Example Output**
```
syslog
auth.log
kern.log
```

**Observation**  
Important logs for debugging system and application issues.

**I would use this when:**  
Investigating errors or troubleshooting system problems.

---

### /tmp

**Purpose**  
Used to store temporary files created by applications.

**Command**
```bash
ls -l /tmp
```

**Example Output**
```
runbook-demo
temp-file.txt
```

**Observation**  
Files here are temporary and often cleared after reboot.

**I would use this when:**  
Storing temporary test files or intermediate data.

---

### /bin

**Purpose**  
Contains essential command binaries needed for system operation.

**Command**
```bash
ls -l /bin
```

**Example Output**
```
bash
cat
cp
ls
mkdir
```

**Observation**  
Basic commands used in Linux are located here.

**I would use this when:**  
Understanding where core Linux commands are stored.

---

### /usr/bin

**Purpose**  
Contains user-level command binaries and applications.

**Command**
```bash
ls -l /usr/bin
```

**Example Output**
```
python3
git
vim
nano
```

**Observation**  
Most user commands and programs are located here.

**I would use this when:**  
Checking installed command-line tools.

---

### /opt

**Purpose**  
Used for optional or third-party software installations.

**Command**
```bash
ls -l /opt
```

**Example Output**
```
docker
custom-app
```

**Observation**  
External applications or manually installed software may appear here.

**I would use this when:**  
Installing third-party software.

---

## Hands-On Commands

### Find largest log files
```bash
du -sh /var/log/* 2>/dev/null | sort -h | tail -5
```

Observation  
Helps identify large log files that may consume disk space.

---

### Check hostname configuration
```bash
cat /etc/hostname
```

Observation  
Displays the system hostname.

---

### View home directory contents
```bash
ls -la ~
```

Observation  
Shows files and hidden configuration files in the user’s home directory.

---

# Part 2: Scenario-Based Practice

---

## Scenario 1: Service Not Starting

**Step 1**
```bash
systemctl status myapp
```

Why:  
Checks whether the service is running, stopped, or failed.

---

**Step 2**
```bash
journalctl -u myapp -n 50
```

Why:  
Displays the latest logs related to the service.

---

**Step 3**
```bash
systemctl is-enabled myapp
```

Why:  
Checks whether the service is enabled to start automatically on boot.

---

**Step 4**
```bash
systemctl restart myapp
```

Why:  
Attempts to restart the service after diagnosing the issue.

---

## Scenario 2: High CPU Usage

**Step 1**
```bash
top
```

Why:  
Displays live CPU and memory usage.

---

**Step 2**
```bash
ps aux --sort=-%cpu | head -10
```

Why:  
Shows processes sorted by CPU usage.

---

**Step 3**
```bash
htop
```

Why:  
Provides an interactive process viewer for better monitoring.

---

## Scenario 3: Finding Service Logs

**Step 1**
```bash
systemctl status docker
```

Why:  
Checks if the docker service is running.

---

**Step 2**
```bash
journalctl -u docker -n 50
```

Why:  
Displays the last 50 log entries for the docker service.

---

**Step 3**
```bash
journalctl -u docker -f
```

Why:  
Streams logs in real time.

---

## Scenario 4: File Permission Issue

**Step 1**
```bash
ls -l /home/user/backup.sh
```

Why:  
Check current file permissions.

---

**Step 2**
```bash
chmod +x /home/user/backup.sh
```

Why:  
Adds execute permission to the script.

---

**Step 3**
```bash
ls -l /home/user/backup.sh
```

Why:  
Verify the execute permission was added.

---

**Step 4**
```bash
./backup.sh
```

Why:  
Run the script to confirm the issue is resolved.

---

# What I Learned

- Linux follows a structured filesystem hierarchy.
- Logs are critical for troubleshooting system issues.
- Using a step-by-step troubleshooting approach helps diagnose problems quickly.

---

# Summary

In this exercise, I explored the Linux filesystem hierarchy and practiced real-world troubleshooting scenarios. Understanding where files, logs, and binaries are located helps DevOps engineers debug systems efficiently and respond to production incidents faster.
