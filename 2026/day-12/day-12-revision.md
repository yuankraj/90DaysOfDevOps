# Day 12 – Breather & Revision (Days 01–11)

## Goal
Review and reinforce the Linux fundamentals learned during Days 01–11.  
This revision focuses on refreshing key commands, verifying concepts, and identifying areas to improve.

---

# Mindset & Plan Review (Day 01)

Originally, my goal was to build strong **Linux and DevOps fundamentals** through the 90DaysOfDevOps challenge.

After reviewing the first 11 days:

- I am becoming more comfortable with Linux command-line usage.
- I can navigate the filesystem and troubleshoot basic system issues.
- My next focus is improving **Linux troubleshooting and Docker skills**.

Possible improvement:
- Practice more real-world troubleshooting scenarios.
- Improve speed using Linux commands.

---

# Processes & Services Review

### Command 1 – Check Running Processes

```bash
ps aux | head
```

Observation  
Displays currently running processes including PID, CPU usage, and command name.

---

### Command 2 – Check Service Status

```bash
systemctl status ssh
```

Observation  
Shows if the SSH service is running, stopped, or failed.

---

### Command 3 – Check Service Logs

```bash
journalctl -u ssh -n 10
```

Observation  
Displays the last 10 log entries for the SSH service.

---

# File Skills Practice

### Append text to file

```bash
echo "DevOps practice line" >> notes.txt
```

Observation  
Adds a new line to the file without overwriting existing content.

---

### Change file permission

```bash
chmod 755 script.sh
```

Observation  
Allows the owner to read/write/execute and others to read/execute.

---

### Change file ownership

```bash
sudo chown tokyo notes.txt
```

Observation  
Changes file owner to user **tokyo**.

---

# Cheat Sheet Refresh (Day 03)

Five commands I would use first during an incident:

| Command | Purpose |
|------|------|
| `top` | Monitor CPU and memory usage |
| `ps aux` | List running processes |
| `systemctl status` | Check service health |
| `journalctl -u service` | View service logs |
| `df -h` | Check disk space |

These commands help quickly identify system health problems.

---

# User & Group Sanity Check

Recreated a small user scenario.

Create user:

```bash
sudo useradd testuser
```

Check user information:

```bash
id testuser
```

Example Output

```
uid=1005(testuser) gid=1005(testuser) groups=1005(testuser)
```

Verify file ownership:

```bash
ls -l
```

Observation  
Confirms user and group assignments correctly.

---

# Mini Self-Check

### 1. Which 3 commands save you the most time right now, and why?

- **top** – quickly shows CPU and memory usage.
- **systemctl status** – immediately tells whether a service is running.
- **ls -l** – helps check file permissions and ownership quickly.

---

### 2. How do you check if a service is healthy?

Commands I run first:

```bash
systemctl status nginx
journalctl -u nginx -n 20
ps aux | grep nginx
```

These commands show service state, logs, and running processes.

---

### 3. How do you safely change ownership and permissions?

Example command:

```bash
sudo chown tokyo:developers project-file.txt
chmod 640 project-file.txt
```

This changes file ownership and limits access to only the owner and group.

---

### 4. What will you focus on improving in the next 3 days?

- Improve Linux troubleshooting skills.
- Practice Docker container deployment.
- Learn more about monitoring and logs.

---

# Key Takeaways

- Linux troubleshooting requires checking **processes, services, logs, and permissions**.
- Commands like `ps`, `top`, `systemctl`, and `journalctl` are essential for debugging.
- File permissions and ownership are critical for system security and team collaboration.

---

# Summary

This revision helped reinforce Linux fundamentals learned in the first 11 days of the DevOps challenge. Practicing commands again improved my understanding of processes, services, file permissions, and system troubleshooting.
