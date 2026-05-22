# Day 37 – Docker Revision

# Goal

Today was a revision day focused on revisiting Docker concepts from Days 29–36 and strengthening my understanding through self-assessment and quick revision tasks.

---

# Self-Assessment Checklist

| Topic | Status |
|--------|---------|
| Run a container from Docker Hub | ✅ Can Do |
| List, stop, remove containers and images | ✅ Can Do |
| Explain image layers and caching | ✅ Can Do |
| Write a Dockerfile from scratch | ✅ Can Do |
| Explain CMD vs ENTRYPOINT | ⚠️ Slightly Shaky |
| Build and tag custom images | ✅ Can Do |
| Create and use named volumes | ✅ Can Do |
| Use bind mounts | ✅ Can Do |
| Create custom networks | ✅ Can Do |
| Write a docker-compose.yml | ✅ Can Do |
| Use environment variables in Compose | ✅ Can Do |
| Write a multi-stage Dockerfile | ✅ Can Do |
| Push images to Docker Hub | ✅ Can Do |
| Use healthchecks and depends_on | ⚠️ Need More Practice |

---

# Quick-Fire Questions & Answers

## 1. What is the difference between an image and a container?

- Docker Image = Blueprint/template
- Docker Container = Running instance of an image

Example:
```bash
docker run nginx
```

Uses the nginx image to create a container.

---

## 2. What happens to data inside a container when you remove it?

Container data is deleted when the container is removed unless:
- Named volumes
- Bind mounts

are used for persistence.

---

## 3. How do two containers on the same custom network communicate?

Containers communicate using:
- Container names
- Docker internal DNS

Example:
```bash
ping postgres-db
```

---

## 4. What does `docker compose down -v` do differently?

```bash
docker compose down
```
Removes:
- Containers
- Networks

But:

```bash
docker compose down -v
```

Also removes:
- Volumes

which deletes persistent data.

---

## 5. Why are multi-stage builds useful?

Benefits:
- Smaller image size
- Better security
- Faster deployment
- Removes unnecessary dependencies/tools

---

## 6. What is the difference between COPY and ADD?

| COPY | ADD |
|------|------|
| Simple file copy | Extra features |
| Recommended for most cases | Can extract tar files & download URLs |

---

## 7. What does `-p 8080:80` mean?

```bash
-p hostPort:containerPort
```

Example:
```bash
-p 8080:80
```

Traffic from:
```bash
localhost:8080
```

is forwarded to:
```bash
container:80
```

---

## 8. How do you check Docker disk usage?

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
- Main executable
- Additional arguments appended

---

## 2. Healthchecks & depends_on

Revised:
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

This ensures services start only when dependencies become healthy.

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

# What I Re-Learned

- Docker networking uses internal DNS.
- Volumes are essential for persistent storage.
- Multi-stage builds optimize image size.
- Docker Compose simplifies multi-container orchestration.
- Healthchecks improve reliability in production environments.

---

# Final Reflection

This revision day helped reinforce my understanding of:
- Docker fundamentals
- Compose workflows
- Volumes & networking
- Multi-stage builds
- Production-ready container practices

I now feel much more confident working with Docker and containerized applications 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
