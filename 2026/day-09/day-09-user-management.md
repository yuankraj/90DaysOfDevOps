# Day 09 – Linux User & Group Management Challenge

## Objective
Practice Linux user and group management by creating users, assigning them to groups, and configuring shared directories with proper permissions.

---

# Users & Groups Created

### Users
- tokyo
- berlin
- professor
- nairobi

### Groups
- developers
- admins
- project-team

---

# Task 1 – Create Users

Create users with home directories:

```bash
sudo useradd -m tokyo
sudo useradd -m berlin
sudo useradd -m professor
```

Set passwords:

```bash
sudo passwd tokyo
sudo passwd berlin
sudo passwd professor
```

### Verify Users

Check `/etc/passwd`:

```bash
cat /etc/passwd | grep tokyo
cat /etc/passwd | grep berlin
cat /etc/passwd | grep professor
```

Check home directories:

```bash
ls /home
```

Example Output

```
tokyo
berlin
professor
```

---

# Task 2 – Create Groups

Create groups:

```bash
sudo groupadd developers
sudo groupadd admins
```

### Verify Groups

```bash
cat /etc/group | grep developers
cat /etc/group | grep admins
```

Example Output

```
developers:x:1001:
admins:x:1002:
```

---

# Task 3 – Assign Users to Groups

Assign users:

```bash
sudo usermod -aG developers tokyo
sudo usermod -aG developers berlin
sudo usermod -aG admins berlin
sudo usermod -aG admins professor
```

### Verify Group Membership

```bash
groups tokyo
groups berlin
groups professor
```

Example Output

```
tokyo : tokyo developers
berlin : berlin developers admins
professor : professor admins
```

---

# Task 4 – Shared Directory Setup

Create project directory:

```bash
sudo mkdir /opt/dev-project
```

Set group ownership:

```bash
sudo chgrp developers /opt/dev-project
```

Set permissions:

```bash
sudo chmod 775 /opt/dev-project
```

### Verify Permissions

```bash
ls -ld /opt/dev-project
```

Example Output

```
drwxrwxr-x 2 root developers 4096 Mar 20 /opt/dev-project
```

### Test File Creation

Create files as different users:

```bash
sudo -u tokyo touch /opt/dev-project/tokyo-file.txt
sudo -u berlin touch /opt/dev-project/berlin-file.txt
```

Verify:

```bash
ls /opt/dev-project
```

Example Output

```
tokyo-file.txt
berlin-file.txt
```

---

# Task 5 – Team Workspace

Create user:

```bash
sudo useradd -m nairobi
sudo passwd nairobi
```

Create group:

```bash
sudo groupadd project-team
```

Add users to group:

```bash
sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo
```

Create shared directory:

```bash
sudo mkdir /opt/team-workspace
```

Set group ownership:

```bash
sudo chgrp project-team /opt/team-workspace
```

Set permissions:

```bash
sudo chmod 775 /opt/team-workspace
```

### Verify Directory

```bash
ls -ld /opt/team-workspace
```

Example Output

```
drwxrwxr-x 2 root project-team 4096 Mar 20 /opt/team-workspace
```

### Test File Creation

```bash
sudo -u nairobi touch /opt/team-workspace/nairobi-file.txt
```

Verify:

```bash
ls /opt/team-workspace
```

Example Output

```
nairobi-file.txt
```

---

# Group Assignments

| User | Groups |
|-----|------|
| tokyo | developers, project-team |
| berlin | developers, admins |
| professor | admins |
| nairobi | project-team |

---

# Directories Created

| Directory | Group Owner | Permissions |
|------|------|------|
| /opt/dev-project | developers | 775 |
| /opt/team-workspace | project-team | 775 |

---

# Commands Used

```
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

- How to create and manage Linux users and groups.
- How group permissions allow multiple users to collaborate in shared directories.
- How directory ownership and permissions control access to files and projects.

---

# Screenshots

Add screenshots for:

1. User creation command output  
`user-creation.png`

2. Group verification  
`group-verification.png`

3. Directory permissions  
`directory-permissions.png`

4. File creation test  
`file-test.png`

---

# Summary

In this challenge, I practiced Linux user and group management by creating multiple users, assigning them to groups, configuring shared directories, and testing access permissions. This simulates real-world DevOps scenarios where teams collaborate on shared environments with controlled access.
