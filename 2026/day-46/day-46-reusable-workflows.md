# Day 46 – Reusable Workflows & Composite Actions

# Objective

Today I learned how to:
- Create reusable GitHub Actions workflows
- Call workflows like reusable functions
- Pass inputs, secrets, and outputs
- Build custom composite GitHub Actions

This introduced me to one of the most powerful production-level GitHub Actions concepts:
🚀 DRY (Don't Repeat Yourself) CI/CD automation.

---

# Task 1 – Understanding `workflow_call`

# What is a Reusable Workflow?

A reusable workflow is a GitHub Actions workflow that can be called by other workflows.

Instead of rewriting the same CI/CD logic in multiple repositories, teams create reusable workflows and share them across projects.

---

# What is `workflow_call`?

`workflow_call` is a special GitHub Actions trigger that allows one workflow to be invoked by another workflow.

Example:

```yaml
on:
  workflow_call:
```

---

# Reusable Workflow vs Regular Action

| Feature | Reusable Workflow | Regular Action |
|---|---|---|
| Contains jobs | Yes | No |
| Contains steps | Yes | Yes |
| Triggered by | workflow_call | uses |
| Best for | Entire CI/CD pipelines | Reusable step logic |

---

# Where Must Reusable Workflows Live?

Reusable workflows must exist inside:

```bash
.github/workflows/
```

---

# Task 2 – Create Reusable Workflow

# File Created

```bash
.github/workflows/reusable-build.yml
```

---

# reusable-build.yml

```yaml
name: Reusable Build Workflow

on:
  workflow_call:

    inputs:
      app_name:
        required: true
        type: string

      environment:
        required: true
        type: string
        default: staging

    secrets:
      docker_token:
        required: true

    outputs:
      build_version:
        value: ${{ jobs.build.outputs.build_version }}

jobs:
  build:
    runs-on: ubuntu-latest

    outputs:
      build_version: ${{ steps.version.outputs.build_version }}

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Print Build Information
        run: |
          echo "Building ${{ inputs.app_name }} for ${{ inputs.environment }}"

      - name: Check Secret
        run: |
          if [ -n "${{ secrets.docker_token }}" ]; then
            echo "Docker token is set: true"
          fi

      - name: Generate Version
        id: version
        run: |
          SHORT_SHA=$(echo $GITHUB_SHA | cut -c1-7)
          echo "build_version=v1.0-$SHORT_SHA" >> $GITHUB_OUTPUT
```

---

# Observation

This workflow:
✅ Accepts inputs  
✅ Accepts secrets  
✅ Generates outputs  
✅ Can be reused by other workflows  

---

# Task 3 – Caller Workflow

# File Created

```bash
.github/workflows/call-build.yml
```

---

# call-build.yml

```yaml
name: Call Reusable Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml

    with:
      app_name: "my-web-app"
      environment: "production"

    secrets:
      docker_token: ${{ secrets.DOCKER_TOKEN }}

  print-version:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Print Build Version
        run: |
          echo "Build Version: ${{ needs.build.outputs.build_version }}"
```

---

# Observation

In the GitHub Actions tab:
✅ Caller workflow triggered reusable workflow  
✅ Inputs printed successfully  
✅ Secret validated securely  
✅ Output passed to second job  

---

# Task 4 – Reusable Workflow Outputs

# Generated Output Example

```bash
v1.0-a1b2c3d
```

---

# Why Outputs Matter

Outputs help:
- Pass build versions
- Share deployment metadata
- Reuse generated values
- Coordinate jobs across workflows

This is heavily used in enterprise CI/CD pipelines.

---

# Task 5 – Create Composite Action

# Directory Created

```bash
.github/actions/setup-and-greet/
```

---

# action.yml

```yaml
name: Setup and Greet

description: Custom composite GitHub Action

inputs:
  name:
    required: true

  language:
    required: false
    default: en

outputs:
  greeted:
    value: true

runs:
  using: composite

  steps:
    - name: Greeting
      shell: bash

      run: |
        if [ "${{ inputs.language }}" = "en" ]; then
          echo "Hello ${{ inputs.name }}"
        else
          echo "Namaste ${{ inputs.name }}"
        fi

    - name: Print Date
      shell: bash
      run: date

    - name: Print Runner OS
      shell: bash
      run: echo $RUNNER_OS
```

---

# Workflow Using Composite Action

# composite-demo.yml

```yaml
name: Composite Action Demo

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Run Custom Action
        uses: ./.github/actions/setup-and-greet

        with:
          name: Vishal
          language: en
```

---

# Observation

Composite action:
✅ Printed greeting  
✅ Printed current date  
✅ Printed runner OS  

---

# Reusable Workflow vs Composite Action

| Feature | Reusable Workflow | Composite Action |
|---|---|---|
| Triggered by | workflow_call | uses |
| Can contain jobs? | Yes | No |
| Can contain multiple steps? | Yes | Yes |
| Lives where? | .github/workflows | .github/actions |
| Can accept secrets directly? | Yes | No |
| Best for | Entire pipelines | Reusable step groups |

---

# GitHub Actions Concepts Learned

| Concept | Meaning |
|---|---|
| workflow_call | Reusable workflow trigger |
| Composite Action | Reusable step group |
| Inputs | Dynamic workflow parameters |
| Outputs | Share generated values |
| Secrets | Secure credentials |
| uses | Reuse workflows/actions |

---

# Commands & Features Practiced

```bash
workflow_call
inputs
outputs
composite actions
uses
needs
GitHub Actions reuse
```

---

# Files Created

```bash
.github/workflows/reusable-build.yml
.github/workflows/call-build.yml
.github/workflows/composite-demo.yml
.github/actions/setup-and-greet/action.yml
day-46-reusable-workflows.md
```

---

# What I Learned

1. Reusable workflows help eliminate duplicated CI/CD logic.
2. Composite actions simplify repeated workflow steps.
3. Outputs allow workflows to share data dynamically.

---

# Why This Matters in DevOps

These concepts are heavily used in:
- Enterprise CI/CD systems
- Shared DevOps platforms
- Large engineering teams
- Internal developer platforms
- Standardized deployment pipelines

---

# Final Reflection

Today I successfully:
✅ Created reusable workflows  
✅ Passed inputs & secrets securely  
✅ Generated reusable outputs  
✅ Built a custom composite action  
✅ Learned DRY CI/CD automation principles  

This was one of the most advanced and production-grade GitHub Actions topics so far 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
