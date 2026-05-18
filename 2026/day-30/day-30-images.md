# Day 30 – Docker Images & Container Lifecycle

# Objective

Today I learned how Docker images and containers actually work behind the scenes.

I practiced:
- Pulling and managing Docker images
- Understanding image layers
- Exploring the full container lifecycle
- Inspecting running containers
- Cleaning up Docker resources

This challenge helped me understand the core workflow behind container-based deployments.

---

# Task 1 – Docker Images

# Pull Docker Images

## Input
```bash
docker pull nginx
docker pull ubuntu
docker pull alpine
```

### Observation
- Images downloaded successfully from Docker Hub.

---

# List Docker Images

## Input
```bash
docker images
```

## Output
```bash
REPOSITORY   TAG       IMAGE ID       SIZE
nginx        latest    xxxxxxxx       188MB
ubuntu       latest    xxxxxxxx       77MB
alpine       latest    xxxxxxxx       7MB
```

---

# Compare Ubuntu vs Alpine

| Ubuntu | Alpine |
|---------|---------|
| Larger image | Very lightweight |
| More packages | Minimal packages |
| Good for full environments | Best for lightweight containers |

---

# Why Alpine is Smaller?

Because Alpine Linux:
- Uses minimal packages
- Lightweight musl libc
- BusyBox utilities
- Designed specifically for containers

---

# Inspect Docker Image

## Input
```bash
docker image inspect nginx
```

### Information Observed
- Image ID
- Architecture
- Environment variables
- Entrypoint
- Layers

---

# Remove Unused Image

## Input
```bash
docker rmi ubuntu
```

### Observation
- Image removed successfully.

---

# Task 2 – Docker Image Layers

# View Image History

## Input
```bash
docker image history nginx
```

## Observation
- Multiple layers displayed.
- Some layers showed actual sizes.
- Some layers showed 0B.

---

# What are Docker Layers?

Docker images are built in layers.

Each layer represents:
- A filesystem change
- Installed package
- Command execution

---

# Why Docker Uses Layers

Benefits:
- Faster builds
- Layer caching
- Smaller downloads
- Efficient image reuse

---

# Example

```bash
Base OS Layer
→ Install Packages
→ Copy Files
→ Start Application
```

Each step creates a reusable layer.

---

# Task 3 – Container Lifecycle

# Create Container Without Starting

## Input
```bash
docker create --name demo-nginx nginx
```

---

# Check Status

## Input
```bash
docker ps -a
```

### Observation
```bash
Status: Created
```

---

# Start Container

## Input
```bash
docker start demo-nginx
```

### Observation
```bash
Status: Up
```

---

# Pause Container

## Input
```bash
docker pause demo-nginx
```

### Observation
```bash
Status: Paused
```

---

# Unpause Container

## Input
```bash
docker unpause demo-nginx
```

---

# Stop Container

## Input
```bash
docker stop demo-nginx
```

### Observation
```bash
Status: Exited
```

---

# Restart Container

## Input
```bash
docker restart demo-nginx
```

---

# Kill Container

## Input
```bash
docker kill demo-nginx
```

### Observation
- Container forcefully stopped.

---

# Remove Container

## Input
```bash
docker rm demo-nginx
```

---

# Container Lifecycle Flow

```bash
Create → Start → Pause → Unpause → Stop → Restart → Kill → Remove
```

---

# Task 4 – Working with Running Containers

# Run Nginx Container

## Input
```bash
docker run -d -p 8080:80 --name my-nginx nginx
```

---

# View Logs

## Input
```bash
docker logs my-nginx
```

---

# Real-Time Logs

## Input
```bash
docker logs -f my-nginx
```

### Observation
- Live logs displayed continuously.

---

# Exec into Running Container

## Input
```bash
docker exec -it my-nginx bash
```

---

# Explore Filesystem

## Commands Used
```bash
ls
pwd
cd /usr/share/nginx/html
```

---

# Run Single Command Without Entering

## Input
```bash
docker exec my-nginx ls /etc
```

---

# Inspect Container

## Input
```bash
docker inspect my-nginx
```

### Information Found
- Container IP address
- Port mappings
- Mount points
- Network configuration

---

# Task 5 – Docker Cleanup

# Stop All Running Containers

## Input
```bash
docker stop $(docker ps -q)
```

---

# Remove All Stopped Containers

## Input
```bash
docker container prune
```

---

# Remove Unused Images

## Input
```bash
docker image prune
```

---

# Check Docker Disk Usage

## Input
```bash
docker system df
```

### Observation
- Displayed space used by:
  - Images
  - Containers
  - Volumes
  - Build cache

---

# Important Commands Learned

| Command | Purpose |
|----------|----------|
| docker pull | Download image |
| docker images | List images |
| docker image history | View image layers |
| docker create | Create container |
| docker start | Start container |
| docker pause | Pause container |
| docker stop | Stop container |
| docker restart | Restart container |
| docker kill | Force stop container |
| docker rm | Remove container |
| docker inspect | Inspect details |
| docker logs | View logs |
| docker exec | Run commands inside container |

---

# Commands Used

```bash
docker pull
docker images
docker image history
docker create
docker start
docker pause
docker unpause
docker stop
docker restart
docker kill
docker rm
docker logs
docker exec
docker inspect
docker system df
```

---

# What I Learned

- Docker images are made of reusable layers.
- Containers move through different lifecycle states.
- Alpine images are lightweight and optimized.
- Docker logs help troubleshoot applications.
- Cleanup commands are important to save disk space.

---

# Why This Matters in DevOps

Docker images and container lifecycle management are critical because:
- CI/CD pipelines build and deploy images
- Kubernetes manages container lifecycle
- Image optimization improves deployment speed
- Logs and inspection help in troubleshooting production issues

---

# Final Summary

Today I practiced:
- Managing Docker images
- Understanding image layers
- Exploring container lifecycle states
- Working inside running containers
- Docker cleanup and optimization

This challenge gave me a deeper understanding of how Docker works internally and how containers are managed in real DevOps environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
