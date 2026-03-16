# Day 25 – Git Reset vs Revert & Branching Strategies

## Task Summary
Today I learned how to undo changes in Git safely using **reset and revert**, and explored common **branching strategies used by development teams**.

---

# Task 1 – Git Reset (Hands-On)

Created three commits:

```
Commit A
Commit B
Commit C
```

---

## Reset with --soft

Command:

```bash
git reset --soft HEAD~1
```

Observation:

- Moves HEAD back one commit
- Changes remain **staged**
- Files are unchanged in the working directory

---

## Reset with --mixed (default)

Command:

```bash
git reset --mixed HEAD~1
```

Observation:

- Moves HEAD back one commit
- Changes remain in the working directory
- Files are **unstaged**

---

## Reset with --hard

Command:

```bash
git reset --hard HEAD~1
```

Observation:

- Moves HEAD back one commit
- Changes are **deleted**
- Working directory is reset completely

---

## Difference Between Reset Modes

| Reset Type | What Happens |
|-------------|-------------|
| --soft | Moves HEAD only, keeps staged changes |
| --mixed | Moves HEAD and unstages changes |
| --hard | Moves HEAD and deletes changes |

---

## Which Reset is Destructive?

`git reset --hard` is destructive because it permanently deletes uncommitted changes from the working directory.

---

## When to Use Each Reset Type

| Command | When to Use |
|--------|-------------|
| --soft | Fix commit message or combine commits |
| --mixed | Unstage changes but keep file edits |
| --hard | Completely discard local changes |

---

## Should You Use Reset on Pushed Commits?

No.  
Using reset on pushed commits can rewrite shared history and cause problems for other collaborators.

---

# Task 2 – Git Revert (Hands-On)

Created commits:

```
Commit X
Commit Y
Commit Z
```

Reverted commit Y:

```bash
git revert <commit-hash>
```

Observation:

- Git created a **new commit that reverses the changes**
- Commit Y still exists in history

Example history:

```
X → Y → Z → Revert-Y
```

---

## Difference Between Revert and Reset

| Feature | git reset | git revert |
|--------|-----------|------------|
| History rewritten | Yes | No |
| Removes commits | Yes | No |
| Safe for shared repos | No | Yes |

---

## Why Revert is Safer

Revert preserves commit history and creates a new commit that undoes changes instead of rewriting history.

---

## When to Use Revert vs Reset

| Use Case | Command |
|---------|---------|
| Undo local commits before push | reset |
| Undo commits already pushed | revert |

---

# Task 3 – Reset vs Revert Comparison

| Feature | git reset | git revert |
|--------|-----------|------------|
| What it does | Moves branch pointer backward | Creates new commit that reverses changes |
| Removes commit from history | Yes | No |
| Safe for shared branches | No | Yes |
| When to use | Fix local commits | Undo commits in shared repo |

---

# Task 4 – Branching Strategies

## 1. GitFlow

### How It Works

GitFlow uses multiple long-lived branches:

```
main → production code
develop → integration branch
feature/* → feature development
release/* → release preparation
hotfix/* → urgent production fixes
```

Example Flow

```
feature → develop → release → main
                   ↑
                 hotfix
```

### When Used

- Large teams
- Scheduled releases

### Pros

- Structured workflow
- Good release management

### Cons

- Complex
- Too heavy for small teams

---

## 2. GitHub Flow

### How It Works

Simple workflow using **main branch and feature branches**.

```
main → always deployable
feature branch → pull request → merge
```

Example

```
main → feature-login → pull request → merge
```

### When Used

- Continuous deployment
- Small teams
- SaaS companies

### Pros

- Simple
- Fast development

### Cons

- Less structure for large teams

---

## 3. Trunk-Based Development

### How It Works

Developers commit frequently to the **main branch** with short-lived branches.

Example

```
main ← small feature branches merged quickly
```

### When Used

- High-speed development
- CI/CD environments

### Pros

- Fast integration
- Encourages small commits

### Cons

- Requires strong testing automation

---

# Strategy Comparison

| Strategy | Best For |
|--------|-----------|
| GitFlow | Large teams with planned releases |
| GitHub Flow | Startups and fast-moving projects |
| Trunk-Based | CI/CD heavy teams |

---

## Answers

### Startup shipping fast

GitHub Flow

Reason: simple workflow and quick deployments.

---

### Large team with scheduled releases

GitFlow

Reason: structured release and hotfix branches.

---

### Example Open Source Project

Example: **Kubernetes**

Uses a structured branching model with release branches.

---

# Task 5 – Git Commands Reference Update

Commands added to `git-commands.md`:

```
git reset --soft
git reset --mixed
git reset --hard
git revert
git reflog
```

---

# Commands Used

```
git reset
git reset --soft
git reset --mixed
git reset --hard
git revert
git log
git reflog
```

---

# What I Learned

1. `git reset` rewrites history while `git revert` safely undoes commits.
2. `git reset --hard` can permanently delete changes.
3. Different branching strategies suit different team sizes and workflows.

---

# Summary

Today I learned how to undo mistakes using Git reset and revert, and explored real-world branching strategies like GitFlow, GitHub Flow, and Trunk-Based Development. These skills are essential for maintaining stable codebases and collaborating effectively in DevOps environments.
