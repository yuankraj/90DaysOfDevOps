# Day 25 – Git Reset vs Revert & Branching Strategies

# Objective

Today I learned how to safely undo mistakes in Git using:
- `git reset`
- `git revert`

I also explored real-world Git branching strategies used by engineering teams and open-source projects.

This challenge helped me understand:
- Safe vs destructive Git operations
- Managing commit history
- Professional branching workflows

---

# Task 1 – Git Reset (Hands-On)

# Create Commits A, B, C

## Input
```bash
echo "Commit A" >> file.txt
git add .
git commit -m "Commit A"

echo "Commit B" >> file.txt
git add .
git commit -m "Commit B"

echo "Commit C" >> file.txt
git add .
git commit -m "Commit C"
```

---

# Soft Reset

## Input
```bash
git reset --soft HEAD~1
```

### Observation
- Last commit removed from history
- Changes stayed staged

---

# Mixed Reset

## Re-commit First

### Input
```bash
git add .
git commit -m "Commit C again"
```

---

## Run Mixed Reset

### Input
```bash
git reset --mixed HEAD~1
```

### Observation
- Last commit removed
- Changes stayed in working directory
- Files became unstaged

---

# Hard Reset

## Re-commit Again

### Input
```bash
git add .
git commit -m "Commit C final"
```

---

## Run Hard Reset

### Input
```bash
git reset --hard HEAD~1
```

### Observation
- Commit removed completely
- Changes deleted permanently from working directory

---

# Difference Between Reset Types

| Reset Type | Commit Removed | Changes Kept | Staged |
|------------|----------------|--------------|---------|
| --soft | Yes | Yes | Yes |
| --mixed | Yes | Yes | No |
| --hard | Yes | No | No |

---

# Which Reset is Destructive?

```bash
git reset --hard
```

Reason:
- Permanently deletes changes from working directory.

---

# When to Use Each Reset

| Command | Use Case |
|----------|----------|
| --soft | Fix commit message or combine commits |
| --mixed | Unstage changes |
| --hard | Completely discard unwanted changes |

---

# Should We Use Reset on Pushed Commits?

Usually NO.

Reason:
- Reset rewrites history
- Can break collaboration
- Causes problems for teammates

---

# Task 2 – Git Revert (Hands-On)

# Create Commits X, Y, Z

## Input
```bash
git commit -m "Commit X"
git commit -m "Commit Y"
git commit -m "Commit Z"
```

---

# Revert Middle Commit

## Input
```bash
git revert <commit-Y-hash>
```

### Observation
- Git created a new commit that reversed changes from commit Y.

---

# Check History

## Input
```bash
git log --oneline
```

### Observation
- Original commit Y still exists in history.
- Revert adds a new undo commit.

---

# Reset vs Revert

| Feature | git reset | git revert |
|----------|------------|-------------|
| Removes history | Yes | No |
| Rewrites commits | Yes | No |
| Safe for teams | No | Yes |
| Creates new commit | No | Yes |

---

# Why Revert is Safer

Because:
- History remains unchanged
- Shared repositories stay consistent
- Other developers are not affected

---

# When to Use Revert vs Reset

## Use Reset
- Local branches
- Personal work
- Before pushing commits

## Use Revert
- Shared repositories
- Production branches
- Team collaboration

---

# Task 3 – Reset vs Revert Summary

| | git reset | git revert |
|---|---|---|
| What it does | Moves branch pointer backward | Creates undo commit |
| Removes commit from history? | Yes | No |
| Safe for pushed branches? | No | Yes |
| Best use case | Local cleanup | Shared repositories |

---

# Task 4 – Branching Strategies

# 1. GitFlow

## How It Works

Uses multiple branches:
- main
- develop
- feature
- release
- hotfix

---

## Simple Flow

```bash
main
 └── develop
      ├── feature-login
      ├── feature-api
      └── release-v1
```

---

## Used For

- Large enterprise projects
- Scheduled releases

---

## Pros

- Organized workflow
- Stable production branch
- Good release management

---

## Cons

- Complex
- Too many branches

---

# 2. GitHub Flow

## How It Works

Simple workflow:
- main branch
- short-lived feature branches

---

## Flow

```bash
main
 ├── feature-1
 ├── bugfix-1
```

---

## Used For

- Startups
- Fast deployments
- CI/CD workflows

---

## Pros

- Simple
- Fast development
- Easy collaboration

---

## Cons

- Less structured for large releases

---

# 3. Trunk-Based Development

## How It Works

Everyone commits frequently to main branch.

---

## Flow

```bash
main
 ├── short-feature
 ├── hotfix
```

---

## Used For

- High-speed teams
- Continuous deployment

---

## Pros

- Faster integration
- Less merge conflict

---

## Cons

- Requires strong testing pipelines

---

# Which Strategy Would I Use?

## Startup Shipping Fast

```bash
GitHub Flow
```

Reason:
- Simple
- Fast
- Works well with CI/CD

---

## Large Team with Scheduled Releases

```bash
GitFlow
```

Reason:
- Better release management
- Structured workflow

---

## Open Source Project Example

Example:
```bash
Kubernetes → GitHub Flow style workflows
```

---

# Task 5 – Git Commands Reference Update

# New Commands Added

```bash
git reset --soft
git reset --mixed
git reset --hard
git revert
git reflog
```

---

# git reflog

## Purpose
Shows all Git history activity, including resets.

## Example
```bash
git reflog
```

---

# Commands Used

```bash
git reset
git revert
git reflog
git log
git branch
git merge
```

---

# What I Learned

- Reset rewrites history while revert preserves it.
- Hard reset is dangerous because it deletes changes permanently.
- Revert is safer for shared repositories.
- Branching strategies vary depending on team size and workflow.
- GitHub Flow is simpler while GitFlow is more structured.

---

# Final Summary

Today I practiced:
- Undoing commits using reset and revert
- Understanding destructive vs safe Git operations
- Learning professional branching strategies
- Comparing GitFlow, GitHub Flow, and Trunk-Based Development

This challenge improved my understanding of safe Git workflows and real-world collaboration strategies used in DevOps and software engineering teams.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
