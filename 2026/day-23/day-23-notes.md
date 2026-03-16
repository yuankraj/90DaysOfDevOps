# Day 23 – Git Branching & Working with GitHub

## Task Summary
Today I learned about Git branching and how to push a repository to GitHub. Branching allows developers to work on features independently without affecting the main codebase.

---

# Task 1 – Understanding Branches

## 1. What is a branch in Git?

A branch in Git is a separate line of development that allows you to work on new features, fixes, or experiments without affecting the main codebase. Each branch points to a specific commit.

---

## 2. Why do we use branches instead of committing everything to main?

Branches help isolate changes so that the main branch stays stable. Developers can work on new features in separate branches and merge them into `main` only after testing.

---

## 3. What is HEAD in Git?

`HEAD` is a pointer that represents the current branch and the latest commit you are working on.

Example:

```
HEAD -> main
```

This means you are currently working on the `main` branch.

---

## 4. What happens to your files when you switch branches?

When switching branches, Git updates the working directory to match the files stored in that branch. Files may change, appear, or disappear depending on what exists in that branch.

---

# Task 2 – Branching Commands (Hands-On)

### List all branches

```bash
git branch
```

---

### Create a new branch

```bash
git branch feature-1
```

---

### Switch to the branch

```bash
git checkout feature-1
```

or modern command:

```bash
git switch feature-1
```

---

### Create and switch branch in one command

```bash
git checkout -b feature-2
```

or modern command:

```bash
git switch -c feature-2
```

---

### Switch back to main

```bash
git switch main
```

---

### Delete a branch

```bash
git branch -d feature-2
```

---

### Commit on feature branch

Example workflow:

```bash
echo "Feature update" >> git-commands.md
git add git-commands.md
git commit -m "Add feature update"
```

When switching back to `main`, this commit does not appear unless merged.

---

# Task 3 – Push to GitHub

### Add GitHub remote

```bash
git remote add origin https://github.com/<username>/devops-git-practice.git
```

---

### Push main branch

```bash
git push -u origin main
```

---

### Push feature branch

```bash
git push -u origin feature-1
```

Both branches should now appear in the GitHub repository.

---

## What is the difference between origin and upstream?

| Term | Meaning |
|------|--------|
| origin | The default remote repository you cloned or pushed to |
| upstream | The original repository you forked from |

Example:

```
origin = your fork
upstream = original project
```

---

# Task 4 – Pull Changes from GitHub

### Pull latest changes

```bash
git pull origin main
```

---

## Difference between git fetch and git pull

| Command | Purpose |
|--------|--------|
| git fetch | Downloads changes from remote but does not merge them |
| git pull | Downloads and automatically merges the changes |

---

# Task 5 – Clone vs Fork

### Clone a repository

```bash
git clone https://github.com/user/repository.git
```

Cloning copies a repository from GitHub to your local machine.

---

### Fork a repository

A fork creates a copy of another user's repository in your own GitHub account.

You can then clone your fork:

```bash
git clone https://github.com/yourusername/repository.git
```

---

## Difference between Clone and Fork

| Feature | Clone | Fork |
|-------|------|------|
| Location | Local machine | GitHub account |
| Purpose | Work locally | Create your own copy on GitHub |
| Ownership | Same repo | Your copy of repo |

---

## When to Clone vs Fork

- **Clone:** When working on your own repository or when you have direct access.
- **Fork:** When contributing to someone else's project.

---

## Sync Fork with Original Repository

Add upstream remote:

```bash
git remote add upstream https://github.com/original/repository.git
```

Fetch updates:

```bash
git fetch upstream
```

Merge updates:

```bash
git merge upstream/main
```

---

# Commands Used

```
git branch
git switch
git checkout
git checkout -b
git switch -c
git branch -d
git remote add origin
git push
git pull
git fetch
git clone
git remote add upstream
git merge
```

---

# What I Learned

1. Git branches allow developers to work on features without affecting the main codebase.
2. `git push` sends local commits to GitHub repositories.
3. Forking allows you to contribute to open-source projects while maintaining your own copy.

---

# Summary

Today I learned how Git branching works and how to connect a local repository to GitHub. Branching enables safe development of new features, while GitHub allows collaboration and remote version control.
