# Day 45 – Docker Build & Push in GitHub Actions

# Objective

Today I built a complete CI/CD pipeline using GitHub Actions that:
- Automatically builds a Docker image
- Pushes it to Docker Hub
- Tags images dynamically
- Uses GitHub Secrets securely
- Runs automatically on every push to `main`

This was my first real Docker CI/CD automation pipeline 🚀

---

# Task 1 – Preparation

# Files Used

```bash
Dockerfile
.github/workflows/docker-publish.yml
README.md
```

---

# GitHub Secrets Configured

Repository:
```bash
Settings → Secrets and Variables → Actions
```

Secrets added:
```bash
DOCKER_USERNAME
DOCKER_TOKEN
```

---

# Sample Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY . .

CMD ["python3", "-m", "http.server", "8000"]
```

---

# Task 2 – Build Docker Image in CI

# Workflow File

```yaml
name: Docker Publish Pipeline

on:
  push:
    branches:
      - main

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set Short SHA
        id: vars
        run: echo "sha_short=$(echo ${GITHUB_SHA} | cut -c1-7)" >> $GITHUB_OUTPUT

      - name: Log in to Docker Hub
        uses: docker/login-action@v3

        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_TOKEN }}

      - name: Build and Push Docker Image
        uses: docker/build-push-action@v5

        with:
          context: .
          push: true

          tags: |
            ${{ secrets.DOCKER_USERNAME }}/github-actions-demo:latest
            ${{ secrets.DOCKER_USERNAME }}/github-actions-demo:sha-${{ steps.vars.outputs.sha_short }}
```

---

# Observation

✅ Workflow automatically triggered on push to `main`.

The pipeline:
1. Checked out repository code
2. Logged into Docker Hub
3. Built Docker image
4. Tagged image
5. Pushed image automatically

---

# Task 3 – Push to Docker Hub

# Docker Image Tags

```bash
latest
sha-<short-commit-hash>
```

Example:
```bash
yuankraj/github-actions-demo:latest
yuankraj/github-actions-demo:sha-a1b2c3d
```

---

# Observation

On Docker Hub:
✅ Both tags appeared successfully  
✅ Images were downloadable and runnable  

---

# Task 4 – Push Only on Main Branch

# Workflow Trigger

```yaml
on:
  push:
    branches:
      - main
```

---

# Test Performed

Feature branch push:
✅ Workflow triggered  
❌ Image NOT pushed to Docker Hub  

Main branch push:
✅ Image successfully pushed  

---

# Why This Matters

This prevents:
- Unstable feature images
- Unwanted production pushes
- Registry pollution

---

# Task 5 – Add Status Badge

# README Badge

```markdown
![Docker Publish](https://github.com/yuankraj/github-actions-demo/actions/workflows/docker-publish.yml/badge.svg)
```

---

# Observation

README displayed:
✅ Green workflow badge  
✅ Real-time pipeline status  

---

# Task 6 – Pull and Run Docker Image

# Commands Used

```bash
docker pull yuankraj/github-actions-demo:latest

docker run -d -p 8000:8000 yuankraj/github-actions-demo:latest
```

---

# Observation

✅ Image pulled successfully  
✅ Container started successfully  
✅ Application accessible locally  

---

# Full Journey: Git Push → Running Container

# CI/CD Flow

```text
Developer pushes code to GitHub
          │
          ▼
GitHub Actions workflow triggers
          │
          ▼
Repository code checked out
          │
          ▼
Docker image built
          │
          ▼
Docker image tagged
          │
          ▼
GitHub Actions logs into Docker Hub
          │
          ▼
Docker image pushed to Docker Hub
          │
          ▼
Server pulls latest image
          │
          ▼
Container starts running
```

---

# GitHub Actions Concepts Learned

| Concept | Meaning |
|---|---|
| docker/login-action | Login to Docker Hub |
| build-push-action | Build & push images |
| GitHub Secrets | Secure credentials |
| SHA Tagging | Unique image versions |
| Workflow Badge | CI/CD status indicator |

---

# Commands & Features Practiced

```bash
docker build
docker push
docker pull
docker run
GitHub Actions
Docker Hub integration
status badges
```

---

# Files Created

```bash
.github/workflows/docker-publish.yml
day-45-docker-cicd.md
README.md
Dockerfile
```

---

# What I Learned

1. GitHub Actions can fully automate Docker image publishing.
2. Secrets securely manage Docker Hub credentials.
3. CI/CD pipelines automate deployment workflows from code push to container delivery.

---

# Why This Matters in DevOps

This is a real-world production workflow used in:
- CI/CD systems
- Kubernetes deployments
- Microservices platforms
- Cloud-native applications
- Enterprise DevOps pipelines

---

# Docker Hub Repository

```bash
https://hub.docker.com/
```

---

# Final Reflection

Today I successfully:
✅ Built Docker images in CI/CD  
✅ Pushed images automatically to Docker Hub  
✅ Used secure GitHub Secrets  
✅ Added workflow status badges  
✅ Automated container delivery  

This was one of the biggest DevOps milestones in my journey so far 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
