# Day 41 – Triggers & Matrix Builds

# Objective

Today I explored advanced GitHub Actions concepts:
- Workflow triggers
- Pull Request automation
- Scheduled workflows
- Manual workflows
- Matrix builds
- Parallel job execution

This helped me understand how professional CI/CD pipelines automate workflows across multiple environments and operating systems.

---

# Task 1 – Pull Request Trigger

# File Created

```bash
.github/workflows/pr-check.yml
```

---

# pr-check.yml

```yaml
name: PR Check Workflow

on:
  pull_request:
    branches:
      - main

jobs:
  pr-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Print PR Branch
        run: echo "PR check running for branch: ${{ github.head_ref }}"
```

---

# What Happened?

When I:
1. Created a feature branch
2. Pushed code
3. Opened a Pull Request

GitHub Actions automatically triggered the workflow.

---

# Observation

✅ The workflow appeared directly on the Pull Request page.

This helps teams automatically:
- Run tests
- Validate code
- Prevent broken merges

before merging into `main`.

---

# Task 2 – Scheduled Trigger

# Cron Workflow Example

```yaml
name: Scheduled Workflow

on:
  schedule:
    - cron: '0 0 * * *'

jobs:
  daily-job:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Running scheduled workflow"
```

---

# What This Cron Means

```bash
0 0 * * *
```

Means:
✅ Run every day at midnight UTC.

---

# Cron Expression for Every Monday at 9 AM

```bash
0 9 * * 1
```

Explanation:
- 0 → minute
- 9 → hour
- * → every day of month
- * → every month
- 1 → Monday

---

# Task 3 – Manual Trigger

# File Created

```bash
.github/workflows/manual.yml
```

---

# manual.yml

```yaml
name: Manual Workflow

on:
  workflow_dispatch:

    inputs:
      environment:
        description: "Choose Environment"
        required: true
        default: "staging"

jobs:
  manual-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print Environment
        run: echo "Environment selected: ${{ github.event.inputs.environment }}"
```

---

# What Happened?

I manually triggered the workflow from:
```bash
GitHub → Actions → Run Workflow
```

and selected:
```bash
staging
```

The pipeline printed:
```bash
Environment selected: staging
```

---

# Why Manual Triggers Matter

Useful for:
- Production deployments
- Emergency rollbacks
- Manual testing
- Database migrations

---

# Task 4 – Matrix Builds

# File Created

```bash
.github/workflows/matrix.yml
```

---

# matrix.yml

```yaml
name: Matrix Build Workflow

on:
  push:

jobs:
  matrix-build:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]

        python-version:
          - "3.10"
          - "3.11"
          - "3.12"

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5

        with:
          python-version: ${{ matrix.python-version }}

      - name: Print Python Version
        run: python --version
```

---

# Total Jobs Executed

Matrix:
- 3 Python versions
- 2 Operating Systems

Total Jobs:
```bash
3 × 2 = 6 jobs
```

---

# Observation

GitHub Actions ran all jobs:
✅ In parallel  
✅ Automatically  
✅ On different operating systems  

This is extremely useful for testing applications across environments.

---

# Task 5 – Exclude & Fail-Fast

# Updated Matrix Configuration

```yaml
strategy:
  fail-fast: false

  matrix:
    os: [ubuntu-latest, windows-latest]

    python-version:
      - "3.10"
      - "3.11"
      - "3.12"

    exclude:
      - os: windows-latest
        python-version: "3.10"
```

---

# What Exclude Does

This prevents:
```bash
Windows + Python 3.10
```

from running.

---

# fail-fast: true vs false

| Setting | Behavior |
|---------|-----------|
| true (default) | Stops remaining jobs if one fails |
| false | Continues running all jobs even if one fails |

---

# Observation

With:
```yaml
fail-fast: false
```

even when one job failed:
✅ Remaining jobs continued running.

This helps teams:
- Detect multiple failures
- Save debugging time

---

# GitHub Actions Concepts Learned

| Concept | Meaning |
|---------|----------|
| pull_request | Trigger on PR activity |
| schedule | Cron-based automation |
| workflow_dispatch | Manual trigger |
| matrix | Parallel environment testing |
| fail-fast | Stop/continue on failures |
| exclude | Skip specific matrix combinations |

---

# Commands & Features Practiced

```bash
pull_request
schedule
workflow_dispatch
matrix builds
fail-fast
exclude
parallel jobs
cron syntax
```

---

# Files Created

```bash
.github/workflows/pr-check.yml
.github/workflows/manual.yml
.github/workflows/matrix.yml
day-41-triggers.md
```

---

# What I Learned

1. GitHub Actions supports multiple workflow triggers.
2. Matrix builds allow parallel testing across environments.
3. Manual workflows are useful for controlled deployments.

---

# Why This Matters in DevOps

These concepts are heavily used in:
- CI/CD pipelines
- Automated testing
- Cross-platform validation
- Production deployment workflows
- Enterprise automation systems

---

# Final Reflection

Today I successfully learned:
✅ PR-based workflows  
✅ Scheduled automation  
✅ Manual workflow execution  
✅ Matrix builds  
✅ Parallel job execution  
✅ Fail-fast behavior  

This was a major step toward building production-ready CI/CD pipelines 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
