# Day 40 – First GitHub Actions Workflow

## Task Summary
Today I created my **first CI pipeline using GitHub Actions**.

The workflow runs automatically whenever code is pushed to the repository.  
It checks out the code, prints messages, and runs basic commands.

---

# Workflow File

Location:

```
.github/workflows/hello.yml
```

Workflow YAML:

```yaml
name: Hello Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Print greeting
        run: echo "Hello from GitHub Actions!"

      - name: Show date and time
        run: date

      - name: Show branch name
        run: echo "Branch: ${{ github.ref_name }}"

      - name: List files
        run: ls -la

      - name: Show runner OS
        run: echo "Running on $RUNNER_OS"
```

---

# Workflow Anatomy

### `on:`

Defines **what triggers the workflow**.

Example:

```
on:
  push:
```

This means the workflow runs whenever code is pushed.

---

### `jobs:`

Defines one or more **jobs** that will run in the pipeline.

Example:

```
jobs:
  greet:
```

Here the job name is **greet**.

---

### `runs-on:`

Specifies the **runner environment** where the job will run.

Example:

```
runs-on: ubuntu-latest
```

GitHub provides a virtual Ubuntu machine to run the job.

---

### `steps:`

A list of actions executed inside the job.

Each step performs a task such as running commands or using actions.

---

### `uses:`

Used to run a **prebuilt GitHub Action**.

Example:

```
uses: actions/checkout@v4
```

This action downloads the repository code onto the runner.

---

### `run:`

Executes a **shell command**.

Example:

```
run: echo "Hello from GitHub Actions!"
```

---

### `name:` (step name)

Provides a readable label for the step.

Example:

```
- name: Print greeting
```

---

# Breaking the Pipeline

I intentionally added a failing command:

```
run: exit 1
```

Result:

- The workflow turned **red**
- The job stopped executing
- GitHub showed an error message

This helped me understand how to debug pipelines.

---

# Fixing the Error

I removed the failing command and pushed again.

Result:

The pipeline ran successfully and showed a **green check mark**.

---

# What I Learned

1. GitHub Actions automates tasks whenever events like `push` occur.
2. Workflows are defined using YAML files in `.github/workflows`.
3. Pipelines run on GitHub-hosted runners like Ubuntu.

---

# Screenshot

Add screenshot of successful run:

```
github-actions-green-run.png
```

---

# Summary

Today I created my first CI workflow using GitHub Actions. The pipeline runs automatically on every push and executes commands in the cloud. This marks the beginning of building real CI/CD pipelines.
