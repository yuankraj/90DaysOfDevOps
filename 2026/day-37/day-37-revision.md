# Day 37 – Docker Revision

## Task Summary
Today was a revision day focused on reviewing all Docker topics learned from Day 29 to Day 36. The goal was to reinforce container fundamentals, Dockerfiles, volumes, networking, Docker Compose, and image optimization.

---

# Self-Assessment Checklist

| Topic | Status |
|------|------|
Run containers (interactive & detached) | Can do confidently |
List, stop, remove containers/images | Can do confidently |
Explain image layers & caching | Can do confidently |
Write Dockerfile from scratch | Can do confidently |
CMD vs ENTRYPOINT | Shaky |
Build and tag images | Can do confidently |
Named volumes | Can do confidently |
Bind mounts | Can do confidently |
Custom networks | Can do confidently |
Docker Compose multi-container apps | Can do confidently |
Environment variables in Compose | Can do confidently |
Multi-stage builds | Can do confidently |
Push images to Docker Hub | Can do confidently |
Healthchecks & depends_on | Shaky |

---

# Quick-Fire Questions

### 1. Difference between an image and a container

An **image** is a blueprint used to create containers.  
A **container** is a running instance of an image.

---

### 2. What happens to data inside a container when you remove it?

Container data is lost unless it is stored in a **volume or bind mount**.

---

### 3. How do two containers on the same custom network communicate?

They communicate using **container names as DNS hostnames**.

Example:

```
docker exec container1 ping container2
```

---

### 4. Difference between `docker compose down` and `docker compose down -v`

| Command | Behavior |
|-------|----------|
docker compose down | Stops and removes containers and networks |
docker compose down -v | Also removes volumes |

---

### 5. Why are multi-stage builds useful?

They reduce image size by separating the **build stage** from the **runtime stage** and copying only the final artifacts.

---

### 6. Difference between COPY and ADD

| Instruction | Behavior |
|------------|---------|
COPY | Copies files into container |
ADD | Copy + extract archives or download URLs |

COPY is generally recommended.

---

### 7. What does `-p 8080:80` mean?

Maps **host port 8080** to **container port 80**.

Access service:

```
http://localhost:8080
```

---

### 8. How to check Docker disk usage

```
docker system df
```

---

# Weak Areas Revisited

### 1. CMD vs ENTRYPOINT

CMD provides a default command that can be overridden.

ENTRYPOINT defines the main command that always runs.

Example:

```
CMD ["echo","hello"]
```

can be overridden.

ENTRYPOINT example:

```
ENTRYPOINT ["echo"]
```

extra arguments get appended.

---

### 2. Healthchecks in Docker Compose

Healthchecks ensure services are ready before dependent services start.

Example:

```
healthcheck:
  test: ["CMD", "pg_isready"]
  interval: 10s
  retries: 5
```

---

# Key Takeaways

1. Docker containers are ephemeral and require volumes for persistent storage.
2. Docker Compose simplifies running multi-service applications.
3. Multi-stage builds improve image performance and reduce size.

---

# Summary

This revision helped reinforce Docker fundamentals including container management, Dockerfiles, volumes, networking, and multi-container orchestration with Docker Compose. Revisiting these topics strengthened my understanding of how containerized applications are built and deployed in DevOps environments.
