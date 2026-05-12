# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

# Part 1 – Linux File System Hierarchy

---

# 1. Root Directory `/`

## Purpose
- `/` is the root of the Linux filesystem.
- Everything in Linux starts from this directory.

## Input
```bash
ls -l /
```

## Output
```bash
drwxr-xr-x home
drwxr-xr-x etc
drwxr-xr-x var
drwxr-xr-x tmp
```

## Observation
- Contains all major Linux directories.

## I would use this when:
- Navigating the Linux filesystem from the top level.

---

# 2. Home Directory `/home`

## Purpose
- Stores personal files and folders of normal users.

## Input
```bash
ls -l /home
```

## Output
```bash
drwxr-xr-x user
```

## Observation
- Contains user home directories.

## I would use this when:
- Managing user files and project directories.

---

# 3. Root User Directory `/root`

## Purpose
- Home directory of the root (administrator) user.

## Input
```bash
ls -l /root
```

## Output
```bash
-rw------- .bashrc
-rw------- .profile
```

## Observation
- Contains root user configuration files.

## I would use this when:
- Performing administrative tasks as root user.

---

# 4. Configuration Directory `/etc`

## Purpose
- Stores system-wide configuration files.

## Input
```bash
ls -l /etc
```

## Output
```bash
-rw-r--r-- hostname
-rw-r--r-- hosts
drwxr-xr-x ssh
```

## Observation
- Contains important configuration files.

## I would use this when:
- Editing service or system configurations.

---

# 5. Log Directory `/var/log`

## Purpose
- Stores system and application logs.

## Input
```bash
ls -l /var/log
```

## Output
```bash
-rw-r----- syslog
-rw-r----- auth.log
```

## Observation
- Contains critical troubleshooting logs.

## I would use this when:
- Diagnosing server or application issues.

---

# 6. Temporary Directory `/tmp`

## Purpose
- Stores temporary files created by users or applications.

## Input
```bash
ls -l /tmp
```

## Output
```bash
drwx------ systemd-private
-rw-r--r-- temp.txt
```

## Observation
- Temporary data stored here may be deleted automatically.

## I would use this when:
- Creating temporary test files or scripts.

---

# 7. Binary Directory `/bin`

## Purpose
- Contains essential Linux commands.

## Input
```bash
ls -l /bin | head
```

## Output
```bash
-rwxr-xr-x bash
-rwxr-xr-x cat
-rwxr-xr-x ls
```

## Observation
- Contains commonly used commands.

## I would use this when:
- Running essential Linux utilities.

---

# 8. User Binary Directory `/usr/bin`

## Purpose
- Stores user-level command binaries and applications.

## Input
```bash
ls -l /usr/bin | head
```

## Output
```bash
-rwxr-xr-x python3
-rwxr-xr-x vim
-rwxr-xr-x gcc
```

## Observation
- Contains installed software commands.

## I would use this when:
- Running developer or system tools.

---

# 9. Optional Applications Directory `/opt`

## Purpose
- Stores third-party or optional applications.

## Input
```bash
ls -l /opt
```

## Output
```bash
drwxr-xr-x google
```

## Observation
- Used for manually installed software.

## I would use this when:
- Installing external applications.

---

# Hands-on Tasks

## Find Largest Log Files

### Input
```bash
du -sh /var/log/* 2>/dev/null | sort -h | tail -5
```

### Output
```bash
15M /var/log/syslog
25M /var/log/journal
```

### Observation
- Journal logs are consuming more storage.

---

## View Hostname Config

### Input
```bash
cat /etc/hostname
```

### Output
```bash
ubuntu
```

### Observation
- Hostname identifies the Linux machine on network.

---

## Check Home Directory

### Input
```bash
ls -la ~
```

### Output
```bash
.bashrc
Documents
Downloads
```

### Observation
- User home contains personal configuration and files.

---

# Part 2 – Scenario-Based Practice

---

# Scenario 1 – Service Not Starting

## Problem
A service called `myapp` failed after reboot.

---

### Step 1
#### Command
```bash
systemctl status myapp
```

#### Why
- Checks whether service is active, failed, or stopped.

---

### Step 2
#### Command
```bash
journalctl -u myapp -n 50
```

#### Why
- Reviews recent logs to identify startup errors.

---

### Step 3
#### Command
```bash
systemctl is-enabled myapp
```

#### Why
- Verifies whether service starts automatically after reboot.

---

### Step 4
#### Command
```bash
systemctl restart myapp
```

#### Why
- Attempts to restart service after checking logs.

---

## What I Learned
- Always check service status and logs before restarting.

---

# Scenario 2 – High CPU Usage

## Problem
Server performance is slow.

---

### Step 1
#### Command
```bash
top
```

#### Why
- Shows live CPU and memory usage.

---

### Step 2
#### Command
```bash
ps aux --sort=-%cpu | head -10
```

#### Why
- Displays top CPU-consuming processes.

---

### Step 3
#### Command
```bash
ps -p 1250 -o pid,%cpu,%mem,cmd
```

#### Why
- Checks detailed usage of suspicious process.

---

## What I Learned
- CPU troubleshooting starts with identifying top-consuming processes.

---

# Scenario 3 – Finding Service Logs

## Problem
Developer asks for Docker service logs.

---

### Step 1
#### Command
```bash
systemctl status docker
```

#### Why
- Confirms whether Docker service is running.

---

### Step 2
#### Command
```bash
journalctl -u docker -n 50
```

#### Why
- Displays recent Docker service logs.

---

### Step 3
#### Command
```bash
journalctl -u docker -f
```

#### Why
- Follows logs live for real-time troubleshooting.

---

## What I Learned
- systemd services store logs inside journald.

---

# Scenario 4 – File Permission Issue

## Problem
Script shows “Permission denied”.

---

### Step 1
#### Command
```bash
ls -l /home/user/backup.sh
```

#### Why
- Checks file permissions.

---

### Step 2
#### Command
```bash
chmod +x /home/user/backup.sh
```

#### Why
- Adds execute permission.

---

### Step 3
#### Command
```bash
ls -l /home/user/backup.sh
```

#### Why
- Verifies execute permission was added.

---

### Step 4
#### Command
```bash
./backup.sh
```

#### Why
- Executes the script.

---

## What I Learned
- Scripts need execute permission (`x`) to run.

---

# Final Summary

Today I learned:
- Linux filesystem hierarchy
- Where logs, configs, binaries, and user files are stored
- Real-world troubleshooting workflows
- Service debugging basics
- File permission troubleshooting

Important Directories Learned:
- /
- /home
- /root
- /etc
- /var/log
- /tmp
- /bin
- /usr/bin
- /opt

This practice improved my Linux troubleshooting confidence and prepared me for real DevOps scenarios.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
