# Day 28 – Revision Day: Everything from Day 1 to Day 27

## Task Summary
Today was a revision day where I reviewed all the topics covered from Day 1 to Day 27. The goal was to reinforce concepts in Linux, Shell Scripting, Networking, Git, and GitHub and identify areas that need improvement.

---

# Task 1 – Self-Assessment Checklist

### Linux

| Topic | Status |
|------|------|
| Navigate file system (ls, cd, cp, mv, rm) | Can do confidently |
| Manage processes (ps, top, kill, bg, fg) | Can do confidently |
| Manage services with systemctl | Can do confidently |
| Edit files using vim/nano | Can do confidently |
| Troubleshoot CPU/memory/disk (top, free, df, du) | Need to revisit |
| Linux filesystem hierarchy | Can do confidently |
| User and group management | Can do confidently |
| File permissions with chmod | Can do confidently |
| File ownership with chown/chgrp | Can do confidently |
| LVM volume management | Need to revisit |
| Networking tools (ping, curl, ss, dig) | Can do confidently |
| DNS, IP, subnet concepts | Need to revisit |

---

### Shell Scripting

| Topic | Status |
|------|------|
| Write scripts with variables and input | Can do confidently |
| If/else conditions | Can do confidently |
| Loops (for, while, until) | Need to revisit |
| Functions in scripts | Need to revisit |
| Text processing (grep, awk, sed) | Need to revisit |
| Error handling in scripts | Haven't done enough |
| Scheduling scripts with crontab | Can do confidently |

---

### Git & GitHub

| Topic | Status |
|------|------|
| Initialize repo and commit changes | Can do confidently |
| Create and switch branches | Can do confidently |
| Push and pull from GitHub | Can do confidently |
| Clone vs fork | Can do confidently |
| Merge branches | Can do confidently |
| Rebase branches | Need to revisit |
| git stash | Can do confidently |
| Cherry-pick commits | Can do confidently |
| Squash merge vs merge commit | Need to revisit |
| git reset vs git revert | Can do confidently |
| Branching strategies | Need to revisit |
| GitHub CLI usage | Can do confidently |

---

# Task 2 – Revisited Topics

I revisited the following topics where I needed improvement:

### 1. LVM Volume Management

Key commands reviewed:

```
pvcreate
vgcreate
lvcreate
lvextend
resize2fs
```

What I relearned:
- LVM allows flexible storage management
- Logical volumes can be resized without repartitioning
- It is commonly used in production servers

---

### 2. Shell Script Loops

Example loop:

```bash
for i in {1..5}
do
  echo "Iteration $i"
done
```

Key takeaway:
Loops help automate repetitive tasks.

---

### 3. DNS and Subnetting

Key points reviewed:
- DNS converts domain names to IP addresses
- CIDR notation determines network size
- Example:

```
192.168.1.0/24
```

This allows **254 usable hosts**.

---

# Task 3 – Quick-Fire Questions

### 1. What does `chmod 755 script.sh` do?

It gives:
- Owner: read, write, execute
- Group: read, execute
- Others: read, execute

---

### 2. Difference between process and service

| Process | Service |
|-------|--------|
| Running program | Background system process |
| Short-lived | Usually long-running |
| Example: vim | Example: nginx |

---

### 3. How to find which process uses port 8080?

```
ss -tulpn | grep 8080
```

or

```
lsof -i :8080
```

---

### 4. What does `set -euo pipefail` do?

- `-e` exit if command fails  
- `-u` error if variable is undefined  
- `-o pipefail` pipeline fails if any command fails  

Used to make scripts safer.

---

### 5. Difference between `git reset --hard` and `git revert`

| Command | Behavior |
|-------|---------|
| reset --hard | Deletes commits and changes |
| revert | Creates new commit that undoes changes |

---

### 6. Branching strategy for small team shipping weekly

**GitHub Flow** is best because it is simple and supports fast deployments.

---

### 7. What does `git stash` do?

Temporarily saves uncommitted changes so you can switch branches safely.

---

### 8. Schedule script every day at 3 AM

```
crontab -e
```

Add:

```
0 3 * * * /path/to/script.sh
```

---

### 9. Difference between `git fetch` and `git pull`

| Command | Action |
|-------|-------|
| fetch | Downloads changes only |
| pull | Downloads and merges changes |

---

### 10. What is LVM?

LVM (Logical Volume Manager) allows flexible disk management by creating logical volumes that can be resized dynamically.

---

# Task 4 – Organizing Work

Completed the following checks:

- Verified all days (1–27) are committed and pushed
- Updated `git-commands.md`
- Reviewed shell scripting cheat sheet
- Confirmed GitHub profile improvements from Day 27

---

# Task 5 – Teach It Back

### Explaining Git Branching

Git branching allows developers to create separate workspaces within a repository. Each branch can contain different changes without affecting the main code. Developers create feature branches for new functionality, test them, and then merge them back into the main branch when ready. This keeps the main code stable while development continues.

---

# Key Takeaways

1. Linux fundamentals and troubleshooting skills are the backbone of DevOps.
2. Shell scripting helps automate repetitive system tasks.
3. Git and GitHub workflows enable collaboration and version control for software projects.

---

# Summary

This revision day helped consolidate everything learned so far in the DevOps journey. Reviewing commands and concepts improved my confidence in Linux administration, scripting, networking, and Git workflows.
