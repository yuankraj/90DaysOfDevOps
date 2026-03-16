# Day 31 – Dockerfile: Build Your Own Images

## Task Summary
Today I learned how to **write Dockerfiles and build custom Docker images**.  
Dockerfiles allow developers to package applications and dependencies into reusable images.

---

# Task 1 – My First Dockerfile

## Project Folder

```
my-first-image/
 ├── Dockerfile
```

---

## Dockerfile

```Dockerfile
FROM ubuntu:latest

RUN apt update && apt install -y curl

CMD ["echo", "Hello from my custom image!"]
```

---

## Build the Image

```bash
docker build -t my-ubuntu:v1 .
```

---

## Run the Container

```bash
docker run my-ubuntu:v1
```

### Output

```
Hello from my custom image!
```

---

# Task 2 – Dockerfile Instructions

## Project Structure

```
dockerfile-demo/
 ├── Dockerfile
 ├── app.txt
```

---

## Dockerfile Example

```Dockerfile
FROM ubuntu:latest

RUN apt update && apt install -y curl

WORKDIR /app

COPY app.txt /app/

EXPOSE 8080

CMD ["cat", "app.txt"]
```

---

## Explanation of Instructions

| Instruction | Purpose |
|-------------|--------|
| FROM | Base image |
| RUN | Executes commands during build |
| COPY | Copies files from host to container |
| WORKDIR | Sets working directory |
| EXPOSE | Documents container port |
| CMD | Default command executed when container runs |

---

## Build Image

```bash
docker build -t dockerfile-demo:v1 .
```

Run Container

```bash
docker run dockerfile-demo:v1
```

---

# Task 3 – CMD vs ENTRYPOINT

## Dockerfile Using CMD

```Dockerfile
FROM alpine

CMD ["echo", "hello"]
```

Run container

```bash
docker run cmd-test
```

Output:

```
hello
```

Run with custom command

```bash
docker run cmd-test ls
```

Result: `ls` replaces CMD.

---

## Dockerfile Using ENTRYPOINT

```Dockerfile
FROM alpine

ENTRYPOINT ["echo"]
```

Run container

```bash
docker run entrypoint-test hello
```

Output:

```
hello
```

Run with additional arguments

```bash
docker run entrypoint-test hello world
```

Output:

```
hello world
```

---

## CMD vs ENTRYPOINT Summary

| Feature | CMD | ENTRYPOINT |
|-------|------|-----------|
| Default command | Yes | Yes |
| Can be overridden | Yes | No |
| Accepts extra arguments | No | Yes |

### When to Use

Use **CMD** when you want a default command that users can override.

Use **ENTRYPOINT** when the container should behave like a specific executable.

---

# Task 4 – Simple Web App Image

## Project Structure

```
my-website/
 ├── Dockerfile
 └── index.html
```

---

## index.html

```html
<!DOCTYPE html>
<html>
<head>
<title>My Docker Website</title>
</head>
<body>
<h1>Hello from my Docker container!</h1>
<p>This page is served by Nginx.</p>
</body>
</html>
```

---

## Dockerfile

```Dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
```

---

## Build Image

```bash
docker build -t my-website:v1 .
```

---

## Run Container

```bash
docker run -d -p 8080:80 my-website:v1
```

Open browser:

```
http://localhost:8080
```

You should see your custom HTML page.

---

# Task 5 – .dockerignore

## .dockerignore File

```
node_modules
.git
*.md
.env
```

Purpose:

`.dockerignore` prevents unnecessary files from being included in the Docker build context.

Benefits:

- Smaller images
- Faster builds
- Better security

---

# Task 6 – Build Optimization

Docker builds images **layer by layer**.

If a layer hasn't changed, Docker uses **cache** instead of rebuilding it.

Example Dockerfile:

```Dockerfile
FROM node:18

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

CMD ["node", "app.js"]
```

### Why Layer Order Matters

Files that change often should be placed **later in the Dockerfile**.

This prevents rebuilding expensive layers like dependency installation.

---

# Commands Used

```
docker build
docker run
docker images
docker ps
docker stop
docker rm
```

---

# What I Learned

1. Dockerfiles define how images are built.
2. Docker uses **layer caching** to speed up builds.
3. CMD and ENTRYPOINT control container startup behavior.

---

# Screenshots

Add screenshots for:

```
docker-build-output.png
docker-run-custom-image.png
docker-nginx-webpage.png
```

---

# Summary

Today I learned how to create Dockerfiles, build custom images, and run containers from them. This is a key skill in DevOps because Docker images are used in CI/CD pipelines and container orchestration platforms like Kubernetes.
