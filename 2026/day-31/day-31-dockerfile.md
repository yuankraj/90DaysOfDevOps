# Day 31 – Dockerfile: Build Your Own Images

# Objective

Today I learned how to write Dockerfiles and build custom Docker images from scratch.

I practiced:
- Writing Dockerfiles
- Building custom images
- Understanding Dockerfile instructions
- CMD vs ENTRYPOINT
- Building a static website image
- Using `.dockerignore`
- Understanding Docker layer caching

This challenge helped me understand how real Dockerized applications are built and shipped.

---

# Task 1 – My First Dockerfile

# Create Project Folder

## Input
```bash
mkdir my-first-image
cd my-first-image
```

---

# Create Dockerfile

## Dockerfile
```dockerfile
FROM ubuntu

RUN apt update && apt install -y curl

CMD ["echo", "Hello from my custom image!"]
```

---

# Build Docker Image

## Input
```bash
docker build -t my-ubuntu:v1 .
```

### Observation
- Docker image built successfully.

---

# Run Container

## Input
```bash
docker run my-ubuntu:v1
```

## Output
```bash
Hello from my custom image!
```

---

# What I Learned

- `FROM` defines base image
- `RUN` executes commands during build
- `CMD` defines default runtime command

---

# Task 2 – Dockerfile Instructions

# Create New Dockerfile

## Dockerfile
```dockerfile
FROM ubuntu

RUN apt update && apt install -y nginx

WORKDIR /app

COPY index.html /app

EXPOSE 80

CMD ["bash"]
```

---

# Build Image

## Input
```bash
docker build -t custom-nginx:v1 .
```

---

# Run Container

## Input
```bash
docker run -it custom-nginx:v1
```

---

# Dockerfile Instructions Explained

| Instruction | Purpose |
|-------------|----------|
| FROM | Base image |
| RUN | Executes commands |
| COPY | Copies files |
| WORKDIR | Sets working directory |
| EXPOSE | Documents container port |
| CMD | Default container command |

---

# Task 3 – CMD vs ENTRYPOINT

# Using CMD

## Dockerfile
```dockerfile
FROM ubuntu

CMD ["echo", "hello"]
```

---

# Run Container

## Input
```bash
docker run cmd-demo
```

## Output
```bash
hello
```

---

# Override CMD

## Input
```bash
docker run cmd-demo ls
```

### Observation
- CMD replaced by custom command.

---

# Using ENTRYPOINT

## Dockerfile
```dockerfile
FROM ubuntu

ENTRYPOINT ["echo"]
```

---

# Run Container

## Input
```bash
docker run entry-demo hello world
```

## Output
```bash
hello world
```

---

# CMD vs ENTRYPOINT

| CMD | ENTRYPOINT |
|-----|-------------|
| Default command | Fixed executable |
| Easily overridden | Arguments appended |
| Flexible runtime behavior | Consistent behavior |

---

# When to Use CMD?

Use CMD:
- For default commands
- Flexible container behavior

---

# When to Use ENTRYPOINT?

Use ENTRYPOINT:
- For fixed application containers
- When container should always run same executable

---

# Task 4 – Build Simple Web App Image

# Create index.html

## index.html
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Docker Website</title>
</head>
<body>
    <h1>Hello from Docker Nginx!</h1>
</body>
</html>
```

---

# Create Dockerfile

## Dockerfile
```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
```

---

# Build Image

## Input
```bash
docker build -t my-website:v1 .
```

---

# Run Website Container

## Input
```bash
docker run -d -p 8080:80 my-website:v1
```

---

# Access Website

```bash
http://localhost:8080
```

### Observation
- Custom HTML page displayed successfully.

---

# Task 5 – .dockerignore

# Create .dockerignore File

## .dockerignore
```bash
node_modules
.git
*.md
.env
```

---

# Why Use .dockerignore?

Benefits:
- Smaller build context
- Faster builds
- Improved security
- Avoid unnecessary files inside images

---

# Verify Ignored Files

## Observation
- Ignored files were not copied during build process.

---

# Task 6 – Build Optimization

# Initial Build

## Input
```bash
docker build -t cache-demo:v1 .
```

---

# Modify One Line & Rebuild

### Observation
- Docker reused cached layers.
- Only changed layers rebuilt.

---

# Why Layer Order Matters?

Docker builds layer by layer.

Best practice:
- Put stable commands first
- Put frequently changing files later

This improves:
- Build speed
- Cache reuse
- CI/CD efficiency

---

# Example Optimized Dockerfile

```dockerfile
FROM node:18

COPY package.json .

RUN npm install

COPY . .
```

---

# Why This Order is Better?

- Dependencies cached separately
- Code changes do not reinstall packages every time

---

# Important Commands Learned

| Command | Purpose |
|----------|----------|
| docker build | Build image |
| docker run | Run container |
| docker images | List images |
| docker exec | Run command inside container |
| docker logs | View logs |

---

# Commands Used

```bash
docker build
docker run
docker images
docker exec
docker logs
```

---

# What I Learned

- Dockerfiles define how images are built.
- Docker images are built layer by layer.
- CMD and ENTRYPOINT behave differently.
- `.dockerignore` improves build efficiency.
- Layer caching speeds up Docker builds.

---

# Why Dockerfiles Matter in DevOps

Dockerfiles are critical because:
- CI/CD pipelines build images automatically
- Applications become portable
- Infrastructure becomes reproducible
- Deployments become faster and consistent

Dockerfiles are one of the most important DevOps skills.

---

# Final Summary

Today I practiced:
- Writing Dockerfiles
- Building custom Docker images
- Understanding image layers & cache
- CMD vs ENTRYPOINT
- Building custom Nginx web app containers
- Docker build optimization

This challenge gave me real hands-on experience with building production-style Docker images.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
