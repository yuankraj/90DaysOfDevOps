# Docker Cheat Sheet

A quick reference for common Docker commands used in daily DevOps workflows.

---

# Container Commands

| Command | Description |
|-------|-------------|
docker run image | Run a container from an image |
docker run -it image | Run container interactively |
docker run -d image | Run container in background |
docker ps | List running containers |
docker ps -a | List all containers |
docker stop container | Stop a container |
docker start container | Start a stopped container |
docker restart container | Restart container |
docker rm container | Remove container |
docker exec -it container bash | Run command inside container |
docker logs container | View container logs |
docker logs -f container | Follow logs in real-time |

---

# Image Commands

| Command | Description |
|-------|-------------|
docker pull image | Download image from registry |
docker images | List local images |
docker build -t name:tag . | Build image from Dockerfile |
docker tag image newname:tag | Tag image |
docker push repo/image | Push image to Docker Hub |
docker rmi image | Remove image |

---

# Volume Commands

| Command | Description |
|-------|-------------|
docker volume create name | Create named volume |
docker volume ls | List volumes |
docker volume inspect name | Show volume details |
docker volume rm name | Remove volume |

Mount volume when running container:

```
docker run -v volume_name:/path
```

Bind mount:

```
docker run -v /host/path:/container/path
```

---

# Network Commands

| Command | Description |
|-------|-------------|
docker network ls | List networks |
docker network create name | Create network |
docker network inspect name | Inspect network |
docker network connect network container | Connect container to network |

---

# Docker Compose Commands

| Command | Description |
|-------|-------------|
docker compose up | Start services |
docker compose up -d | Start services in background |
docker compose down | Stop and remove services |
docker compose ps | Show running services |
docker compose logs | Show logs |
docker compose logs -f | Follow logs |
docker compose build | Build services |
docker compose up --build | Rebuild and start |

---

# Cleanup Commands

| Command | Description |
|-------|-------------|
docker container prune | Remove stopped containers |
docker image prune | Remove unused images |
docker volume prune | Remove unused volumes |
docker network prune | Remove unused networks |
docker system prune | Clean unused Docker data |
docker system df | Show Docker disk usage |

---

# Dockerfile Instructions

| Instruction | Purpose |
|-------------|--------|
FROM | Base image |
RUN | Execute command during build |
COPY | Copy files into image |
ADD | Copy files + extra features |
WORKDIR | Set working directory |
EXPOSE | Document port |
CMD | Default container command |
ENTRYPOINT | Main executable command |
ENV | Set environment variable |
USER | Set container user |

---

# Useful Flags

| Flag | Meaning |
|------|--------|
-it | Interactive terminal |
-d | Detached mode |
-p | Port mapping |
-v | Volume mount |
--name | Assign container name |

---

# Example Workflow

Build image

```
docker build -t myapp:v1 .
```

Run container

```
docker run -d -p 8080:80 myapp:v1
```

Push to Docker Hub

```
docker tag myapp:v1 username/myapp:v1
docker push username/myapp:v1
```

---

# Summary

Docker helps package applications and dependencies into containers that run consistently across environments. This cheat sheet provides quick access to commonly used commands in container management, networking, storage, and Compose orchestration.
