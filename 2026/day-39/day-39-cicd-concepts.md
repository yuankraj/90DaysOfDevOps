# Day 39 – What is CI/CD?

# Objective

Today I learned the fundamentals of CI/CD and why modern software teams rely heavily on automation pipelines.

Before creating pipelines, it is important to understand:
- Why CI/CD exists
- Problems with manual deployments
- Pipeline workflow concepts
- How real-world automation works

This day focused on concepts, workflow understanding, and pipeline structure.

---

# Task 1 – The Problem

## Scenario

A team of 5 developers pushes code manually and deploys directly to production.

---

# What Can Go Wrong?

Problems:
- Code conflicts between developers
- Manual deployment mistakes
- Different environments causing bugs
- Broken production deployments
- No automated testing
- Downtime during deployment
- Hard rollback process

---

# What Does “It Works on My Machine” Mean?

This means:
- The application works in the developer’s local environment
- But fails on another machine/server

Why?
- Different OS
- Missing dependencies
- Different package versions
- Environment variable issues

This is a real problem because production environments must behave consistently.

---

# How Many Times Can Teams Safely Deploy Manually?

Usually:
- Very few deployments per day
- Manual deployments are slow and risky

Without automation:
- Errors increase
- Rollbacks become difficult
- Developers lose productivity

CI/CD enables safe and frequent deployments.

---

# Task 2 – CI vs CD

# 1. Continuous Integration (CI)

Continuous Integration means:
- Developers frequently merge code into a shared repository
- Automated builds and tests run after every push

CI helps catch:
- Bugs
- Build failures
- Integration issues early

### Real Example
Developer pushes code → tests run automatically → build succeeds/fails.

---

# 2. Continuous Delivery (CD)

Continuous Delivery means:
- Code is automatically prepared for deployment
- Production release still requires manual approval

The application is always deployment-ready.

### Real Example
Code passes tests → Docker image built → waits for manual production deployment.

---

# 3. Continuous Deployment

Continuous Deployment means:
- Every successful change is automatically deployed to production
- No manual approval required

### Real Example
Push code → tests pass → automatically deployed live.

---

# Difference Between Delivery & Deployment

| Continuous Delivery | Continuous Deployment |
|--------------------|-----------------------|
| Manual approval needed | Fully automatic |
| Safer for controlled releases | Faster releases |
| Common in enterprises | Common in startups |

---

# Task 3 – Pipeline Anatomy

# Trigger

The event that starts the pipeline.

Examples:
- Git push
- Pull request
- Schedule
- Manual trigger

---

# Stage

A logical pipeline phase.

Examples:
- Build
- Test
- Deploy

---

# Job

A group of related tasks inside a stage.

Example:
- Run unit tests
- Build Docker image

---

# Step

A single command/action inside a job.

Example:
```bash
npm install
```

---

# Runner

The machine/system that executes pipeline jobs.

Examples:
- GitHub-hosted runner
- Jenkins agent
- Self-hosted runner

---

# Artifact

Files generated during a pipeline.

Examples:
- Docker images
- Build binaries
- Test reports

---

# Task 4 – CI/CD Pipeline Diagram

# Text-Based Pipeline

```text
Developer Pushes Code
          │
          ▼
+-------------------+
|   Trigger Event   |
|   GitHub Push     |
+-------------------+
          │
          ▼
+-------------------+
|   Build Stage     |
| Install Packages  |
| Build Application |
+-------------------+
          │
          ▼
+-------------------+
|    Test Stage     |
| Run Unit Tests    |
| Run Lint Checks   |
+-------------------+
          │
          ▼
+-------------------+
| Docker Build Stage|
| Build Docker Img  |
+-------------------+
          │
          ▼
+-------------------+
| Deploy to Staging |
| Docker Compose Up |
+-------------------+
```

---

# Task 5 – Explore in the Wild

# Repository Explored

I explored the GitHub Actions workflow of:

```bash
FastAPI
```

Repository:
```bash
https://github.com/fastapi/fastapi
```

---

# Workflow Folder

```bash
.github/workflows/
```

---

# Observations

## Trigger

Triggered on:
- Push
- Pull Request

---

## Jobs Found

The workflow had multiple jobs:
- Testing
- Linting
- Documentation checks

---

## What the Workflow Does

The pipeline:
- Installs dependencies
- Runs tests
- Validates code formatting
- Checks documentation

This ensures code quality before merging changes.

---

# Common CI/CD Tools

| Tool | Purpose |
|------|----------|
| GitHub Actions | GitHub CI/CD |
| Jenkins | Automation server |
| GitLab CI/CD | GitLab pipelines |
| CircleCI | Cloud CI/CD |
| Azure DevOps | Microsoft CI/CD |

---

# Why CI/CD Matters in DevOps

CI/CD helps teams:
- Deploy faster
- Reduce manual errors
- Improve software quality
- Catch bugs early
- Automate testing & deployment
- Release features continuously

---

# Commands/Concepts Learned

```bash
.github/workflows/
pipeline
runner
artifact
trigger
stage
job
step
```

---

# What I Learned

1. CI/CD automates software testing and deployment.
2. CI catches issues early before production.
3. Pipelines make deployments faster and safer.

---

# Final Reflection

Today I understood:
✅ Why CI/CD exists  
✅ Problems with manual deployments  
✅ CI vs Continuous Delivery vs Continuous Deployment  
✅ Pipeline structure & workflow  
✅ Real-world GitHub Actions workflows  

This was an important foundational step before building real CI/CD pipelines 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
