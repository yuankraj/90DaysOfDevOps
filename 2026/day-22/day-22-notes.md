# Day 22 – Introduction to Git: Your First Repository

## Task Summary
Today I started learning Git and created my first local repository.  
I practiced initializing a repository, staging files, committing changes, and viewing commit history.

---

# Task 1 – Install and Configure Git

### Check Git Version

```bash
git --version
```

Example Output

```
git version 2.43.0
```

---

### Configure Git Identity

```bash
git config --global user.name "Vishal Kumar"
git config --global user.email "yuankraj@gmail.com"
```

---

### Verify Configuration

```bash
git config --list
```

Example Output

```
user.name=Vishal Kumar
user.email=yuankraj@gmail.com
```

---

# Task 2 – Create Git Project

### Create Project Folder

```bash
mkdir devops-git-practice
cd devops-git-practice
```

---

### Initialize Git Repository

```bash
git init
```

Output

```
Initialized empty Git repository in devops-git-practice/.git/
```

---

### Check Repository Status

```bash
git status
```

Observation  
Git shows that the repository has **no commits yet** and no files tracked.

---

### Explore .git Directory

```bash
ls -la .git
```

Important folders inside `.git`:

| Folder/File | Purpose |
|--------------|--------|
| HEAD | Points to the current branch |
| objects | Stores Git objects (commits, trees, blobs) |
| refs | Stores references to commits |
| config | Repository configuration |

---

# Task 3 – Git Commands Reference File

Created file:

```
git-commands.md
```

Example content:

## Setup & Config

### git config
Used to configure Git username and email.

Example

```
git config --global user.name "Your Name"
```

---

### git init
Creates a new Git repository.

Example

```
git init
```

---

## Basic Workflow

### git add
Adds files to the staging area.

Example

```
git add file.txt
```

---

### git commit
Records staged changes to the repository history.

Example

```
git commit -m "Initial commit"
```

---

## Viewing Changes

### git status
Shows the current state of the working directory and staging area.

Example

```
git status
```

---

### git log
Displays commit history.

Example

```
git log
```

---

# Task 4 – Stage and Commit

### Stage File

```bash
git add git-commands.md
```

Check staged files:

```bash
git status
```

---

### Commit Changes

```bash
git commit -m "Add git commands reference file"
```

---

### View Commit History

```bash
git log
```

Example Output

```
commit 1a2b3c4d
Author: Vishal Kumar
Message: Add git commands reference file
```

---

# Task 5 – Build Commit History

Updated `git-commands.md` multiple times and created several commits.

Example commit history:

```bash
git log --oneline
```

Example Output

```
a31f9c2 Add more git commands
91bc8ad Update git command descriptions
54d8c6f Add git commands reference file
```

This shows a **clean commit history with multiple commits**.

---

# Task 6 – Git Workflow Concepts

## 1. Difference between git add and git commit

- `git add` moves changes to the **staging area**.
- `git commit` permanently records those staged changes in the repository history.

---

## 2. What is the staging area?

The staging area is an intermediate step where you select which changes will go into the next commit.  
It helps organize commits and include only relevant changes.

---

## 3. What does git log show?

`git log` shows:

- commit ID
- author name
- date
- commit message

It provides the full history of the repository.

---

## 4. What is the .git folder?

The `.git` directory contains **all repository data**, including commits, configuration, and branches.

If you delete `.git`, the project will **no longer be a Git repository**, and all version history will be lost.

---

## 5. Working Directory vs Staging Area vs Repository

| Area | Description |
|------|-------------|
| Working Directory | Where you edit files |
| Staging Area | Where changes are prepared for commit |
| Repository | Where committed history is stored |

---

# What I Learned

1. Git tracks file changes and maintains version history.
2. The staging area helps organize changes before committing.
3. A clean commit history makes collaboration and debugging easier.

---

# Commands Used

```
git --version
git config
git config --list
git init
git status
git add
git commit
git log
git log --oneline
ls -la .git
```

---

# Summary

Today I created my first Git repository, staged files, made commits, and explored how Git tracks changes.  
This is the foundation for version control used in DevOps, CI/CD pipelines, and collaborative development.
