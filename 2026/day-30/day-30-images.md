# Day 30 – Docker Images & Container Lifecycle

## Task Summary
Today I explored how **Docker images and containers work internally**.  
I learned about image layers, how containers move through different lifecycle states, and how to manage running containers.

---

# Task 1 – Docker Images

## Pull Images from Docker Hub

```bash
docker pull nginx
docker pull ubuntu
docker pull alpine
```

---

## List All Images

```bash
docker images
```

Example Output

```
REPOSITORY   TAG       IMAGE ID       SIZE
nginx        latest    abcd1234       188MB
ubuntu       latest    efgh5678       77MB
alpine       latest    ijkl9012       7MB
```

---

## Compare Ubuntu vs Alpine

| Image | Size |
|------|------|
| Ubuntu | ~70MB |
| Alpine | ~7MB |

### Why Alpine is Smaller

- Alpine uses a minimal Linux distribution
- Uses **musl libc** instead of glibc
- Includes fewer packages by default

Because of this, Alpine is widely used for lightweight containers.

---

## Inspect an Image

```bash
docker inspect nginx
```

Information available includes:

- Image ID
- Creation date
- Architecture
- Environment variables
- Layers
- Entrypoint commands

---

## Remove an Image

```bash
docker rmi alpine
```

---

# Task 2 – Image Layers

Run:

```bash
docker image history nginx
```

Example Output

```
IMAGE          CREATED         CREATED BY        SIZE
abcd1234       2 weeks ago     /bin/sh -c ...    0B
abcd5678       2 weeks ago     /bin/sh -c ...    23MB
abcd9012       2 weeks ago     /bin/sh -c ...    50MB
```

---

## What Are Docker Image Layers?

Docker images are built in **layers**.

Each instruction in a Dockerfile creates a new layer.

Example:

```
FROM ubuntu
RUN apt update
RUN apt install nginx
COPY index.html
```

This creates multiple layers stacked on top of each other.

---

## Why Docker Uses Layers

Benefits:

- **Caching** speeds up builds
- **Reuse** layers across images
- **Efficient storage**
- **Faster downloads**

Only changed layers are rebuilt.

---

# Task 3 – Container Lifecycle

## Create Container Without Starting

```bash
docker create nginx
```

---

## Start Container

```bash
docker start <container-id>
```

---

## Pause Container

```bash
docker pause <container-id>
```

---

## Unpause Container

```bash
docker unpause <container-id>
```

---

## Stop Container

```bash
docker stop <container-id>
```

---

## Restart Container

```bash
docker restart <container-id>
```

---

## Kill Container

```bash
docker kill <container-id>
```

---

## Remove Container

```bash
docker rm <container-id>
```

---

## Check Container States

```bash
docker ps -a
```

Container states include:

| State | Description |
|------|-------------|
| created | Container created but not started |
| running | Container actively running |
| paused | Container execution paused |
| stopped | Container stopped |
| exited | Container finished execution |

---

# Task 4 – Working with Running Containers

## Run Nginx Container

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

---

## View Logs

```bash
docker logs my-nginx
```

---

## View Real-Time Logs

```bash
docker logs -f my-nginx
```

---

## Exec Into Container

```bash
docker exec -it my-nginx bash
```

Explore filesystem:

```bash
ls
pwd
```

Exit container shell:

```
exit
```

---

## Run Command Inside Container

```bash
docker exec my-nginx ls /
```

---

## Inspect Container

```bash
docker inspect my-nginx
```

Information available:

- Container IP address
- Port mappings
- Network configuration
- Mounted volumes

Example IP lookup:

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my-nginx
```

---

# Task 5 – Cleanup

## Stop All Running Containers

```bash
docker stop $(docker ps -q)
```

---

## Remove All Stopped Containers

```bash
docker container prune
```

---

## Remove Unused Images

```bash
docker image prune
```

---

## Check Docker Disk Usage

```bash
docker system df
```

Example Output

```
TYPE            TOTAL     ACTIVE    SIZE
Images          5         2         350MB
Containers      3         1         10MB
```

---

# Commands Used

```
docker pull
docker images
docker inspect
docker rmi
docker image history
docker create
docker start
docker pause
docker unpause
docker stop
docker restart
docker kill
docker rm
docker ps
docker logs
docker exec
docker container prune
docker image prune
docker system df
```

---

# What I Learned

1. Docker images are built using **layers**, which makes builds faster and more efficient.
2. Containers go through multiple lifecycle states such as created, running, paused, and stopped.
3. Docker provides many commands to inspect, monitor, and manage containers.

---

# Screenshots

Add screenshots for:

```
docker-images-list.png
docker-history-nginx.png
docker-container-states.png
docker-nginx-running.png
```

---

# Summary

Today I learned how Docker images are structured using layers and how containers move through their lifecycle. Understanding images, layers, and container states is essential for managing containerized applications in DevOps environments.
