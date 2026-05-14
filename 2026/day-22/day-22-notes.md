# Day 22 – Introduction to Git: Your First Repository

# Objective

Today I started my Git journey and learned the fundamentals of version control.

I practiced:
- Installing and configuring Git
- Creating my first Git repository
- Understanding the Git workflow
- Staging and committing changes
- Building commit history

Git is one of the most important tools in DevOps because it helps track changes, collaborate with teams, and manage project history.

---

# Task 1 – Install and Configure Git

## Verify Git Installation

### Input
```bash
git --version
```

### Output
```bash
git version 2.43.0
```

### Observation
- Git installed successfully.

---

## Configure Git Username

### Input
```bash
git config --global user.name "Vishal Singh"
```

---

## Configure Git Email

### Input
```bash
git config --global user.email "vishal@example.com"
```

---

## Verify Git Configuration

### Input
```bash
git config --list
```

### Output
```bash
user.name=Vishal Singh
user.email=vishal@example.com
```

### Observation
- Git identity configured successfully.

---

# Task 2 – Create Git Project

## Create Project Folder

### Input
```bash
mkdir devops-git-practice
cd devops-git-practice
```

---

## Initialize Git Repository

### Input
```bash
git init
```

### Output
```bash
Initialized empty Git repository
```

### Observation
- Git repository created successfully.

---

## Check Repository Status

### Input
```bash
git status
```

### Output
```bash
On branch master

No commits yet
nothing to commit
```

### Observation
- Repository initialized but no files tracked yet.

---

## Explore .git Directory

### Input
```bash
ls -la .git
```

### Output
```bash
HEAD
config
objects
refs
```

### Observation
- `.git/` stores all repository history and configuration.

---

# Task 3 – Create Git Commands Reference

## Create File

### Input
```bash
touch git-commands.md
```

---

# git-commands.md Content

## Setup & Config

### git --version
- Checks installed Git version

Example:
```bash
git --version
```

---

### git config --global user.name
- Sets Git username

Example:
```bash
git config --global user.name "Vishal Singh"
```

---

### git config --global user.email
- Sets Git email

Example:
```bash
git config --global user.email "vishal@example.com"
```

---

## Basic Workflow

### git init
- Initializes a new Git repository

Example:
```bash
git init
```

---

### git add
- Adds files to staging area

Example:
```bash
git add git-commands.md
```

---

### git commit
- Saves staged changes into repository history

Example:
```bash
git commit -m "Add git commands reference"
```

---

## Viewing Changes

### git status
- Shows repository status

Example:
```bash
git status
```

---

### git log
- Shows commit history

Example:
```bash
git log
```

---

# Task 4 – Stage and Commit

## Stage File

### Input
```bash
git add git-commands.md
```

---

## Check Staged Changes

### Input
```bash
git status
```

### Output
```bash
Changes to be committed:
new file: git-commands.md
```

---

## Commit Changes

### Input
```bash
git commit -m "Add initial git commands reference"
```

### Output
```bash
1 file changed
```

### Observation
- First commit created successfully.

---

## View Commit History

### Input
```bash
git log --oneline
```

### Output
```bash
d92ab1c Add initial git commands reference
```

---

# Task 5 – Build Commit History

## Edit File

### Added More Commands
```bash
git diff
git log --oneline
git branch
```

---

## Check Changes

### Input
```bash
git diff
```

### Observation
- Shows changes since last commit.

---

## Commit #2

### Input
```bash
git add .
git commit -m "Add git diff and log commands"
```

---

## Commit #3

### Input
```bash
git add .
git commit -m "Update git workflow notes"
```

---

## Commit #4

### Input
```bash
git add .
git commit -m "Improve git commands documentation"
```

---

## View Compact History

### Input
```bash
git log --oneline
```

### Output
```bash
f42a123 Improve git commands documentation
e31ab91 Update git workflow notes
b22c821 Add git diff and log commands
d92ab1c Add initial git commands reference
```

### Observation
- Multiple commits successfully created.

---

# Task 6 – Understanding Git Workflow

# day-22-notes.md

## 1. Difference between git add and git commit

- `git add` moves changes into the staging area.
- `git commit` permanently saves staged changes into Git history.

---

## 2. What does the staging area do?

- The staging area lets developers select specific changes before committing.
- It helps organize commits properly instead of saving everything directly.

---

## 3. What does git log show?

- Commit history
- Commit IDs
- Author details
- Commit messages
- Dates and timestamps

---

## 4. What is the .git folder?

- `.git/` stores all repository history, branches, commits, and configuration.
- If deleted, the project is no longer a Git repository.

---

## 5. Difference between working directory, staging area, and repository

| Component | Purpose |
|-----------|----------|
| Working Directory | Current files being edited |
| Staging Area | Files prepared for commit |
| Repository | Permanent Git history |

---

# Commands Used

```bash
git --version
git config
git init
git status
git add
git commit
git log
git diff
```

---

# What I Learned

- Git tracks project history efficiently.
- The staging area helps create clean commits.
- Commits should have meaningful messages.
- Git repositories store complete project history.
- Version control is essential for DevOps collaboration.

---

# Final Summary

Today I learned:
- Git fundamentals
- Repository initialization
- Staging and committing workflow
- Commit history management
- Basic version control concepts

This challenge gave me hands-on experience with Git and helped me understand how developers and DevOps engineers manage code changes professionally.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
