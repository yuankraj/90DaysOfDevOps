# Day 34 – Docker Compose: Real-World Multi-Container Apps

## Task Summary
Today I built a **production-like multi-container application** using Docker Compose.  
The stack includes:

- A **web application** (Python Flask)
- A **database** (PostgreSQL)
- A **cache** (Redis)

I also explored **healthchecks, restart policies, custom Dockerfiles, networks, and scaling**.

---

# Task 1 – Build a 3-Service App Stack

## Project Structure

```
app-stack/
│
├── docker-compose.yml
└── web/
    ├── Dockerfile
    ├── app.py
    └── requirements.txt
```

---

## Flask App (web/app.py)

```python
from flask import Flask
import redis
import os

app = Flask(__name__)

redis_host = os.getenv("REDIS_HOST", "redis")
cache = redis.Redis(host=redis_host, port=6379)

@app.route("/")
def hello():
    count = cache.incr("hits")
    return f"Hello DevOps! This page has been visited {count} times."

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## requirements.txt

```
flask
redis
```

---

## Dockerfile (web/Dockerfile)

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

# docker-compose.yml

```yaml
version: "3.9"

services:

  web:
    build: ./web
    ports:
      - "5000:5000"
    depends_on:
      db:
        condition: service_healthy
    environment:
      REDIS_HOST: redis
    networks:
      - app-network

  db:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_USER: devops
      POSTGRES_PASSWORD: devops
      POSTGRES_DB: appdb
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U devops"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:alpine
    networks:
      - app-network

networks:
  app-network:

volumes:
  db_data:
```

---

# Start the Application Stack

```bash
docker compose up -d
```

Access the application:

```
http://localhost:5000
```

The page shows a visit counter stored in Redis.

---

# Task 2 – depends_on & Healthchecks

### depends_on

The web service depends on the database:

```yaml
depends_on:
  db:
    condition: service_healthy
```

This ensures the web service only starts **after the database is ready**.

---

### Healthcheck Example

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U devops"]
  interval: 10s
  timeout: 5s
  retries: 5
```

This checks whether PostgreSQL is ready to accept connections.

---

# Task 3 – Restart Policies

### restart: always

```yaml
restart: always
```

Behavior:

- Container restarts automatically if it crashes
- Restarts on Docker daemon restart

---

### restart: on-failure

```yaml
restart: on-failure
```

Behavior:

- Container restarts only if it exits with an error code

---

## When to Use Each

| Policy | When to Use |
|------|-------------|
| always | Databases, critical services |
| on-failure | Batch jobs or workers |

---

# Task 4 – Custom Dockerfiles in Compose

Instead of pulling a pre-built image:

```yaml
web:
  build: ./web
```

Docker Compose builds the image from the Dockerfile automatically.

---

## Rebuild After Code Change

```bash
docker compose up --build
```

This rebuilds images and restarts services.

---

# Task 5 – Named Networks & Volumes

## Named Network

```yaml
networks:
  app-network:
```

All services communicate through this network.

---

## Named Volume

```yaml
volumes:
  db_data:
```

Used to store PostgreSQL data persistently.

---

## Why Volumes Matter

Without volumes, database data would be lost when containers are removed.

---

# Task 6 – Scaling (Bonus)

Scale web service:

```bash
docker compose up --scale web=3
```

---

## What Happens?

Docker creates multiple containers:

```
web_1
web_2
web_3
```

---

## Problem with Port Mapping

If the service exposes:

```
5000:5000
```

Only one container can use port 5000.

This breaks scaling.

---

## Why Scaling Works Better with Load Balancers

In real production systems:

```
Load Balancer
      │
 ┌────┴────┐
web1 web2 web3
```

Tools used:

- Nginx
- Traefik
- Kubernetes Services

---

# Commands Used

```
docker compose up
docker compose up -d
docker compose up --build
docker compose down
docker compose ps
docker compose logs
docker compose logs -f
docker compose up --scale web=3
```

---

# What I Learned

1. Docker Compose can run **multi-container application stacks** easily.
2. Healthchecks ensure services start only when dependencies are ready.
3. Volumes preserve database data even if containers are removed.

---

# Screenshots

Add screenshots for:

```
compose-app-running.png
compose-logs.png
compose-scale-test.png
flask-app-browser.png
```

---

# Summary

Today I built a real-world application stack using Docker Compose with a web service, database, and cache. I also learned how to manage service dependencies, restart policies, and container scaling. These concepts are widely used in production containerized applications.
