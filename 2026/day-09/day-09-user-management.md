# Day 09 – Linux User & Group Management Challenge

# Objective

Today I practiced Linux user and group management by creating users, assigning groups, and configuring shared directories with permissions.

---

# Task 1 – Create Users

## Create Users with Home Directories

### Input
```bash
sudo useradd -m tokyo
sudo useradd -m berlin
sudo useradd -m professor
```

### Set Passwords
```bash
sudo passwd tokyo
sudo passwd berlin
sudo passwd professor
```

### Verify Users

#### Input
```bash
cat /etc/passwd | grep -E 'tokyo|berlin|professor'
```

#### Output
```bash
tokyo:x:1001:1001::/home/tokyo:/bin/sh
berlin:x:1002:1002::/home/berlin:/bin/sh
professor:x:1003:1003::/home/professor:/bin/sh
```

---

### Verify Home Directories

#### Input
```bash
ls /home
```

#### Output
```bash
berlin
professor
tokyo
```

### Observation
- Users created successfully with their home directories.

---

# Task 2 – Create Groups

## Create Groups

### Input
```bash
sudo groupadd developers
sudo groupadd admins
```

---

## Verify Groups

### Input
```bash
cat /etc/group | grep -E 'developers|admins'
```

### Output
```bash
developers:x:1004:
admins:x:1005:
```

### Observation
- Groups created successfully.

---

# Task 3 – Assign Users to Groups

## Add Users to Groups

### Input
```bash
sudo usermod -aG developers tokyo
sudo usermod -aG developers,admins berlin
sudo usermod -aG admins professor
```

---

## Verify Group Membership

### Input
```bash
groups tokyo
groups berlin
groups professor
```

### Output
```bash
tokyo : tokyo developers
berlin : berlin developers admins
professor : professor admins
```

### Observation
- Group assignments completed correctly.

---

# Task 4 – Shared Directory Setup

## Create Shared Directory

### Input
```bash
sudo mkdir -p /opt/dev-project
```

---

## Set Group Ownership

### Input
```bash
sudo chgrp developers /opt/dev-project
```

---

## Set Permissions

### Input
```bash
sudo chmod 775 /opt/dev-project
```

---

## Verify Permissions

### Input
```bash
ls -ld /opt/dev-project
```

### Output
```bash
drwxrwxr-x 2 root developers 4096 May 13 12:20 /opt/dev-project
```

---

## Test File Creation

### Input
```bash
sudo -u tokyo touch /opt/dev-project/tokyo.txt
sudo -u berlin touch /opt/dev-project/berlin.txt
```

---

## Verify Files

### Input
```bash
ls -l /opt/dev-project
```

### Output
```bash
-rw-r--r-- 1 tokyo developers 0 May 13 12:25 tokyo.txt
-rw-r--r-- 1 berlin developers 0 May 13 12:25 berlin.txt
```

### Observation
- Shared directory working properly for developers group.

---

# Task 5 – Team Workspace Challenge

## Create User Nairobi

### Input
```bash
sudo useradd -m nairobi
sudo passwd nairobi
```

---

## Create Project Team Group

### Input
```bash
sudo groupadd project-team
```

---

## Add Users to Group

### Input
```bash
sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo
```

---

## Create Team Workspace

### Input
```bash
sudo mkdir -p /opt/team-workspace
sudo chgrp project-team /opt/team-workspace
sudo chmod 775 /opt/team-workspace
```

---

## Verify Workspace Permissions

### Input
```bash
ls -ld /opt/team-workspace
```

### Output
```bash
drwxrwxr-x 2 root project-team 4096 May 13 12:40 /opt/team-workspace
```

---

## Test File Creation as Nairobi

### Input
```bash
sudo -u nairobi touch /opt/team-workspace/nairobi.txt
```

---

## Verify File

### Input
```bash
ls -l /opt/team-workspace
```

### Output
```bash
-rw-r--r-- 1 nairobi project-team 0 May 13 12:42 nairobi.txt
```

### Observation
- Team workspace permissions configured successfully.

---

# Users & Groups Created

## Users
- tokyo
- berlin
- professor
- nairobi

## Groups
- developers
- admins
- project-team

---

# Group Assignments

| User       | Groups                    |
|------------|---------------------------|
| tokyo      | developers, project-team |
| berlin     | developers, admins       |
| professor  | admins                   |
| nairobi    | project-team             |

---

# Directories Created

| Directory              | Group Owner   | Permissions |
|-----------------------|---------------|-------------|
| /opt/dev-project      | developers    | 775         |
| /opt/team-workspace   | project-team  | 775         |

---

# Commands Used

```bash
useradd
passwd
groupadd
usermod
groups
mkdir
chgrp
chmod
ls
touch
```

---

# What I Learned

- Linux users and groups help manage access securely.
- Group permissions are important for shared collaboration.
- chmod and chgrp are essential for permission management.
- Shared directories are commonly used in DevOps teams.
- Proper permission setup prevents unauthorized access.

---

# Final Summary

Today I completed a Linux user and group management challenge by:
- Creating users and groups
- Assigning users to multiple groups
- Configuring shared directories
- Testing permissions practically

This hands-on practice improved my understanding of Linux access management and real-world DevOps collaboration workflows.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
