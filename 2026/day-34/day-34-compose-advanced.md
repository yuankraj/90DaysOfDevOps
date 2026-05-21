# Day 34 – Docker Compose: Real-World Multi-Container Apps

# Objective

Today I built a production-like multi-container application using Docker Compose.

I practiced:
- Running a 3-service application stack
- Building custom Docker images using Dockerfiles
- Using databases and Redis cache
- Healthchecks & service dependencies
- Restart policies
- Explicit networks & named volumes
- Scaling containers

This challenge helped me understand how real DevOps teams manage multi-service applications.

---

# Project Structure

```bash
compose-advanced/
│
├── docker-compose.yml
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
└── .env
```

---

# Task 1 – Build 3-Service Stack

# Web App (Flask)

## app.py
```python
from flask import Flask
import redis
import psycopg2

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Flask + Postgres + Redis!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

# requirements.txt

```txt
flask
redis
psycopg2-binary
```

---

# Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

---

# docker-compose.yml

```yaml
services:

  web:
    build: ./app

    ports:
      - "5000:5000"

    depends_on:
      db:
        condition: service_healthy

    restart: on-failure

    networks:
      - app-network

    labels:
      project: "90days-devops"
      service: "flask-app"

  db:
    image: postgres

    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: appdb

    restart: always

    volumes:
      - postgres-data:/var/lib/postgresql/data

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

    networks:
      - app-network

    labels:
      service: "postgres-db"

  redis:
    image: redis

    restart: always

    networks:
      - app-network

    labels:
      service: "redis-cache"

volumes:
  postgres-data:

networks:
  app-network:
```

---

# Start Application Stack

## Input
```bash
docker compose up -d
```

### Observation
- Flask app started successfully.
- Postgres initialized correctly.
- Redis cache running successfully.

---

# Access Flask Application

```bash
http://localhost:5000
```

### Result
✅ Flask application accessible successfully.

---

# Task 2 – depends_on & Healthchecks

# Healthcheck Added

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U postgres"]
```

---

# Why Healthchecks Matter?

Without healthchecks:
- App may start before database is ready
- Causes connection failures

---

# depends_on with condition

```yaml
depends_on:
  db:
    condition: service_healthy
```

---

# Observation

✅ Web app waited for Postgres healthcheck before starting.

---

# Task 3 – Restart Policies

# restart: always

```yaml
restart: always
```

### Behavior
- Container restarts automatically even after manual stop or reboot.

---

# restart: on-failure

```yaml
restart: on-failure
```

### Behavior
- Container restarts only if application exits with error.

---

# Test Restart Policy

## Kill Database Container

```bash
docker kill compose-advanced-db-1
```

### Observation
✅ Database container restarted automatically.

---

# When to Use Restart Policies?

| Policy | Use Case |
|--------|-----------|
| always | Databases, production services |
| on-failure | Application crash recovery |
| no | Temporary/debug containers |

---

# Task 4 – Custom Dockerfiles in Compose

# Build from Dockerfile

```yaml
build: ./app
```

---

# Make Code Change

## Update app.py
```python
return "Updated Flask Application!"
```

---

# Rebuild Application

## Input
```bash
docker compose up --build
```

### Observation
✅ Docker rebuilt image and restarted application.

---

# Task 5 – Named Networks & Volumes

# Explicit Network

```yaml
networks:
  app-network:
```

---

# Named Volume

```yaml
volumes:
  postgres-data:
```

---

# Why Named Volumes?

Benefits:
- Persistent database storage
- Safer container deletion
- Easier backups

---

# Labels Added

```yaml
labels:
  project: "90days-devops"
```

---

# Why Labels Matter?

Labels help:
- Organize containers
- Filter services
- Add metadata
- Improve monitoring

---

# Task 6 – Scaling

# Scale Web App

## Input
```bash
docker compose up --scale web=3
```

---

# Observation

⚠️ Scaling issue occurred with port mapping.

---

# Why Did Scaling Break?

Because:
```yaml
ports:
  - "5000:5000"
```

Only one container can bind to same host port.

---

# Real-World Solution

Use:
- Load balancer
- Reverse proxy (Nginx)
- Kubernetes services

---

# Important Docker Compose Concepts Learned

| Concept | Purpose |
|----------|----------|
| depends_on | Service startup order |
| healthcheck | Verify service readiness |
| restart policy | Automatic recovery |
| build | Build custom images |
| networks | Container communication |
| volumes | Persistent storage |
| labels | Service metadata |

---

# Commands Used

```bash
docker compose up
docker compose up -d
docker compose up --build
docker compose down
docker compose ps
docker compose logs
docker kill
docker inspect
```

---

# What I Learned

- Multi-container applications require orchestration.
- Healthchecks improve application reliability.
- Restart policies help recover failed services.
- Named volumes persist database data safely.
- Scaling containers needs load balancing.

---

# Why This Matters in DevOps

These concepts are critical because:
- Production applications use multiple services
- Containers depend on databases & caches
- Reliability requires healthchecks & restart policies
- Scaling is essential for high availability
- Docker Compose prepares you for Kubernetes

---

# Final Summary

Today I practiced:
- Building a real-world 3-service stack
- Flask + Postgres + Redis integration
- Docker Compose orchestration
- Healthchecks & dependencies
- Restart policies
- Named networks & volumes
- Scaling containers

This challenge gave me a much deeper understanding of production-style Docker Compose deployments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
