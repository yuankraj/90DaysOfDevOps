# Day 35 – Multi-Stage Builds & Docker Hub

## Task Summary
Today I learned how to **optimize Docker images using multi-stage builds** and how to **push images to Docker Hub** so they can be shared and used anywhere.

Multi-stage builds help reduce image size and improve security by keeping only the final compiled application in the runtime image.

---

# Task 1 – Problem with Large Images

## Simple Node.js App

Project Structure

```
node-app/
│
├── app.js
├── package.json
└── Dockerfile
```

---

## app.js

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.write("Hello from Docker!");
  res.end();
});

server.listen(3000);
```

---

## package.json

```json
{
  "name": "docker-demo",
  "version": "1.0.0",
  "main": "app.js"
}
```

---

## Single Stage Dockerfile

```Dockerfile
FROM node:18

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["node", "app.js"]
```

---

## Build Image

```bash
docker build -t node-single:v1 .
```

Check size

```bash
docker images
```

Example Result

```
node-single   v1   900MB
```

This image is large because it includes:

- Node runtime
- Build tools
- All dependencies
- Development files

---

# Task 2 – Multi-Stage Build

## Multi-Stage Dockerfile

```Dockerfile
# Stage 1 – Builder
FROM node:18 AS builder

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# Stage 2 – Runtime
FROM node:18-alpine

WORKDIR /app

COPY --from=builder /app .

EXPOSE 3000

CMD ["node", "app.js"]
```

---

## Build Image

```bash
docker build -t node-multistage:v1 .
```

Check size

```bash
docker images
```

Example Result

```
node-multistage   v1   150MB
```

---

## Comparison

| Build Type | Image Size |
|-------------|-----------|
| Single Stage | ~900MB |
| Multi-Stage | ~150MB |

---

## Why Multi-Stage Is Smaller

Multi-stage builds remove:

- Build dependencies
- Temporary files
- Compilers
- Package managers

Only the **final runtime artifact** is included.

---

# Task 3 – Push Image to Docker Hub

## Login to Docker Hub

```bash
docker login
```

Enter your Docker Hub username and password.

---

## Tag Image

Format:

```
username/image-name:tag
```

Example:

```bash
docker tag node-multistage:v1 yourusername/node-demo:v1
```

---

## Push Image

```bash
docker push yourusername/node-demo:v1
```

---

## Verify Push

Check Docker Hub repository:

```
https://hub.docker.com/r/yourusername/node-demo
```

---

## Pull Image from Docker Hub

Remove local image:

```bash
docker rmi yourusername/node-demo:v1
```

Pull it again:

```bash
docker pull yourusername/node-demo:v1
```

Run container:

```bash
docker run -p 3000:3000 yourusername/node-demo:v1
```

---

# Task 4 – Docker Hub Repository

Visit Docker Hub and explore your repository.

Features available:

- Description
- Image tags
- Pull commands
- Automated builds

---

## Tags Example

```
node-demo:v1
node-demo:v2
node-demo:latest
```

---

## Pull Specific Tag

```bash
docker pull yourusername/node-demo:v1
```

---

## Pull Latest Tag

```bash
docker pull yourusername/node-demo:latest
```

The **latest tag** refers to the most recent version.

---

# Task 5 – Docker Image Best Practices

## 1. Use Minimal Base Image

Example:

```
node:18 → 900MB
node:18-alpine → ~150MB
```

---

## 2. Don't Run as Root

Add a user:

```Dockerfile
RUN adduser -D appuser
USER appuser
```

This improves container security.

---

## 3. Combine RUN Commands

Instead of:

```Dockerfile
RUN apt update
RUN apt install curl
```

Use:

```Dockerfile
RUN apt update && apt install -y curl
```

This reduces layers.

---

## 4. Avoid Latest Tags

Bad practice:

```
FROM node:latest
```

Better:

```
FROM node:18-alpine
```

Using specific versions improves reproducibility.

---

# Commands Used

```
docker build
docker images
docker tag
docker login
docker push
docker pull
docker rmi
docker run
```

---

# What I Learned

1. Multi-stage builds significantly reduce Docker image sizes.
2. Docker Hub allows developers to share and distribute container images.
3. Using minimal base images and non-root users improves security and performance.

---

# Docker Hub Repository

Example:

```
https://hub.docker.com/r/yourusername/node-demo
```

(Add your actual Docker Hub link here.)

---

# Screenshots

Add screenshots for:

```
docker-single-stage-size.png
docker-multistage-size.png
docker-push-output.png
docker-hub-repo.png
```

---

# Summary

Today I learned how to optimize Docker images using multi-stage builds and publish them to Docker Hub. This workflow is used in real DevOps pipelines to create smaller, secure, and efficient container images for deployment.
