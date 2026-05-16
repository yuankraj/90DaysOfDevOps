# Day 24 – Advanced Git: Merge, Rebase, Stash & Cherry Pick

# Objective

Today I learned advanced Git workflows used in real-world development and DevOps environments.

I practiced:
- Git merge
- Git rebase
- Squash merge
- Git stash
- Cherry-picking commits
- Merge conflicts

This challenge helped me understand how professional teams manage clean Git history and handle multiple workflows safely.

---

# Task 1 – Git Merge (Hands-On)

## Create feature-login Branch

### Input
```bash
git switch -c feature-login
```

---

## Add Commits

### Input
```bash
echo "Login feature" >> app.txt

git add .
git commit -m "Add login feature"

echo "Login validation" >> app.txt

git add .
git commit -m "Add login validation"
```

---

## Merge into main

### Input
```bash
git switch main
git merge feature-login
```

### Output
```bash
Fast-forward
```

### Observation
- Git performed a fast-forward merge because `main` had no new commits.

---

# Fast-Forward Merge

A fast-forward merge happens when Git simply moves the branch pointer forward because there are no diverging commits.

---

# Create feature-signup Branch

### Input
```bash
git switch -c feature-signup
```

---

## Add Commits

### Input
```bash
echo "Signup feature" >> signup.txt

git add .
git commit -m "Add signup feature"
```

---

## Add Commit to main Before Merge

### Input
```bash
git switch main

echo "Main branch update" >> main.txt

git add .
git commit -m "Update main branch"
```

---

## Merge feature-signup

### Input
```bash
git merge feature-signup
```

### Output
```bash
Merge made by the 'ort' strategy.
```

### Observation
- Git created a merge commit because histories diverged.

---

# Merge Commit

Git creates a merge commit when both branches contain separate commits.

---

# Merge Conflict

A merge conflict happens when:
- Same file
- Same line
- Modified differently in two branches

Git cannot automatically decide which version to keep.

---

# Task 2 – Git Rebase

## Create feature-dashboard Branch

### Input
```bash
git switch -c feature-dashboard
```

---

## Add Commits

### Input
```bash
echo "Dashboard UI" >> dashboard.txt
git add .
git commit -m "Add dashboard UI"

echo "Dashboard API" >> dashboard.txt
git add .
git commit -m "Add dashboard API"
```

---

## Add New Commit on main

### Input
```bash
git switch main

echo "Main update" >> app.txt

git add .
git commit -m "Update app configuration"
```

---

## Rebase feature-dashboard

### Input
```bash
git switch feature-dashboard
git rebase main
```

### Observation
- Git replayed feature-dashboard commits on top of updated main branch.

---

# Compare History

### Input
```bash
git log --oneline --graph --all
```

### Observation
- Rebase created cleaner linear history.
- Merge creates branch-style history.

---

# What Does Rebase Do?

Rebase:
- Moves commits
- Rewrites commit history
- Places commits on top of another branch

---

# Why Avoid Rebasing Shared Commits?

Because rebasing changes commit history and hashes.
This can break collaboration and create conflicts for teammates.

---

# Merge vs Rebase

| Merge | Rebase |
|-------|---------|
| Preserves history | Creates clean linear history |
| Safer for teams | Cleaner for personal branches |
| Creates merge commits | Rewrites commits |

---

# Task 3 – Squash Merge

## Create feature-profile Branch

### Input
```bash
git switch -c feature-profile
```

---

## Add Small Commits

### Input
```bash
git commit -m "Fix typo"
git commit -m "Update formatting"
git commit -m "Add profile validation"
```

---

## Squash Merge

### Input
```bash
git switch main
git merge --squash feature-profile
git commit -m "Add profile feature"
```

### Observation
- Multiple commits combined into one commit.

---

# Squash Merge Result

Only one commit added to main history.

---

# Regular Merge Comparison

Regular merge preserves all individual commits.

---

# When to Use Squash Merge?

Use squash merge when:
- Branch contains many small commits
- You want cleaner history

---

# Trade-Off of Squashing

- Cleaner history
- But loses detailed commit history

---

# Task 4 – Git Stash

## Make Changes Without Commit

### Input
```bash
echo "Temporary work" >> temp.txt
```

---

## Stash Changes

### Input
```bash
git stash push -m "WIP temp changes"
```

### Output
```bash
Saved working directory and index state
```

---

## Switch Branch

### Input
```bash
git switch feature-login
```

---

## Return and Restore Stash

### Input
```bash
git switch main
git stash pop
```

### Observation
- Changes restored successfully.

---

# List Stashes

### Input
```bash
git stash list
```

---

# Apply Specific Stash

### Input
```bash
git stash apply stash@{0}
```

---

# Difference Between stash pop & apply

| Command | Behavior |
|----------|-----------|
| git stash pop | Applies and removes stash |
| git stash apply | Applies but keeps stash |

---

# Real-World Use of Stash

Used when:
- Urgent bug fix appears
- Need to switch branches quickly
- Work is incomplete but must be saved temporarily

---

# Task 5 – Cherry Pick

## Create feature-hotfix Branch

### Input
```bash
git switch -c feature-hotfix
```

---

## Add Multiple Commits

### Input
```bash
git commit -m "Fix typo"
git commit -m "Fix login bug"
git commit -m "Update UI"
```

---

## Cherry Pick Second Commit

### Input
```bash
git switch main

git cherry-pick <commit-hash>
```

### Observation
- Only selected commit copied to main branch.

---

# What Does Cherry Pick Do?

Cherry-pick copies a specific commit from one branch to another.

---

# Real-World Use of Cherry Pick

Useful when:
- Only one bug fix needed
- Hotfix required in production
- Avoid merging full branch

---

# Risks of Cherry Pick

- Duplicate commits
- Merge conflicts
- Confusing history if overused

---

# Commands Used

```bash
git merge
git merge --squash
git rebase
git stash
git stash pop
git stash apply
git stash list
git cherry-pick
git log --graph
```

---

# What I Learned

- Merge combines branch histories.
- Rebase creates cleaner history.
- Squash merge keeps history compact.
- Stash temporarily saves unfinished work.
- Cherry-pick copies specific commits safely.

---

# Final Summary

Today I practiced:
- Git merge workflows
- Rebasing branches
- Squash merging
- Using Git stash
- Cherry-picking commits
- Understanding merge conflicts

This challenge gave me a much deeper understanding of advanced Git workflows used in real DevOps and software engineering environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
