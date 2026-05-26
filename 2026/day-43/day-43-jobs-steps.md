# Day 43 – Jobs, Steps, Env Vars & Conditionals

# Objective

Today I learned how to control workflow execution in GitHub Actions using:
- Multi-job workflows
- Job dependencies
- Environment variables
- Job outputs
- Conditionals
- Smart pipeline logic

This helped me understand how real CI/CD pipelines manage workflow execution and automate decision-making.

---

# Task 1 – Multi-Job Workflow

# File Created

```bash
.github/workflows/multi-job.yml
```

---

# multi-job.yml

```yaml
name: Multi Job Workflow

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build Step
        run: echo "Building the app"

  test:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Test Step
        run: echo "Running tests"

  deploy:
    runs-on: ubuntu-latest
    needs: test

    steps:
      - name: Deploy Step
        run: echo "Deploying"
```

---

# Observation

The workflow graph showed:

```text
build → test → deploy
```

Meaning:
- `test` only runs after `build`
- `deploy` only runs after `test`

---

# What does `needs:` do?

`needs:` creates job dependencies.

It ensures:
- Jobs run in sequence
- Failed jobs stop dependent jobs
- Pipelines maintain proper execution order

---

# Task 2 – Environment Variables

# env-vars.yml

```yaml
name: Environment Variables Workflow

on:
  push:

env:
  APP_NAME: myapp

jobs:
  env-demo:
    runs-on: ubuntu-latest

    env:
      ENVIRONMENT: staging

    steps:
      - name: Print Variables
        env:
          VERSION: 1.0.0

        run: |
          echo "App: $APP_NAME"
          echo "Environment: $ENVIRONMENT"
          echo "Version: $VERSION"

      - name: Print GitHub Context Variables
        run: |
          echo "Commit SHA: $GITHUB_SHA"
          echo "Triggered By: $GITHUB_ACTOR"
```

---

# Environment Variable Levels

| Level | Scope |
|---|---|
| Workflow Level | Entire workflow |
| Job Level | Specific job |
| Step Level | Single step |

---

# GitHub Context Variables Used

```bash
GITHUB_SHA
GITHUB_ACTOR
```

These provide:
- Commit information
- Triggering user details

---

# Task 3 – Job Outputs

# outputs.yml

```yaml
name: Job Outputs Workflow

on:
  push:

jobs:
  generate-date:
    runs-on: ubuntu-latest

    outputs:
      today: ${{ steps.date-step.outputs.today }}

    steps:
      - name: Generate Date
        id: date-step

        run: echo "today=$(date)" >> $GITHUB_OUTPUT

  print-date:
    runs-on: ubuntu-latest
    needs: generate-date

    steps:
      - name: Print Date
        run: echo "Today's date is ${{ needs.generate-date.outputs.today }}"
```

---

# Observation

The second job successfully received:
✅ Date output from the first job.

---

# Why Use Job Outputs?

Outputs are useful for:
- Passing data between jobs
- Sharing build artifacts info
- Passing version numbers
- Deployment decisions
- Dynamic pipeline behavior

---

# Task 4 – Conditionals

# conditional.yml

```yaml
name: Conditional Workflow

on:
  push:
  pull_request:

jobs:
  conditional-job:
    runs-on: ubuntu-latest

    if: github.event_name == 'push'

    steps:
      - name: Main Branch Only
        if: github.ref == 'refs/heads/main'

        run: echo "Running on main branch"

      - name: Intentional Failure
        run: exit 1
        continue-on-error: true

      - name: Run After Failure
        if: failure()

        run: echo "Previous step failed"
```

---

# What `continue-on-error: true` Does

Normally:
❌ Workflow stops on failure.

With:
```yaml
continue-on-error: true
```

✅ Workflow continues even if the step fails.

---

# What `failure()` Does

Runs the step only when:
- Previous step fails

Useful for:
- Notifications
- Cleanup tasks
- Error reporting

---

# Task 5 – Smart Pipeline

# smart-pipeline.yml

```yaml
name: Smart Pipeline

on:
  push:

jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Linting code"

  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Running tests"

  summary:
    runs-on: ubuntu-latest

    needs: [lint, test]

    steps:
      - name: Branch Information
        run: |
          if [[ "${GITHUB_REF}" == "refs/heads/main" ]]; then
            echo "Main branch push"
          else
            echo "Feature branch push"
          fi

      - name: Print Commit Message
        run: echo "${{ github.event.commits[0].message }}"
```

---

# Observation

The pipeline:
✅ Ran `lint` and `test` in parallel  
✅ Waited for both jobs  
✅ Executed summary afterward  

---

# GitHub Actions Concepts Learned

| Concept | Meaning |
|---|---|
| needs | Job dependency |
| env | Environment variables |
| outputs | Share data between jobs |
| if | Conditional execution |
| failure() | Detect failed steps |
| continue-on-error | Ignore step failure |

---

# Commands & Features Practiced

```bash
needs:
outputs:
env:
if:
failure()
continue-on-error
GITHUB_OUTPUT
```

---

# Files Created

```bash
.github/workflows/multi-job.yml
.github/workflows/env-vars.yml
.github/workflows/outputs.yml
.github/workflows/conditional.yml
.github/workflows/smart-pipeline.yml
day-43-jobs-steps.md
```

---

# What I Learned

1. Multi-job workflows create structured CI/CD pipelines.
2. Environment variables simplify reusable configuration.
3. Conditionals allow intelligent pipeline behavior.

---

# Why This Matters in DevOps

These concepts are heavily used in:
- CI/CD automation
- Deployment workflows
- Release pipelines
- Production approvals
- Dynamic infrastructure workflows

---

# Final Reflection

Today I successfully learned:
✅ Multi-job workflow execution  
✅ Job dependency chains  
✅ Environment variables  
✅ Passing outputs between jobs  
✅ Conditional workflow execution  
✅ Smart CI/CD pipeline logic  

This was a major step toward building production-ready GitHub Actions workflows 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
