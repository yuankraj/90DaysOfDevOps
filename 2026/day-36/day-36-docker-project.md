# Day 36 – Docker Project: Dockerize a Full Application

## Task Summary
Today I Dockerized a complete application including the app service and database using **Dockerfile and Docker Compose**.  
The goal was to simulate a real-world DevOps workflow: containerizing the app, managing dependencies, and deploying it using Docker.

---

# Task 1 – Application Chosen

I chose a **Python Flask web application with a PostgreSQL database**.

### Why This App?
- Simple to build and understand
- Demonstrates real backend architecture
- Common stack used in DevOps learning

---

# Project Structure

```
flask-docker-app/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── docker-compose.yml
├── .env
├── .dockerignore
└── README.md
```

---

# Flask Application Code

## app/app.py

```python
from flask import Flask
import os

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from my Dockerized Flask App!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## app/requirements.txt

```
flask
psycopg2-binary
```

---

# Task 2 – Dockerfile

## app/Dockerfile

```Dockerfile
# Base image
FROM python:3.10-alpine

# Set working directory
WORKDIR /app

# Copy requirements
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Create non-root user
RUN adduser -D appuser
USER appuser

# Expose application port
EXPOSE 5000

# Start application
CMD ["python", "app.py"]
```

---

## Explanation

| Instruction | Purpose |
|-------------|--------|
| FROM | Base Python runtime image |
| WORKDIR | Sets working directory |
| COPY | Copies app files |
| RUN | Installs dependencies |
| USER | Runs container as non-root |
| EXPOSE | Documents port |
| CMD | Default startup command |

---

# .dockerignore

```
.git
.env
*.md
__pycache__
```

This keeps the Docker image smaller.

---

# Task 3 – Docker Compose Setup

## docker-compose.yml

```yaml
version: "3.9"

services:

  web:
    build: ./app
    ports:
      - "5000:5000"
    environment:
      DB_HOST: db
    depends_on:
      db:
        condition: service_healthy
    networks:
      - app-network

  db:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_USER: devops
      POSTGRES_PASSWORD: devops
      POSTGRES_DB: devopsdb
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U devops"]
      interval: 10s
      timeout: 5s
      retries: 5

networks:
  app-network:

volumes:
  db_data:
```

---

# Environment Variables

## .env

```
POSTGRES_USER=devops
POSTGRES_PASSWORD=devops
POSTGRES_DB=devopsdb
```

---

# Run the Application

Start services:

```bash
docker compose up -d
```

Access application:

```
http://localhost:5000
```

---

# Task 4 – Push Image to Docker Hub

## Build Image

```bash
docker build -t yourusername/flask-devops:v1 ./app
```

---

## Login

```bash
docker login
```

---

## Push Image

```bash
docker push yourusername/flask-devops:v1
```

---

# Docker Hub Repository

Example:

```
https://hub.docker.com/r/yourusername/flask-devops
```

---

# Task 5 – Test Full Deployment

Remove everything locally:

```bash
docker compose down
docker system prune -a
```

Pull image again and run:

```bash
docker compose up
```

Result:

Application runs successfully using only the compose file.

---

# Challenges Faced

### 1. Database Startup Timing

The web container tried connecting to the database before it was ready.

**Solution:** Added a healthcheck and `depends_on` condition.

---

### 2. Image Size Optimization

Using `python:3.10` created a large image.

**Solution:** Switched to `python:3.10-alpine`.

---

# Final Image Size

Example:

```
flask-devops:v1 → ~120MB
```

---

# Commands Used

```
docker build
docker run
docker compose up
docker compose down
docker login
docker push
docker system prune
```

---

# What I Learned

1. Dockerizing an application requires packaging code, dependencies, and runtime environment.
2. Docker Compose simplifies running multi-container apps.
3. Healthchecks and volumes are critical for reliable production deployments.

---

# Screenshots

Add screenshots for:

```
flask-app-browser.png
docker-compose-ps.png
docker-hub-repo.png
```

---

# Summary

Today I Dockerized a full web application stack with Flask and PostgreSQL. I created Dockerfiles, managed services using Docker Compose, and published the container image to Docker Hub. This workflow reflects how real applications are containerized and deployed in modern DevOps environments.
