# Day 11 – File Ownership Challenge (chown & chgrp)

# Objective

Today I practiced Linux file ownership and group management using:
- `chown`
- `chgrp`
- Recursive ownership changes

I learned how Linux controls access using owners and groups.

---

# Task 1 – Understanding Ownership

## Check Ownership in Home Directory

### Input
```bash
ls -l
```

### Output
```bash
-rw-r--r-- 1 user user 0 May 13 devops.txt
-rw-r--r-- 1 user user 35 May 13 notes.txt
drwxr-xr-x 2 user user 4096 May 13 project
```

---

## Observation

Format:
```bash
-rw-r--r-- 1 owner group size date filename
```

Example:
- Owner → user who owns the file
- Group → users belonging to same group can access based on permissions

### What I Learned
- Owner has primary control over the file.
- Group permissions help teams collaborate securely.

---

# Task 2 – Basic chown Operations

## Create File

### Input
```bash
touch devops-file.txt
```

---

## Check Current Owner

### Input
```bash
ls -l devops-file.txt
```

### Output
```bash
-rw-r--r-- 1 user user 0 May 13 devops-file.txt
```

---

## Change Owner to tokyo

### Input
```bash
sudo chown tokyo devops-file.txt
```

---

## Verify Change

### Input
```bash
ls -l devops-file.txt
```

### Output
```bash
-rw-r--r-- 1 tokyo user 0 May 13 devops-file.txt
```

---

## Change Owner to berlin

### Input
```bash
sudo chown berlin devops-file.txt
```

---

## Verify Again

### Output
```bash
-rw-r--r-- 1 berlin user 0 May 13 devops-file.txt
```

### Observation
- File ownership changed successfully.

---

# Task 3 – Basic chgrp Operations

## Create File

### Input
```bash
touch team-notes.txt
```

---

## Create Group

### Input
```bash
sudo groupadd heist-team
```

---

## Change Group

### Input
```bash
sudo chgrp heist-team team-notes.txt
```

---

## Verify Group Change

### Input
```bash
ls -l team-notes.txt
```

### Output
```bash
-rw-r--r-- 1 user heist-team 0 May 13 team-notes.txt
```

### Observation
- Group ownership changed successfully.

---

# Task 4 – Combined Owner & Group Change

## Create File

### Input
```bash
touch project-config.yaml
```

---

## Change Owner and Group Together

### Input
```bash
sudo chown professor:heist-team project-config.yaml
```

---

## Verify

### Input
```bash
ls -l project-config.yaml
```

### Output
```bash
-rw-r--r-- 1 professor heist-team 0 May 13 project-config.yaml
```

---

## Create Directory

### Input
```bash
mkdir app-logs
```

---

## Change Ownership

### Input
```bash
sudo chown berlin:heist-team app-logs
```

---

## Verify

### Input
```bash
ls -ld app-logs
```

### Output
```bash
drwxr-xr-x 2 berlin heist-team 4096 May 13 app-logs
```

---

# Task 5 – Recursive Ownership

## Create Directory Structure

### Input
```bash
mkdir -p heist-project/vault
mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf
```

---

## Create Group

### Input
```bash
sudo groupadd planners
```

---

## Recursive Ownership Change

### Input
```bash
sudo chown -R professor:planners heist-project/
```

---

## Verify Recursive Changes

### Input
```bash
ls -lR heist-project/
```

### Output
```bash
heist-project/plans:
-rw-r--r-- 1 professor planners 0 strategy.conf

heist-project/vault:
-rw-r--r-- 1 professor planners 0 gold.txt
```

### Observation
- Ownership updated recursively for all files and directories.

---

# Task 6 – Practice Challenge

## Create Directory

### Input
```bash
mkdir bank-heist
```

---

## Create Files

### Input
```bash
touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt
```

---

## Create Groups

### Input
```bash
sudo groupadd vault-team
sudo groupadd tech-team
```

---

## Set Ownership

### Input
```bash
sudo chown tokyo:vault-team bank-heist/access-codes.txt

sudo chown berlin:tech-team bank-heist/blueprints.pdf

sudo chown nairobi:vault-team bank-heist/escape-plan.txt
```

---

## Verify Ownership

### Input
```bash
ls -l bank-heist/
```

### Output
```bash
-rw-r--r-- 1 tokyo vault-team 0 access-codes.txt
-rw-r--r-- 1 berlin tech-team 0 blueprints.pdf
-rw-r--r-- 1 nairobi vault-team 0 escape-plan.txt
```

### Observation
- Different ownership applied successfully.

---

# Files & Directories Created

## Files
- devops-file.txt
- team-notes.txt
- project-config.yaml
- access-codes.txt
- blueprints.pdf
- escape-plan.txt

## Directories
- app-logs/
- heist-project/
- bank-heist/

---

# Ownership Changes

| File/Directory | Before | After |
|----------------|--------|-------|
| devops-file.txt | user:user | berlin:user |
| team-notes.txt | user:user | user:heist-team |
| project-config.yaml | user:user | professor:heist-team |
| app-logs/ | user:user | berlin:heist-team |

---

# Commands Used

```bash
ls -l
touch
mkdir
chown
chgrp
groupadd
useradd
chmod
```

---

# What I Learned

- Linux ownership controls file access and security.
- `chown` changes file owner and group ownership.
- `chgrp` changes only group ownership.
- Recursive ownership is useful for project directories.
- Proper ownership management is important in DevOps and production systems.

---

# Final Summary

Today I practiced:
- File ownership management
- Group ownership management
- Recursive ownership changes
- Shared directory access control

This challenge improved my understanding of Linux ownership and how teams securely manage files in real DevOps environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
