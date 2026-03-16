# Day 11 – File Ownership Challenge (chown & chgrp)

## Objective
Learn how Linux file ownership works and practice changing file owners and groups using `chown` and `chgrp`.

---

# Task 1 – Understanding Ownership

Run the command:

```bash
ls -l
```

Example Output

```
-rw-r--r-- 1 user user 120 Mar 21 notes.txt
-rw-r--r-- 1 user user 40 Mar 21 devops-file.txt
```

Format explanation:

```
-rw-r--r-- 1 owner group size date filename
```

Example:

```
-rw-r--r-- 1 vishal vishal 40 Mar 21 devops-file.txt
```

Meaning:

| Column | Description |
|------|-------------|
| owner | The user who owns the file |
| group | The group that has access to the file |

### Difference between Owner and Group

- **Owner:** The user who created or owns the file.
- **Group:** A set of users who can share access to the file.

---

# Task 2 – Basic chown Operations

### Create file

```bash
touch devops-file.txt
```

Check owner:

```bash
ls -l devops-file.txt
```

Example Output

```
-rw-r--r-- 1 vishal vishal 0 Mar 21 devops-file.txt
```

---

### Change owner to tokyo

```bash
sudo chown tokyo devops-file.txt
```

Verify:

```bash
ls -l devops-file.txt
```

Example Output

```
-rw-r--r-- 1 tokyo vishal 0 Mar 21 devops-file.txt
```

---

### Change owner to berlin

```bash
sudo chown berlin devops-file.txt
```

Verify again:

```bash
ls -l devops-file.txt
```

Example Output

```
-rw-r--r-- 1 berlin vishal 0 Mar 21 devops-file.txt
```

---

# Task 3 – Basic chgrp Operations

Create file:

```bash
touch team-notes.txt
```

Check group:

```bash
ls -l team-notes.txt
```

Example Output

```
-rw-r--r-- 1 vishal vishal 0 Mar 21 team-notes.txt
```

---

Create group:

```bash
sudo groupadd heist-team
```

Change group:

```bash
sudo chgrp heist-team team-notes.txt
```

Verify:

```bash
ls -l team-notes.txt
```

Example Output

```
-rw-r--r-- 1 vishal heist-team 0 Mar 21 team-notes.txt
```

---

# Task 4 – Combined Owner & Group Change

Create file:

```bash
touch project-config.yaml
```

Change owner and group in one command:

```bash
sudo chown professor:heist-team project-config.yaml
```

Verify:

```bash
ls -l project-config.yaml
```

Example Output

```
-rw-r--r-- 1 professor heist-team 0 Mar 21 project-config.yaml
```

---

Create directory:

```bash
mkdir app-logs
```

Change owner and group:

```bash
sudo chown berlin:heist-team app-logs
```

Verify:

```bash
ls -ld app-logs
```

Example Output

```
drwxr-xr-x 2 berlin heist-team 4096 Mar 21 app-logs
```

---

# Task 5 – Recursive Ownership

Create directory structure:

```bash
mkdir -p heist-project/vault
mkdir -p heist-project/plans

touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf
```

Create group:

```bash
sudo groupadd planners
```

Change ownership recursively:

```bash
sudo chown -R professor:planners heist-project
```

Verify:

```bash
ls -lR heist-project
```

Example Output

```
heist-project/
vault/
gold.txt
plans/
strategy.conf
```

All files now owned by:

```
professor planners
```

---

# Task 6 – Practice Challenge

Create users (if not already created):

```bash
sudo useradd -m tokyo
sudo useradd -m berlin
sudo useradd -m nairobi
```

Create groups:

```bash
sudo groupadd vault-team
sudo groupadd tech-team
```

Create directory:

```bash
mkdir bank-heist
```

Create files:

```bash
touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt
```

Set ownership:

```bash
sudo chown tokyo:vault-team bank-heist/access-codes.txt
sudo chown berlin:tech-team bank-heist/blueprints.pdf
sudo chown nairobi:vault-team bank-heist/escape-plan.txt
```

Verify:

```bash
ls -l bank-heist
```

Example Output

```
-rw-r--r-- 1 tokyo vault-team access-codes.txt
-rw-r--r-- 1 berlin tech-team blueprints.pdf
-rw-r--r-- 1 nairobi vault-team escape-plan.txt
```

---

# Files & Directories Created

```
devops-file.txt
team-notes.txt
project-config.yaml
app-logs/
heist-project/
bank-heist/
```

---

# Ownership Changes

Example:

| File | Before | After |
|-----|------|------|
| devops-file.txt | vishal:vishal | berlin:vishal |
| team-notes.txt | vishal:vishal | vishal:heist-team |
| project-config.yaml | vishal:vishal | professor:heist-team |
| heist-project/* | vishal:vishal | professor:planners |

---

# Commands Used

```
ls
touch
mkdir
useradd
groupadd
chown
chgrp
ls -l
ls -lR
```

---

# Screenshots

Add screenshots for:

```
ownership-before.png
ownership-after.png
recursive-change.png
bank-heist-files.png
```

---

# What I Learned

- Linux files have both **owner and group ownership**.
- `chown` can change both owner and group at the same time.
- Recursive ownership changes are useful for managing application directories.

---

# Summary

In this challenge, I practiced managing file ownership and groups using `chown` and `chgrp`. Proper ownership ensures that users and teams can safely access files and directories, which is critical for DevOps tasks such as deployments, shared project directories, and log management.
