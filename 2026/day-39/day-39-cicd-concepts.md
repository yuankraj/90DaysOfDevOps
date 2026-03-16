# Day 39 – What is CI/CD?

## Task Summary
Today I learned the **fundamental concepts of CI/CD (Continuous Integration and Continuous Deployment/Delivery)**.  
CI/CD helps automate the process of building, testing, and deploying applications, making software delivery faster and more reliable.

---

# Task 1 – The Problem

Imagine a team of **5 developers pushing code manually and deploying manually**.

### What Can Go Wrong?

- Code conflicts between developers
- Bugs introduced into production
- Broken builds
- Missing dependencies
- Human errors during deployment

---

### What Does "It Works on My Machine" Mean?

This phrase means the application works correctly on the developer's computer but fails in another environment such as staging or production.

Reasons this happens:

- Different operating systems
- Different library versions
- Missing dependencies
- Environment configuration differences

CI/CD solves this by ensuring code runs in **consistent environments**.

---

### How Many Times Can a Team Safely Deploy Manually?

Manual deployments are slow and risky.

Typically:

- 1–2 deployments per day safely
- High chance of mistakes

With CI/CD, teams can deploy **multiple times per day automatically**.

---

# Task 2 – CI vs CD

## Continuous Integration (CI)

Continuous Integration is the practice of **frequently merging code changes into a shared repository**.  
Every commit triggers automated builds and tests to catch issues early.

Example:

A developer pushes code → pipeline runs tests → if tests fail, the build stops.

---

## Continuous Delivery (CD)

Continuous Delivery ensures that code changes are **automatically built, tested, and prepared for deployment**.

Deployment still requires **manual approval**.

Example:

Code passes tests → pipeline builds Docker image → ready to deploy to production.

---

## Continuous Deployment

Continuous Deployment goes one step further.

Every successful change **is automatically deployed to production without manual approval**.

Example:

Code pushed → tests pass → app deployed automatically to production.

---

## Comparison

| Practice | Description |
|--------|-------------|
| Continuous Integration | Automatically build and test code |
| Continuous Delivery | Code ready for deployment but requires approval |
| Continuous Deployment | Fully automated deployment |

---

# Task 3 – Pipeline Anatomy

### Trigger

An event that starts the pipeline.

Examples:

- Git push
- Pull request
- Scheduled job

---

### Stage

A major phase in the pipeline.

Examples:

```
Build
Test
Deploy
```

---

### Job

A group of steps executed within a stage.

Example:

A **build job** that compiles the application.

---

### Step

A single command or action executed in a job.

Example:

```
npm install
npm test
docker build
```

---

### Runner

The machine or container that executes the pipeline jobs.

Examples:

- GitHub Actions Runner
- Jenkins Agent
- GitLab Runner

---

### Artifact

Files produced during a pipeline run.

Examples:

- Compiled binaries
- Docker images
- Test reports

Artifacts can be used in later stages.

---

# Task 4 – CI/CD Pipeline Diagram

Example pipeline flow:

```
Developer Pushes Code
        │
        ▼
Trigger (Git Push)
        │
        ▼
Build Stage
- Install dependencies
- Build application
        │
        ▼
Test Stage
- Run unit tests
- Run integration tests
        │
        ▼
Package Stage
- Build Docker image
- Push to registry
        │
        ▼
Deploy Stage
- Deploy to staging server
```

---

# Task 5 – Explore Open Source Repository

Repository Example:

```
https://github.com/fastapi/fastapi
```

Workflow folder:

```
.github/workflows/
```

Example workflow file:

```
tests.yml
```

---

### What Triggers It?

The workflow runs on:

```
push
pull_request
```

---

### How Many Jobs?

Example pipeline contains multiple jobs:

- lint
- test
- build

---

### What Does It Do?

The workflow:

- Installs dependencies
- Runs Python tests
- Ensures code quality before merging

This ensures that broken code does not enter the main branch.

---

# What I Learned

1. CI/CD automates building, testing, and deploying applications.
2. Continuous Integration helps catch bugs early through automated testing.
3. CI/CD pipelines improve deployment speed, reliability, and developer productivity.

---

# Summary

CI/CD is a key DevOps practice that automates the software delivery process. Instead of manual deployments, pipelines ensure that code is automatically built, tested, and deployed reliably. This allows teams to ship software faster and with fewer errors.
