# Day 23 – Git Branching & Working with GitHub

# Objective

Today I learned one of the most powerful Git concepts: **Branching**.

I practiced:
- Creating and switching branches
- Understanding isolated development workflows
- Pushing repositories and branches to GitHub
- Pulling changes from remote repositories
- Understanding clone vs fork

This challenge helped me understand real-world collaborative Git workflows used by developers and DevOps engineers.

---

# Task 1 – Understanding Branches

## 1. What is a branch in Git?

A branch is an independent line of development in Git.
It allows developers to work on features or fixes separately without affecting the main codebase.

---

## 2. Why do we use branches instead of committing everything to main?

- Prevents breaking stable code
- Allows multiple developers to work independently
- Makes testing and feature development safer

---

## 3. What is HEAD in Git?

`HEAD` is a pointer that refers to the currently checked-out branch or commit.

Example:
```bash
HEAD -> main
```

---

## 4. What happens to your files when you switch branches?

- Git changes your working directory to match the selected branch
- Files specific to that branch appear/disappear automatically

---

# Task 2 – Branching Commands (Hands-On)

## List All Branches

### Input
```bash
git branch
```

### Output
```bash
* main
```

---

## Create Branch feature-1

### Input
```bash
git branch feature-1
```

---

## Switch to feature-1

### Input
```bash
git switch feature-1
```

### Output
```bash
Switched to branch 'feature-1'
```

---

## Create & Switch Branch in One Command

### Input
```bash
git switch -c feature-2
```

### Output
```bash
Switched to a new branch 'feature-2'
```

---

## Difference Between git switch & git checkout

| Command | Purpose |
|----------|----------|
| git checkout | Older command used for branches and files |
| git switch | Modern command only for branch switching |

---

## Make Commit on feature-1

### Input
```bash
echo "Feature 1 changes" >> feature.txt

git add .
git commit -m "Add feature 1 changes"
```

### Output
```bash
[feature-1] Add feature 1 changes
```

---

## Verify Commit Not in main

### Input
```bash
git switch main
git log --oneline
```

### Observation
- Commit from `feature-1` not visible in `main`.

---

## Delete Unused Branch

### Input
```bash
git branch -d feature-2
```

### Output
```bash
Deleted branch feature-2
```

---

# New Commands Added to git-commands.md

```bash
git branch
git switch
git switch -c
git branch -d
git push -u origin branch-name
```

---

# Task 3 – Push to GitHub

## Create GitHub Repository

Created new GitHub repository:
```bash
devops-git-practice
```

---

## Add Remote Repository

### Input
```bash
git remote add origin https://github.com/username/devops-git-practice.git
```

---

## Push Main Branch

### Input
```bash
git push -u origin main
```

### Output
```bash
Branch 'main' set up to track remote branch 'main'
```

---

## Push feature-1 Branch

### Input
```bash
git push -u origin feature-1
```

---

## Verify on GitHub

### Observation
- Both branches visible successfully on GitHub.

---

## Difference Between origin & upstream

| Term | Meaning |
|------|----------|
| origin | Your own remote repository |
| upstream | Original repository you forked from |

---

# Task 4 – Pull from GitHub

## Edit File on GitHub

Changed README.md directly from GitHub web editor.

---

## Pull Changes Locally

### Input
```bash
git pull origin main
```

### Output
```bash
Updating files successfully
```

---

## Difference Between git fetch & git pull

| Command | Purpose |
|----------|----------|
| git fetch | Downloads changes only |
| git pull | Downloads + merges changes |

---

# Task 5 – Clone vs Fork

## Clone Public Repository

### Input
```bash
git clone https://github.com/user/repo.git
```

### Observation
- Repository copied locally.

---

## Fork Repository

Forked repository using GitHub UI and cloned fork.

---

## Difference Between Clone & Fork

| Clone | Fork |
|-------|------|
| Local copy of repository | Personal GitHub copy of repository |
| Git concept | GitHub concept |
| Used for downloading code | Used for contributing independently |

---

## When to Use Clone vs Fork

### Clone
- When working on your own repositories
- When you already have write access

### Fork
- When contributing to someone else's project

---

## Keeping Fork Updated

### Commands
```bash
git remote add upstream https://github.com/original/repo.git

git fetch upstream

git merge upstream/main
```

---

# Commands Used

```bash
git branch
git switch
git switch -c
git branch -d
git remote add
git push
git pull
git fetch
git clone
```

---

# What I Learned

- Branches isolate development safely.
- GitHub stores remote repositories online.
- `git switch` is cleaner than `git checkout`.
- Forking is important for open-source contributions.
- Remote repositories help collaboration and backup.

---

# Final Summary

Today I practiced:
- Git branching workflow
- Switching and deleting branches
- Connecting repositories to GitHub
- Pushing and pulling code
- Understanding clone vs fork

This challenge improved my understanding of real-world Git collaboration and GitHub workflows used in DevOps teams.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
