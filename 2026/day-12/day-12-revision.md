# Day 12 – Breather & Revision (Days 01–11)

# Goal

Today was a revision and reinforcement day for everything learned from Days 01–11 of my DevOps journey.

Instead of learning new concepts, I focused on:
- Revising Linux fundamentals
- Re-running important commands
- Strengthening weak areas
- Building better command memory

---

# Revision Notes

## 1. Mindset & Learning Plan Review

### Reflection
- My goal of becoming strong in Linux and DevOps fundamentals is still the same.
- I realized consistency matters more than speed.
- I want to improve:
  - Troubleshooting confidence
  - Command memory
  - Real cloud deployment skills

### Updated Focus
Next few days I will focus more on:
- Linux troubleshooting
- Docker basics
- Networking concepts
- Faster terminal usage

---

# 2. Processes & Services Revision

## Command 1 – Check Running Processes

### Input
```bash
ps aux | head
```

### Observation
- Shows active processes running in the system.
- Useful during troubleshooting and monitoring.

---

## Command 2 – Check Service Status

### Input
```bash
systemctl status nginx
```

### Output
```bash
Active: active (running)
```

### Observation
- Confirms whether service is healthy or not.

---

## Command 3 – Check Logs

### Input
```bash
journalctl -u nginx -n 10
```

### Observation
- Displays recent logs of nginx service.
- Very useful for debugging issues quickly.

---

# 3. File Skills Revision

## Append Text to File

### Input
```bash
echo "DevOps Revision Day" >> notes.txt
```

### Observation
- `>>` appends content without overwriting.

---

## Change Permissions

### Input
```bash
chmod 755 script.sh
```

### Observation
- Makes script executable for everyone.

---

## Check File Ownership

### Input
```bash
ls -l
```

### Observation
- Displays permissions, owner, and group information.

---

# 4. Cheat Sheet Refresh

## Top 5 Commands I Would Use During an Incident

| Command | Purpose |
|----------|----------|
| top | Monitor CPU & memory usage |
| ps aux | View running processes |
| systemctl status | Check service health |
| journalctl | View service logs |
| ls -l | Check permissions & ownership |

---

# 5. User & Group Sanity Check

## Create Test User

### Input
```bash
sudo useradd -m revision-user
```

---

## Verify User

### Input
```bash
id revision-user
```

### Output
```bash
uid=1005(revision-user) gid=1005(revision-user)
```

---

## Create Test File

### Input
```bash
touch revision.txt
sudo chown revision-user revision.txt
```

---

## Verify Ownership

### Input
```bash
ls -l revision.txt
```

### Output
```bash
-rw-r--r-- 1 revision-user user 0 May 13 revision.txt
```

### Observation
- Ownership changed successfully.

---

# Mini Self-Check

## 1. Which 3 commands save you the most time right now, and why?

### top
- Quickly shows CPU and memory usage.

### systemctl status
- Helps instantly verify if a service is running.

### journalctl
- Useful for checking service logs during troubleshooting.

---

## 2. How do you check if a service is healthy?

### Commands I would run:
```bash
systemctl status nginx
journalctl -u nginx -n 20
ps aux | grep nginx
```

### Why
- Checks service state, logs, and running processes.

---

## 3. How do you safely change ownership and permissions?

### Example
```bash
sudo chown tokyo:developers project.txt
chmod 640 project.txt
```

### Why
- Ownership controls access.
- Permissions control read/write/execute behavior.

---

## 4. What will you focus on improving in the next 3 days?

- Faster Linux troubleshooting
- Docker container basics
- Better understanding of networking
- More hands-on practice

---

# Key Takeaways

- Repetition improves Linux command memory.
- Logs are extremely important during troubleshooting.
- Permissions and ownership are critical for security.
- Service monitoring commands are essential for DevOps work.
- Hands-on practice helps more than theory alone.

---

# Final Summary

Today I revised:
- Linux commands
- Services & logs
- File permissions
- Ownership management
- Process monitoring

This revision day helped reinforce my Linux fundamentals and increased my confidence with day-to-day DevOps operations.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
