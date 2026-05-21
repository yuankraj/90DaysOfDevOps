# Day 35 – Multi-Stage Builds & Docker Hub

# Objective

Today I learned:
- Multi-stage Docker builds
- Image optimization techniques
- Docker Hub image publishing
- Docker image security best practices

This challenge helped me understand how production-ready Docker images are built and shared.

---

# Task 1 – Problem with Large Images

# Create Simple Node.js App

## app.js
```javascript
console.log("Hello from Multi-Stage Docker Build!");
```

---

# package.json

```json
{
  "name": "docker-demo",
  "version": "1.0.0",
  "main": "app.js"
}
```

---

# Single Stage Dockerfile

## Dockerfile.single
```dockerfile
FROM node:20

WORKDIR /app

COPY . .

CMD ["node", "app.js"]
```

---

# Build Single Stage Image

## Input
```bash
docker build -f Dockerfile.single -t node-single:v1 .
```

---

# Check Image Size

## Input
```bash
docker images
```

## Output
```bash
node-single   v1   1.1GB
```

---

# Observation

⚠️ Single-stage image size was very large.

Reason:
- Full Node.js environment included
- Build tools and dependencies included
- Unnecessary files inside image

---

# Task 2 – Multi-Stage Build

# Multi-Stage Dockerfile

## Dockerfile
```dockerfile
# Stage 1 - Builder
FROM node:20 AS builder

WORKDIR /app

COPY . .

# Stage 2 - Production
FROM alpine

WORKDIR /app

COPY --from=builder /app/app.js .

RUN apk add --no-cache nodejs

CMD ["node", "app.js"]
```

---

# Build Multi-Stage Image

## Input
```bash
docker build -t node-multistage:v1 .
```

---

# Check Image Size Again

## Input
```bash
docker images
```

## Output
```bash
node-multistage   v1   60MB
```

---

# Image Size Comparison

| Build Type | Size |
|------------|------|
| Single Stage | 1.1GB |
| Multi-Stage | 60MB |

---

# Why Multi-Stage is Smaller?

Multi-stage builds:
- Remove unnecessary build tools
- Copy only required artifacts
- Exclude development dependencies
- Use lightweight production images

This improves:
- Security
- Performance
- Deployment speed

---

# Task 3 – Push to Docker Hub

# Login to Docker Hub

## Input
```bash
docker login
```

---

# Tag Docker Image

## Input
```bash
docker tag node-multistage:v1 yuankraj/node-multistage:v1
```

---

# Push Image

## Input
```bash
docker push yuankraj/node-multistage:v1
```

---

# Verify Docker Hub Repository

## Observation
✅ Image successfully uploaded to Docker Hub.

---

# Pull Image Again

# Remove Local Image

## Input
```bash
docker rmi yuankraj/node-multistage:v1
```

---

# Pull from Docker Hub

## Input
```bash
docker pull yuankraj/node-multistage:v1
```

### Observation
✅ Image downloaded successfully from Docker Hub.

---

# Task 4 – Docker Hub Repository

# Docker Hub Features Explored

- Repository descriptions
- Image tags
- Latest tag behavior
- Version management

---

# Pull Specific Tag

## Input
```bash
docker pull yuankraj/node-multistage:v1
```

---

# Pull Latest Tag

## Input
```bash
docker pull yuankraj/node-multistage:latest
```

---

# Difference Between Tags

| Tag | Purpose |
|-----|----------|
| latest | Default newest image |
| v1 | Specific stable version |
| v2 | Updated release |

---

# Why Version Tags Matter?

Benefits:
- Stable deployments
- Rollback capability
- Better CI/CD pipelines
- Controlled upgrades

---

# Task 5 – Docker Image Best Practices

# Use Minimal Base Image

## Before
```dockerfile
FROM ubuntu
```

---

## After
```dockerfile
FROM alpine
```

---

# Size Comparison

| Base Image | Size |
|------------|------|
| Ubuntu | Large |
| Alpine | Very Small |

---

# Run Container as Non-Root User

## Secure Dockerfile Example
```dockerfile
FROM alpine

RUN adduser -D appuser

USER appuser

CMD ["sh"]
```

---

# Why Avoid Root User?

Benefits:
- Better security
- Reduced attack surface
- Production best practice

---

# Combine RUN Commands

## Before
```dockerfile
RUN apt update

RUN apt install curl
```

---

## After
```dockerfile
RUN apt update && apt install -y curl
```

---

# Why Combine RUN Commands?

Benefits:
- Fewer layers
- Smaller image size
- Faster builds

---

# Avoid latest Tag

## Bad Practice
```dockerfile
FROM node:latest
```

---

## Better Practice
```dockerfile
FROM node:20-alpine
```

---

# Why Avoid latest?

Because:
- Builds become unpredictable
- Updates may break applications
- Harder debugging

---

# Important Commands Learned

| Command | Purpose |
|----------|----------|
| docker build | Build image |
| docker images | List images |
| docker login | Login to Docker Hub |
| docker tag | Tag image |
| docker push | Upload image |
| docker pull | Download image |

---

# Commands Used

```bash
docker build
docker images
docker login
docker tag
docker push
docker pull
docker rmi
```

---

# What I Learned

- Multi-stage builds drastically reduce image size.
- Smaller images improve security & deployment speed.
- Docker Hub stores and distributes images globally.
- Running containers as non-root improves security.
- Version tagging is critical for CI/CD pipelines.

---

# Why This Matters in DevOps

These concepts are critical because:
- Production images must be optimized
- CI/CD pipelines build & push images automatically
- Secure containers reduce vulnerabilities
- Small images deploy faster in Kubernetes & cloud environments

---

# Final Summary

Today I practiced:
- Multi-stage Docker builds
- Docker image optimization
- Docker Hub image publishing
- Security best practices
- Version tagging & image management

This challenge gave me real-world understanding of how professional Docker images are built, optimized, and shared globally.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
