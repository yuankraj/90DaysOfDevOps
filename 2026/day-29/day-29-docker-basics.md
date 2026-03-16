# Day 29 – Introduction to Docker

## Task Summary
Today I started learning **Docker**, one of the most important tools in modern DevOps.  
I learned what containers are, how they differ from virtual machines, and ran my first containers from Docker Hub.

---

# Task 1 – What is Docker?

## What is a Container?

A **container** is a lightweight package that includes an application and all its dependencies needed to run it. Containers ensure that the application runs the same way on every machine.

### Why Do We Need Containers?

Containers solve the classic problem:

> "It works on my machine but not on yours."

They provide:

- Consistent environments
- Fast deployment
- Isolation between applications
- Lightweight resource usage

---

## Containers vs Virtual Machines

| Feature | Containers | Virtual Machines |
|--------|------------|------------------|
| OS | Share host OS kernel | Each VM has its own OS |
| Startup Time | Seconds | Minutes |
| Resource Usage | Lightweight | Heavy |
| Isolation | Process-level | Full OS-level |
| Example Tools | Docker | VMware, VirtualBox |

Example architecture:

```
Virtual Machine Model

Hardware
  ↓
Hypervisor
  ↓
Guest OS
  ↓
Application


Container Model

Hardware
  ↓
Host OS
  ↓
Docker Engine
  ↓
Containers
```

---

## Docker Architecture

Docker uses a **client-server architecture**.

### Components

| Component | Description |
|----------|-------------|
| Docker Client | Command line tool (`docker`) used to interact with Docker |
| Docker Daemon | Background service that manages containers |
| Docker Images | Templates used to create containers |
| Docker Containers | Running instances of Docker images |
| Docker Registry | Storage for images (e.g., Docker Hub) |

### Simple Architecture Diagram

```
Docker Client
      ↓
Docker Daemon
      ↓
Docker Images
      ↓
Docker Containers
      ↓
Docker Registry (Docker Hub)
```

---

# Task 2 – Install Docker

## Install Docker

Ubuntu example:

```bash
sudo apt update
sudo apt install docker.io -y
```

Start Docker service:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

## Verify Installation

```bash
docker --version
```

Example output:

```
Docker version 24.x.x
```

---

## Run Hello World Container

```bash
sudo docker run hello-world
```

Output explains:

- Docker pulls the image from Docker Hub
- Creates a container
- Runs it
- Displays a success message

This confirms Docker is installed correctly.

---

# Task 3 – Run Real Containers

## Run Nginx Container

```bash
docker run -d -p 8080:80 nginx
```

Explanation:

| Flag | Meaning |
|-----|--------|
| -d | Run container in background |
| -p | Map host port to container port |

Open browser:

```
http://localhost:8080
```

You should see the **Nginx welcome page**.

---

## Run Ubuntu Container (Interactive Mode)

```bash
docker run -it ubuntu
```

Explanation:

| Flag | Meaning |
|-----|--------|
| -i | Interactive |
| -t | Terminal |

You can now run Linux commands inside the container.

Example:

```bash
ls
pwd
apt update
```

Exit container:

```
exit
```

---

## List Running Containers

```bash
docker ps
```

---

## List All Containers

```bash
docker ps -a
```

---

## Stop a Container

```bash
docker stop <container-id>
```

---

## Remove a Container

```bash
docker rm <container-id>
```

---

# Task 4 – Explore Docker

## Run Container in Detached Mode

```bash
docker run -d nginx
```

Detached mode runs the container **in the background**.

---

## Give Container a Custom Name

```bash
docker run -d --name my-nginx nginx
```

---

## Port Mapping

```bash
docker run -d -p 8081:80 nginx
```

Host port **8081** maps to container port **80**.

---

## Check Container Logs

```bash
docker logs <container-id>
```

Example:

```
docker logs my-nginx
```

---

## Execute Command in Running Container

```bash
docker exec -it my-nginx bash
```

This allows you to run commands inside a running container.

---

# Commands Used

```
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

1. Docker containers package applications with their dependencies to ensure consistent environments.
2. Containers are lightweight compared to virtual machines because they share the host OS kernel.
3. Docker commands allow us to easily run, stop, inspect, and manage containers.

---

# Screenshots

Add screenshots for:

```
docker-hello-world.png
docker-nginx-running.png
docker-ps-output.png
```

---

# Summary

Today I installed Docker, ran my first container, and explored container management commands. Docker is a fundamental technology in DevOps because it allows developers to package and deploy applications consistently across different environments.
