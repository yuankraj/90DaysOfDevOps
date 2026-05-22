# Day 37 – Docker Revision & Cheat Sheet

# Goal

Today was a Docker revision day focused on consolidating everything from Days 29–36.

I revised:
- Docker fundamentals
- Images & containers
- Dockerfiles
- Volumes & networking
- Docker Compose
- Multi-stage builds
- Docker Hub
- Full application Dockerization

This helped strengthen my understanding of real-world Docker workflows.

---

# Self-Assessment Checklist

| Skill | Status |
|-------|---------|
| Run containers from Docker Hub | ✅ Can Do |
| List, stop, remove containers/images | ✅ Can Do |
| Explain image layers & caching | ✅ Can Do |
| Write Dockerfiles from scratch | ✅ Can Do |
| Explain CMD vs ENTRYPOINT | ⚠️ Slightly Shaky |
| Build & tag custom images | ✅ Can Do |
| Create named volumes | ✅ Can Do |
| Use bind mounts | ✅ Can Do |
| Create custom networks | ✅ Can Do |
| Write multi-container Compose apps | ✅ Can Do |
| Use .env variables in Compose | ✅ Can Do |
| Write multi-stage Dockerfiles | ✅ Can Do |
| Push images to Docker Hub | ✅ Can Do |
| Use healthchecks & depends_on | ⚠️ Need More Practice |

---

# Quick-Fire Questions

## 1. Difference between Image and Container?

- Image = Blueprint/template
- Container = Running instance of an image

---

## 2. What happens to container data after removal?

Container data is lost unless:
- Named volumes
- Bind mounts
are used.

---

## 3. How do containers communicate on same network?

Containers communicate using:
- Container names
- Internal Docker DNS

Example:
```bash
ping postgres-db
```

---

## 4. What does `docker compose down -v` do?

It removes:
- Containers
- Networks
- Volumes

Unlike normal `docker compose down`.

---

## 5. Why are multi-stage builds useful?

Benefits:
- Smaller image size
- Better security
- Faster deployment
- Removes unnecessary build tools

---

## 6. Difference between COPY and ADD?

| COPY | ADD |
|------|------|
| Simple file copy | Extra features |
| Recommended | Can extract tar files & download URLs |

---

## 7. Meaning of `-p 8080:80`

```bash
HostPort:ContainerPort
```

Traffic from:
```bash
localhost:8080
```

goes to:
```bash
container:80
```

---

## 8. Check Docker Disk Usage

```bash
docker system df
```

---

# Weak Areas Revisited

## 1. CMD vs ENTRYPOINT

### CMD
- Default command
- Can be overridden

### ENTRYPOINT
- Fixed executable
- Additional arguments appended

---

## 2. Healthchecks & depends_on

I revised:
```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U postgres"]
```

and:

```yaml
depends_on:
  db:
    condition: service_healthy
```

This ensures services start only when dependencies are truly healthy.

---

# What I Re-Learned

- Docker networking uses internal DNS.
- Volumes are critical for persistence.
- Multi-stage builds dramatically reduce image size.
- Compose simplifies multi-container management.
- Healthchecks improve reliability in production.

---

# Important Docker Concepts Revised

| Concept | Key Idea |
|---------|-----------|
| Images | Read-only templates |
| Containers | Running image instances |
| Volumes | Persistent storage |
| Networks | Container communication |
| Compose | Multi-container orchestration |
| Docker Hub | Image registry |
| Multi-stage | Optimized builds |

---

# Docker Cheat Sheet

# Container Commands

| Command | Purpose |
|---------|----------|
| docker run | Run container |
| docker ps | List running containers |
| docker ps -a | List all containers |
| docker stop | Stop container |
| docker rm | Remove container |
| docker exec -it | Enter running container |
| docker logs | View logs |

---

# Image Commands

| Command | Purpose |
|---------|----------|
| docker build | Build image |
| docker pull | Download image |
| docker push | Upload image |
| docker images | List images |
| docker rmi | Remove image |
| docker tag | Tag image |

---

# Volume Commands

| Command | Purpose |
|---------|----------|
| docker volume create | Create volume |
| docker volume ls | List volumes |
| docker volume inspect | Inspect volume |
| docker volume rm | Remove volume |

---

# Network Commands

| Command | Purpose |
|---------|----------|
| docker network create | Create network |
| docker network ls | List networks |
| docker network inspect | Inspect network |
| docker network connect | Connect container |

---

# Docker Compose Commands

| Command | Purpose |
|---------|----------|
| docker compose up | Start services |
| docker compose up -d | Detached mode |
| docker compose down | Stop/remove services |
| docker compose ps | List compose containers |
| docker compose logs | View logs |
| docker compose build | Build services |

---

# Cleanup Commands

| Command | Purpose |
|---------|----------|
| docker system prune | Remove unused resources |
| docker system df | Check disk usage |

---

# Dockerfile Instructions

| Instruction | Purpose |
|-------------|----------|
| FROM | Base image |
| RUN | Execute build commands |
| COPY | Copy files |
| WORKDIR | Set working directory |
| EXPOSE | Document port |
| CMD | Default command |
| ENTRYPOINT | Main executable |

---

# Commands Practiced Today

```bash
docker ps
docker images
docker volume ls
docker network ls
docker compose up -d
docker compose down -v
docker system df
docker system prune
```

---

# Final Reflection

This revision day helped reinforce:
- Docker fundamentals
- Compose workflows
- Networking & storage concepts
- Production-ready Docker practices

I feel much more confident working with containers now 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
