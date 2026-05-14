# Git Commands Reference

> This file will be updated continuously throughout my Git & DevOps journey.

---

# 1. Setup & Configuration

## Check Git Version
```bash
git --version
```
### Purpose
Checks installed Git version.

### Example
```bash
git --version
```

---

## Configure Username
```bash
git config --global user.name "Your Name"
```
### Purpose
Sets global Git username.

### Example
```bash
git config --global user.name "Vishal Singh"
```

---

## Configure Email
```bash
git config --global user.email "yourmail@example.com"
```
### Purpose
Sets global Git email.

### Example
```bash
git config --global user.email "vishal@example.com"
```

---

## View Git Configuration
```bash
git config --list
```
### Purpose
Displays all Git configuration settings.

### Example
```bash
git config --list
```

---

# 2. Repository Initialization

## Initialize Repository
```bash
git init
```
### Purpose
Creates a new Git repository.

### Example
```bash
git init
```

---

## Clone Repository
```bash
git clone <repository-url>
```
### Purpose
Downloads a remote repository to local machine.

### Example
```bash
git clone https://github.com/user/repo.git
```

---

# 3. Basic Workflow

## Check Repository Status
```bash
git status
```
### Purpose
Shows current repository status.

### Example
```bash
git status
```

---

## Add Single File
```bash
git add filename
```
### Purpose
Stages a specific file.

### Example
```bash
git add app.py
```

---

## Add All Files
```bash
git add .
```
### Purpose
Stages all modified files.

### Example
```bash
git add .
```

---

## Commit Changes
```bash
git commit -m "message"
```
### Purpose
Saves staged changes permanently.

### Example
```bash
git commit -m "Add login feature"
```

---

# 4. Viewing Changes & History

## View Commit History
```bash
git log
```
### Purpose
Shows detailed commit history.

### Example
```bash
git log
```

---

## Compact Commit History
```bash
git log --oneline
```
### Purpose
Displays commits in compact format.

### Example
```bash
git log --oneline
```

---

## Show File Changes
```bash
git diff
```
### Purpose
Displays unstaged file changes.

### Example
```bash
git diff
```

---

## Show Staged Changes
```bash
git diff --staged
```
### Purpose
Displays staged changes before commit.

### Example
```bash
git diff --staged
```

---

# 5. Branching

## View Branches
```bash
git branch
```
### Purpose
Lists all local branches.

### Example
```bash
git branch
```

---

## Create New Branch
```bash
git branch branch-name
```
### Purpose
Creates a new branch.

### Example
```bash
git branch feature-login
```

---

## Switch Branch
```bash
git checkout branch-name
```
### Purpose
Switches to another branch.

### Example
```bash
git checkout feature-login
```

---

## Create & Switch Branch
```bash
git checkout -b branch-name
```
### Purpose
Creates and switches branch together.

### Example
```bash
git checkout -b dev
```

---

# 6. Remote Repositories

## Add Remote Repository
```bash
git remote add origin <url>
```
### Purpose
Connects local repo with remote repo.

### Example
```bash
git remote add origin https://github.com/user/repo.git
```

---

## View Remote URLs
```bash
git remote -v
```
### Purpose
Displays remote repository URLs.

### Example
```bash
git remote -v
```

---

## Push Changes
```bash
git push origin branch-name
```
### Purpose
Uploads commits to remote repository.

### Example
```bash
git push origin main
```

---

## Pull Changes
```bash
git pull origin branch-name
```
### Purpose
Fetches and merges remote changes.

### Example
```bash
git pull origin main
```

---

# 7. Undo & Restore

## Unstage File
```bash
git restore --staged filename
```
### Purpose
Removes file from staging area.

### Example
```bash
git restore --staged app.py
```

---

## Restore File Changes
```bash
git restore filename
```
### Purpose
Discards local changes.

### Example
```bash
git restore app.py
```

---

## Reset Last Commit
```bash
git reset --soft HEAD~1
```
### Purpose
Removes last commit but keeps changes.

### Example
```bash
git reset --soft HEAD~1
```

---

# 8. Useful Inspection Commands

## Show Commit Details
```bash
git show
```
### Purpose
Displays commit details.

### Example
```bash
git show
```

---

## View Tracked Files
```bash
git ls-files
```
### Purpose
Lists tracked files.

### Example
```bash
git ls-files
```

---

## Check Current Branch
```bash
git branch --show-current
```
### Purpose
Displays current branch name.

### Example
```bash
git branch --show-current
```

---

# 9. Helpful Git Concepts

| Concept | Meaning |
|----------|----------|
| Working Directory | Current files being edited |
| Staging Area | Files prepared for commit |
| Repository | Git history database |
| Commit | Snapshot of project changes |
| Branch | Independent line of development |
| Merge | Combining branches together |

---

# Commands I Use Most Often

```bash
git status
git add .
git commit -m "message"
git push
git pull
git log --oneline
git diff
```

---

# Notes

- Always check `git status` before committing.
- Write meaningful commit messages.
- Commit small logical changes.
- Pull latest changes before pushing.
- Keep commit history clean and understandable.

---

# Future Updates

This file will continue growing as I learn:
- Git branching strategies
- Merge conflicts
- Rebasing
- GitHub workflows
- CI/CD integrations
- Advanced Git troubleshooting

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
