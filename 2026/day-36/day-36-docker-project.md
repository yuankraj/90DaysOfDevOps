# Day 36 – Docker Project: Dockerize a Full Application

# Objective

Today I Dockerized a complete real-world Flask application with PostgreSQL using Docker Compose.

This challenge combined everything I learned in Docker so far:
- Dockerfiles
- Multi-stage builds
- Docker Compose
- Volumes
- Networks
- Environment variables
- Healthchecks
- Docker Hub deployment

This felt like a real DevOps project 🚀

---

# Project Chosen

## Flask + PostgreSQL Application

### Why I Chose This Project?

I selected a Flask app because:
- Python is one of my current learning focuses
- Flask is lightweight and beginner-friendly
- Real-world applications commonly use Flask with databases
- Good practice for containerizing backend applications

---

# Project Structure

```bash
dockerized-flask-app/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   ├── Dockerfile
│   └── .dockerignore
│
├── docker-compose.yml
├── .env
└── README.md
```

---

# Task 1 – Application Code

# app.py

```python
from flask import Flask
import psycopg2
import os

app = Flask(__name__)

@app.route("/")
def home():

    db_host = os.getenv("DB_HOST")

    conn = psycopg2.connect(
        host=db_host,
        database="appdb",
        user="postgres",
        password="password"
    )

    cur = conn.cursor()
    cur.execute("SELECT version();")

    db_version = cur.fetchone()

    cur.close()
    conn.close()

    return f"""
    <h1>Dockerized Flask App 🚀</h1>
    <p>Connected to PostgreSQL ✅</p>
    <p>{db_version}</p>
    """

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

# requirements.txt

```txt
flask
psycopg2-binary
gunicorn
```

---

# Task 2 – Dockerfile

# Multi-Stage Dockerfile

## Dockerfile

```dockerfile
# Stage 1 - Builder
FROM python:3.11-slim AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Stage 2 - Production
FROM python:3.11-alpine

WORKDIR /app

COPY --from=builder /usr/local/lib/python3.11 /usr/local/lib/python3.11

COPY --from=builder /app /app

RUN adduser -D appuser

USER appuser

EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```

---

# Why This Dockerfile?

| Feature | Benefit |
|----------|----------|
| Multi-stage build | Smaller image |
| Alpine base image | Lightweight |
| Non-root user | Better security |
| Gunicorn | Production-ready server |

---

# .dockerignore

```bash
__pycache__
*.pyc
.env
.git
README.md
```

---

# Task 3 – Docker Compose

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

    environment:
      DB_HOST: db

    restart: always

    networks:
      - flask-network

  db:
    image: postgres:15-alpine

    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: appdb

    volumes:
      - postgres-data:/var/lib/postgresql/data

    restart: always

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

    networks:
      - flask-network

volumes:
  postgres-data:

networks:
  flask-network:
```

---

# .env File

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password
POSTGRES_DB=appdb
```

---

# Start Application

## Input
```bash
docker compose up -d --build
```

---

# Verify Containers

## Input
```bash
docker ps
```

### Observation
✅ Flask app running  
✅ PostgreSQL healthy  
✅ Network created successfully

---

# Access Application

```bash
http://localhost:5000
```

### Result
✅ Flask app successfully connected to PostgreSQL.

---

# Task 4 – Push to Docker Hub

# Build Image

## Input
```bash
docker build -t yuankraj/flask-app:v1 .
```

---

# Login to Docker Hub

## Input
```bash
docker login
```

---

# Push Image

## Input
```bash
docker push yuankraj/flask-app:v1
```

---

# Docker Hub Repository

```bash
https://hub.docker.com/r/yuankraj/flask-app
```

---

# README.md

## README Example

```markdown
# Dockerized Flask App

## Run with Docker Compose

docker compose up -d --build

## Stop Application

docker compose down
```

---

# Task 5 – Fresh Environment Test

# Remove Local Containers & Images

## Input
```bash
docker compose down

docker system prune -a
```

---

# Pull from Docker Hub

## Input
```bash
docker pull yuankraj/flask-app:v1
```

---

# Run Again

## Input
```bash
docker compose up -d
```

### Observation
✅ Application worked successfully from fresh setup.

---

# Challenges Faced

| Problem | Solution |
|----------|----------|
| Postgres unhealthy | Removed old volumes |
| Gunicorn missing | Added to requirements.txt |
| Flask app couldn't connect to DB | Fixed environment variables |
| Large image size | Used multi-stage build |

---

# Final Image Size

| Build Type | Size |
|------------|------|
| Normal Build | 1GB+ |
| Optimized Multi-Stage | ~90MB |

---

# Important Commands Used

```bash
docker build
docker compose up
docker compose down
docker ps
docker logs
docker push
docker pull
docker login
```

---

# What I Learned

- How to Dockerize a complete application
- Multi-stage builds drastically reduce image size
- Docker Compose simplifies multi-container setups
- Healthchecks improve reliability
- Non-root containers improve security
- Docker Hub distributes images globally

---

# Why This Matters in DevOps

This is real DevOps work because:
- Applications must run consistently everywhere
- Teams deploy containerized apps daily
- CI/CD pipelines build & ship Docker images automatically
- Kubernetes runs containerized applications in production

---

# Final Summary

Today I successfully:
✅ Dockerized a real Flask application  
✅ Connected it with PostgreSQL  
✅ Used Docker Compose orchestration  
✅ Created optimized multi-stage Docker images  
✅ Used non-root containers  
✅ Added healthchecks & persistent volumes  
✅ Pushed images to Docker Hub  
✅ Tested the full deployment workflow  

This challenge gave me real-world hands-on experience with production-style containerized application deployment 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
