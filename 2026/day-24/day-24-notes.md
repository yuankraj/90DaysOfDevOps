# Day 24 – Advanced Git: Merge, Rebase, Stash & Cherry Pick

## Task Summary
Today I practiced advanced Git workflows including **merge, rebase, squash merging, stashing work, and cherry-picking commits**. These techniques help manage complex development workflows and keep repository history organized.

---

# Task 1 – Git Merge

## Steps Performed

Create a feature branch:

```bash
git switch -c feature-login
```

Add commits:

```bash
echo "Login feature code" >> app.txt
git add app.txt
git commit -m "Add login feature"
```

Switch back to main:

```bash
git switch main
```

Merge branch:

```bash
git merge feature-login
```

---

## Observation

The merge was a **fast-forward merge** because the `main` branch had no new commits since the branch was created.

---

## Fast-Forward Merge

A fast-forward merge occurs when the target branch has not moved ahead.  
Git simply moves the branch pointer forward.

Example:

```
main ----A----B
             \
feature       C
```

After merge:

```
main ----A----B----C
```

---

## Merge Commit Example

If both branches have new commits:

```
main ----A----B----D
             \
feature       C
```

Git creates a **merge commit**:

```
main ----A----B----D
             \     /
              C---M
```

---

## Merge Conflict

A merge conflict occurs when two branches modify **the same line in the same file**.

Example conflict markers:

```
<<<<<<< HEAD
code from main
=======
code from feature branch
>>>>>>> feature
```

To resolve:
1. Edit the file
2. Remove conflict markers
3. Stage and commit

---

# Task 2 – Git Rebase

Create feature branch:

```bash
git switch -c feature-dashboard
```

Add commits:

```bash
echo "Dashboard feature" >> dashboard.txt
git add dashboard.txt
git commit -m "Add dashboard feature"
```

Switch to main and add commit:

```bash
git switch main
echo "Main branch update" >> app.txt
git commit -am "Update main branch"
```

Rebase feature branch:

```bash
git switch feature-dashboard
git rebase main
```

---

## Observation

Rebase **rewrites commit history** so that feature commits appear after the latest `main` commit.

Before rebase:

```
A---B---D main
     \
      C feature-dashboard
```

After rebase:

```
A---B---D---C feature-dashboard
```

---

## Merge vs Rebase History

Merge:

```
A---B---D
     \   \
      C---M
```

Rebase:

```
A---B---D---C
```

---

## Why Not Rebase Shared Commits?

Rebasing changes commit hashes.  
If commits are already pushed and others have pulled them, rebasing will create **history conflicts**.

---

## When to Use Rebase vs Merge

| Use Case | Command |
|--------|--------|
| Keep clean linear history | Rebase |
| Preserve full branch history | Merge |

---

# Task 3 – Squash Merge

Create branch:

```bash
git switch -c feature-profile
```

Add multiple commits:

```bash
git commit -m "Fix typo"
git commit -m "Improve formatting"
git commit -m "Update styles"
```

Merge with squash:

```bash
git switch main
git merge --squash feature-profile
git commit -m "Add profile feature"
```

---

## Observation

All commits were combined into **one single commit**.

---

## Squash Merge

Squash merge combines multiple commits into a single commit before merging.

Advantages:
- Cleaner history
- Easier to review

Trade-off:
- Original commit details are lost.

---

# Task 4 – Git Stash

Make changes but do not commit.

Try switching branches:

```
git switch main
```

Git warns about uncommitted changes.

---

## Save work temporarily

```bash
git stash
```

Switch branches:

```bash
git switch feature-login
```

Return to previous branch:

```bash
git switch main
```

Restore changes:

```bash
git stash pop
```

---

## Multiple Stashes

Create stash with message:

```bash
git stash push -m "WIP login feature"
```

List stashes:

```bash
git stash list
```

Apply specific stash:

```bash
git stash apply stash@{0}
```

---

## Difference: stash pop vs stash apply

| Command | Behavior |
|------|------|
| git stash pop | Apply and remove stash |
| git stash apply | Apply but keep stash |

---

## Real-World Use of Stash

Used when:
- You need to quickly switch branches
- Your work is incomplete
- You want to save temporary progress

---

# Task 5 – Cherry Pick

Create hotfix branch:

```bash
git switch -c feature-hotfix
```

Create commits:

```bash
git commit -m "Fix bug A"
git commit -m "Fix bug B"
git commit -m "Fix bug C"
```

Switch to main:

```bash
git switch main
```

Cherry-pick second commit:

```bash
git cherry-pick <commit-hash>
```

Verify:

```bash
git log --oneline
```

---

## What Cherry-Pick Does

Cherry-pick applies **a specific commit from one branch onto another branch**.

---

## When to Use Cherry-Pick

- Apply urgent hotfixes
- Bring a single commit from a feature branch
- Backport fixes to older releases

---

## Risks of Cherry-Pick

- Duplicate commits
- Potential conflicts
- Messy history if overused

---

# Commands Used

```
git merge
git merge --squash
git rebase
git stash
git stash pop
git stash apply
git stash list
git cherry-pick
git switch
git log --oneline --graph --all
```

---

# What I Learned

1. **Merge** combines branches while preserving history.
2. **Rebase** rewrites commit history for a cleaner timeline.
3. **Stash and cherry-pick** help manage temporary work and selective commits.

---

# Summary

Today I explored advanced Git workflows including merging, rebasing, stashing changes, and cherry-picking commits. These tools allow developers to manage complex feature development, maintain clean commit history, and apply fixes efficiently.
