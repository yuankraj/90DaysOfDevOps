# Day 28 – Revision Day (Days 1–27)

# Objective

Today was a complete revision day for everything I learned from Day 1 to Day 27 in my #90DaysOfDevOps journey.

Instead of learning new topics, I focused on:
- Revising weak areas
- Re-running important hands-on tasks
- Checking my practical understanding
- Organizing my learning properly

This helped me identify strengths, gaps, and areas that need more practice.

---

# Task 1 – Self Assessment Checklist

# Linux

| Topic | Status |
|-------|---------|
| Navigate Linux file system | ✅ Can do confidently |
| Manage processes & services | ✅ Can do confidently |
| Work with systemd | ✅ Can do confidently |
| Edit files with vim/nano | ✅ Can do confidently |
| Troubleshoot CPU/memory/disk | ⚠️ Need more practice |
| Explain Linux hierarchy | ✅ Can do confidently |
| User & group management | ✅ Can do confidently |
| File permissions | ✅ Can do confidently |
| File ownership | ✅ Can do confidently |
| LVM management | ⚠️ Need revision |
| Networking checks | ✅ Can do confidently |
| DNS, IP, subnets & ports | ⚠️ Need revision |

---

# Shell Scripting

| Topic | Status |
|-------|---------|
| Variables & arguments | ⚠️ Haven't practiced fully |
| Conditions & loops | ⚠️ Haven't practiced fully |
| Functions | ⚠️ Haven't practiced fully |
| grep, awk, sed | ⚠️ Need revision |
| Error handling | ⚠️ Need revision |
| Crontab scheduling | ⚠️ Need revision |

---

# Git & GitHub

| Topic | Status |
|-------|---------|
| Git basics | ✅ Can do confidently |
| Branching | ✅ Can do confidently |
| Push/Pull GitHub | ✅ Can do confidently |
| Clone vs Fork | ✅ Can do confidently |
| Merge & Merge commits | ✅ Can do confidently |
| Rebase | ⚠️ Need more practice |
| Stash | ✅ Can do confidently |
| Cherry-pick | ⚠️ Need more practice |
| Squash merge | ⚠️ Need more practice |
| Reset vs Revert | ✅ Can explain confidently |
| Branching strategies | ✅ Can explain confidently |
| GitHub CLI | ✅ Can do confidently |

---

# Task 2 – Revisit Weak Areas

# 1. LVM Management

## Re-learned:
- Difference between PV, VG, and LV
- How logical volumes are resized
- Why LVM is useful in production systems

## Commands Practiced
```bash
pvs
vgs
lvs
lvextend
resize2fs
```

---

# 2. Git Rebase

## Re-learned:
- Rebase rewrites commit history
- Creates cleaner linear history
- Dangerous on shared branches

## Commands Practiced
```bash
git rebase main
git log --graph
```

---

# 3. DNS & Networking

## Re-learned:
- DNS resolution flow
- Difference between public and private IPs
- CIDR notation basics

## Commands Practiced
```bash
dig google.com
ss -tulpn
ip addr show
```

---

# Task 3 – Quick Fire Questions

# 1. What does chmod 755 script.sh do?

```bash
Owner → read, write, execute
Group → read, execute
Others → read, execute
```

Makes the script executable.

---

# 2. Difference between process and service?

| Process | Service |
|----------|----------|
| Running program | Background managed process |
| Temporary | Usually long-running |

---

# 3. How to find which process uses port 8080?

```bash
ss -tulpn | grep 8080
```

---

# 4. What does set -euo pipefail do?

| Option | Purpose |
|---------|----------|
| -e | Exit on error |
| -u | Error on undefined variable |
| pipefail | Fail pipeline if any command fails |

---

# 5. Difference between git reset --hard and git revert?

| reset --hard | revert |
|---------------|--------|
| Deletes commits & changes | Creates undo commit |
| Rewrites history | Preserves history |

---

# 6. Best branching strategy for 5 developers shipping weekly?

```bash
GitHub Flow
```

Reason:
- Simple
- Fast workflow
- Easy CI/CD integration

---

# 7. What does git stash do?

Temporarily saves unfinished changes without committing.

Used when:
- Need to switch branches quickly
- Temporary work in progress

---

# 8. Schedule script daily at 3 AM

```bash
0 3 * * * /path/to/script.sh
```

---

# 9. Difference between git fetch and git pull?

| git fetch | git pull |
|------------|-----------|
| Downloads changes only | Downloads + merges changes |

---

# 10. What is LVM?

LVM = Logical Volume Manager

Used because:
- Flexible storage management
- Easy resizing
- Better disk management than traditional partitions

---

# Task 4 – Organize Work

# Checklist Completed

| Task | Status |
|------|---------|
| All days committed & pushed | ✅ |
| git-commands.md updated | ✅ |
| Shell cheat sheet checked | ✅ |
| GitHub profile cleaned | ✅ |

---

# Task 5 – Teach It Back

# Explaining Git Branching to a Beginner

Git branching allows developers to work on different features separately without affecting the main project.

For example:
- One developer works on login
- Another works on payment system

Both use separate branches.

After testing, the branches are merged into the main project safely.

Branches help teams:
- Avoid breaking stable code
- Work independently
- Manage features more efficiently

This is one of the most important concepts in modern software development and DevOps workflows.

---

# Biggest Learnings So Far

- Linux fundamentals are extremely important for DevOps.
- Git & GitHub workflows are essential for collaboration.
- Networking basics help understand real infrastructure problems.
- Public learning improves consistency and confidence.
- Hands-on practice teaches more than theory alone.

---

# Areas I Still Need to Improve

- Advanced shell scripting
- CI/CD pipelines
- Docker & Kubernetes
- Advanced networking
- Cloud automation

---

# Final Summary

Today I revised:
- Linux fundamentals
- Networking basics
- Git & GitHub workflows
- LVM management
- DevOps troubleshooting concepts

This revision day helped me strengthen concepts, identify weak areas, and prepare for upcoming DevOps topics with better confidence.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
