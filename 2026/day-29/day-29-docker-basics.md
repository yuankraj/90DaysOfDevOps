# Day 29 – Introduction to Docker

# Objective

Today I started learning Docker and containerization fundamentals.

I practiced:
- Understanding containers and Docker architecture
- Installing Docker
- Running my first containers
- Managing containers using Docker commands
- Exploring detached mode, logs, and port mapping

This challenge helped me understand the foundation of modern DevOps deployments.

---

# Task 1 – What is Docker?

# What is a Container?

A container is a lightweight package that contains:
- Application code
- Dependencies
- Libraries
- Runtime environment

Containers help applications run consistently across different systems.

---

# Why Do We Need Containers?

Containers solve:
- "Works on my machine" problems
- Dependency conflicts
- Environment inconsistency

They make deployment faster, portable, and reliable.

---

# Containers vs Virtual Machines

| Containers | Virtual Machines |
|------------|------------------|
| Lightweight | Heavy |
| Share host OS kernel | Separate OS |
| Faster startup | Slower startup |
| Less resource usage | More resource usage |
| Best for microservices | Best for full isolation |

---

# Docker Architecture

Docker consists of:

| Component | Purpose |
|-----------|----------|
| Docker Client | User commands (`docker run`) |
| Docker Daemon | Background service managing containers |
| Docker Images | Blueprints/templates |
| Docker Containers | Running instances of images |
| Docker Registry | Stores images (Docker Hub) |

---

# Docker Architecture Flow

```bash
User → Docker Client → Docker Daemon → Container/Image
                         ↓
                    Docker Hub
```

---

# Task 2 – Install Docker

# Verify Docker Installation

## Input
```bash
docker --version
```

## Output
```bash
Docker version 26.0.0
```

### Observation
- Docker installed successfully.

---

# Run Hello World Container

## Input
```bash
docker run hello-world
```

## Output
```bash
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### Observation
- Docker downloaded image from Docker Hub
- Created and ran container successfully

---

# Task 3 – Run Real Containers

# Run Nginx Container

## Input
```bash
docker run -d -p 8080:80 --name nginx-container nginx
```

### Observation
- Nginx container started successfully.

---

# Access in Browser

```bash
http://localhost:8080
```

### Result
- Nginx welcome page displayed successfully.

---

# Run Ubuntu Interactive Container

## Input
```bash
docker run -it ubuntu bash
```

### Observation
- Entered Ubuntu container shell.
- Container behaves like mini Linux system.

---

# Explore Inside Container

## Commands Used
```bash
ls
pwd
cat /etc/os-release
```

---

# List Running Containers

## Input
```bash
docker ps
```

---

# List All Containers

## Input
```bash
docker ps -a
```

---

# Stop Container

## Input
```bash
docker stop nginx-container
```

---

# Remove Container

## Input
```bash
docker rm nginx-container
```

---

# Task 4 – Explore Docker Features

# Run Detached Container

## Input
```bash
docker run -d nginx
```

### Observation
- Container runs in background.
- Terminal remains free.

---

# Run Named Container

## Input
```bash
docker run -d --name my-nginx nginx
```

### Observation
- Easier container management using custom names.

---

# Port Mapping

## Input
```bash
docker run -d -p 9090:80 nginx
```

### Meaning
```bash
Host Port 9090 → Container Port 80
```

---

# Check Container Logs

## Input
```bash
docker logs my-nginx
```

### Observation
- Displays container output and server logs.

---

# Execute Command Inside Running Container

## Input
```bash
docker exec -it my-nginx bash
```

### Observation
- Entered running container shell.

---

# Important Docker Commands Learned

| Command | Purpose |
|----------|----------|
| docker run | Run container |
| docker ps | List running containers |
| docker ps -a | List all containers |
| docker stop | Stop container |
| docker rm | Remove container |
| docker logs | View container logs |
| docker exec | Run command inside container |

---

# Commands Used

```bash
docker --version
docker run
docker ps
docker ps -a
docker stop
docker rm
docker logs
docker exec
```

---

# What I Learned

- Containers package applications with dependencies.
- Docker containers are lightweight compared to VMs.
- Docker Hub stores reusable images.
- Port mapping exposes container services externally.
- Detached mode allows containers to run in background.

---

# Why Docker Matters in DevOps

Docker is important because:
- CI/CD pipelines use containers
- Kubernetes runs containers
- Applications become portable
- Deployment becomes faster and more reliable

Docker is one of the core technologies in modern DevOps workflows.

---

# Final Summary

Today I practiced:
- Docker fundamentals
- Running containers
- Interactive vs detached containers
- Container logs & management
- Port mapping & container networking

This challenge gave me my first hands-on experience with Docker and containerization.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
