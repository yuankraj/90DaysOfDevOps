# Day 40 – Your First GitHub Actions Workflow

# Objective

Today I created and executed my very first GitHub Actions workflow.

This was my first real CI/CD pipeline running in the cloud using GitHub Actions 🚀

I learned:
- How workflows are triggered
- How jobs and steps work
- How GitHub runners execute automation
- How to debug failed pipelines

---

# Repository Created

```bash
github-actions-practice
```

---

# Folder Structure

```bash
.github/
└── workflows/
    └── hello.yml
```

---

# Task 1 – Setup

## Commands Used

```bash
mkdir github-actions-practice

cd github-actions-practice

git init

mkdir -p .github/workflows
```

---

# Task 2 – Hello Workflow

# hello.yml

```yaml
name: First GitHub Actions Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Print Greeting
        run: echo "Hello from GitHub Actions!"
```

---

# Workflow Result

✅ Pipeline executed successfully

The workflow automatically triggered after every push to GitHub.

---

# What Happened?

1. GitHub detected a push event
2. A runner machine was created
3. Repository code was checked out
4. Commands were executed
5. Workflow completed successfully

---

# Task 3 – Workflow Anatomy

# on:

Defines the trigger event.

Example:
```yaml
on:
  push:
```

Meaning:
- Run workflow whenever code is pushed.

---

# jobs:

Contains all jobs inside the workflow.

Example:
```yaml
jobs:
```

---

# runs-on:

Defines the operating system for the runner.

Example:
```yaml
runs-on: ubuntu-latest
```

---

# steps:

List of actions/commands executed inside a job.

Example:
```yaml
steps:
```

---

# uses:

Uses a pre-built GitHub Action.

Example:
```yaml
uses: actions/checkout@v4
```

---

# run:

Executes shell commands.

Example:
```yaml
run: echo "Hello"
```

---

# name:

Human-readable name for workflow steps.

Example:
```yaml
name: Print Greeting
```

---

# Task 4 – Add More Steps

# Updated hello.yml

```yaml
name: First GitHub Actions Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Print Greeting
        run: echo "Hello from GitHub Actions!"

      - name: Print Date & Time
        run: date

      - name: Print Branch Name
        run: echo "Branch: ${{ github.ref_name }}"

      - name: List Repository Files
        run: ls -la

      - name: Print Runner OS
        run: echo $RUNNER_OS
```

---

# Output Observed

The workflow successfully displayed:
- Current date & time
- Branch name
- Repository files
- Runner operating system

---

# Task 5 – Break the Pipeline

# Intentional Failure

```yaml
- name: Fail Pipeline
  run: exit 1
```

---

# What Happened?

❌ Pipeline failed

GitHub Actions showed:
- Red cross mark
- Failed step
- Error logs

---

# How I Fixed It

Removed:
```yaml
exit 1
```

Then pushed again.

✅ Pipeline became green again.

---

# What a Failed Pipeline Looks Like

A failed pipeline:
- Shows red ❌ status
- Stops execution at failed step
- Displays error logs for debugging

---

# How to Read Errors

GitHub Actions provides:
- Step-by-step logs
- Exact failing command
- Exit code
- Error message

This helps identify problems quickly.

---

# GitHub Actions Concepts Learned

| Concept | Meaning |
|---------|----------|
| Workflow | Automation pipeline |
| Runner | Machine executing jobs |
| Job | Group of steps |
| Step | Individual command/action |
| Trigger | Event starting workflow |
| Action | Reusable automation component |

---

# Commands Used

```bash
git init
git add .
git commit -m "Added GitHub Actions workflow"
git push origin main
```

---

# Files Created

```bash
.github/workflows/hello.yml
day-40-first-workflow.md
```

---

# What I Learned

1. GitHub Actions automates CI/CD workflows.
2. Every push can trigger automated jobs.
3. Failed pipelines are useful because they help catch issues early.

---

# Why GitHub Actions Matters in DevOps

GitHub Actions helps automate:
- Testing
- Building
- Docker image creation
- Deployments
- Security scanning
- CI/CD workflows

This is one of the most important DevOps automation tools today.

---

# Final Reflection

Today I successfully:
✅ Created my first GitHub Actions workflow  
✅ Triggered automated pipelines  
✅ Understood workflow anatomy  
✅ Debugged a failed pipeline  
✅ Learned cloud-based CI/CD basics  

Seeing my first green pipeline felt amazing 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
