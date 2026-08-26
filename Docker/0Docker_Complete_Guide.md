# Docker: Complete Guide (Basic to Advanced) — Industry Level

## Table of Contents
1. [Introduction to Docker](#1-introduction-to-docker)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Docker Architecture](#4-docker-architecture)
5. [Basic Docker Commands](#5-basic-docker-commands)
6. [Working with Images](#6-working-with-images)
7. [Working with Containers](#7-working-with-containers)
8. [Dockerfile — Building Custom Images](#8-dockerfile--building-custom-images)
9. [Docker Volumes & Data Persistence](#9-docker-volumes--data-persistence)
10. [Docker Networking](#10-docker-networking)
11. [Docker Compose](#11-docker-compose)
12. [Multi-Stage Builds](#12-multi-stage-builds)
13. [Docker Registry & Docker Hub](#13-docker-registry--docker-hub)
14. [Environment Variables & Secrets](#14-environment-variables--secrets)
15. [Docker in CI/CD Pipelines](#15-docker-in-cicd-pipelines)
16. [Security Best Practices](#16-security-best-practices)
17. [Performance Optimization](#17-performance-optimization)
18. [Docker Swarm (Orchestration)](#18-docker-swarm-orchestration)
19. [Docker with Kubernetes](#19-docker-with-kubernetes)
20. [Monitoring & Logging](#20-monitoring--logging)
21. [Debugging & Troubleshooting](#21-debugging--troubleshooting)
22. [Industry Best Practices Checklist](#22-industry-best-practices-checklist)
23. [Interview Questions & Answers](#23-interview-questions--answers)
24. [Cheat Sheet](#24-cheat-sheet)

---

## 1. Introduction to Docker

### What is Docker?
Docker is an open-source platform used to **build, ship, and run applications inside containers**. A container packages an application together with all its dependencies (libraries, binaries, configuration files) so it runs reliably across any environment — developer laptop, testing server, or production cloud.

### Why Docker?
| Problem (Traditional Deployment) | Docker Solution |
|---|---|
| "Works on my machine" issue | Same container runs everywhere |
| Heavy Virtual Machines | Lightweight containers share OS kernel |
| Slow environment setup | Spin up environments in seconds |
| Dependency conflicts | Isolated environments per app |
| Difficult scaling | Easy to replicate containers |

### Containers vs Virtual Machines
```
Virtual Machine                     Container
┌─────────────┐                    ┌─────────────┐
│   App A      │                    │   App A      │
│  Bins/Libs   │                    │  Bins/Libs   │
│  Guest OS    │                    ├─────────────┤
├─────────────┤                    │ Docker Engine│
│  Hypervisor  │                    ├─────────────┤
│  Host OS     │                    │  Host OS     │
│  Hardware    │                    │  Hardware    │
└─────────────┘                    └─────────────┘
```
- VMs virtualize hardware (heavy, slow boot, GBs in size).
- Containers virtualize the OS (lightweight, fast boot, MBs in size).

---

## 2. Core Concepts

| Term | Meaning |
|---|---|
| **Image** | A read-only template with app code, runtime, libraries, and dependencies |
| **Container** | A running instance of an image |
| **Dockerfile** | A script of instructions to build an image |
| **Registry** | A storage/distribution system for images (e.g., Docker Hub, ECR, GCR) |
| **Volume** | Persistent storage mechanism for containers |
| **Network** | Communication layer between containers |
| **Docker Engine** | The core service (daemon) that builds and runs containers |
| **Docker Compose** | Tool to define and run multi-container applications |

---

## 3. Installation

### Linux (Ubuntu/Debian)
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Verify Installation
```bash
docker --version
docker run hello-world
```

### Windows / macOS
Install **Docker Desktop** from https://www.docker.com/products/docker-desktop

### Post-install (Linux) — run Docker without sudo
```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## 4. Docker Architecture

```
┌───────────────────────────────────────────────┐
│                Docker Client (CLI)              │
│         docker build / run / pull / push        │
└───────────────────────┬───────────────────────┘
                         │ REST API
┌───────────────────────▼───────────────────────┐
│              Docker Daemon (dockerd)             │
│  - Manages images, containers, networks, volumes │
└───────────────────────┬───────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
   Images Store      Containers      Docker Registry
                                     (Docker Hub, etc.)
```

**Components:**
- **Docker Client** — CLI/API interface used by the user.
- **Docker Daemon (dockerd)** — Background service managing containers.
- **containerd** — Manages container lifecycle at a lower level.
- **runc** — Actual low-level container runtime (OCI spec).
- **Docker Registry** — Stores and distributes images.

---

## 5. Basic Docker Commands

```bash
docker version                 # Show client/server version
docker info                    # System-wide information
docker help                    # List all commands

# Images
docker images                  # List local images
docker pull <image>            # Download image from registry
docker rmi <image>             # Remove image

# Containers
docker ps                      # List running containers
docker ps -a                   # List all containers (including stopped)
docker run <image>             # Create and start a container
docker start <container>       # Start a stopped container
docker stop <container>        # Stop a running container
docker restart <container>     # Restart container
docker rm <container>          # Remove a container
docker logs <container>        # View container logs
docker exec -it <container> sh # Access shell inside a running container
docker inspect <container>     # Detailed container/image info
```

---

## 6. Working with Images

### Pulling and Running
```bash
docker pull nginx:latest
docker run -d -p 8080:80 --name my-nginx nginx:latest
```

### Common Flags
| Flag | Purpose |
|---|---|
| `-d` | Detached mode (background) |
| `-p host:container` | Port mapping |
| `-v host:container` | Volume mount |
| `--name` | Custom container name |
| `-e KEY=VALUE` | Environment variable |
| `--rm` | Auto-remove container on exit |
| `-it` | Interactive terminal |
| `--network` | Attach to a specific network |

### Tagging & Saving Images
```bash
docker tag myapp:latest myrepo/myapp:v1.0
docker save -o myapp.tar myapp:latest
docker load -i myapp.tar
```

### Image Layers
Every Dockerfile instruction (`RUN`, `COPY`, `ADD`) creates a new **read-only layer**. Layers are cached and reused — this is why layer ordering matters for build speed.

---

## 7. Working with Containers

```bash
docker run -it ubuntu bash          # Interactive container
docker run -d --name web -p 80:80 nginx
docker pause web                    # Pause processes
docker unpause web
docker stats                        # Live resource usage
docker top web                      # Running processes in container
docker cp file.txt web:/app/        # Copy file into container
docker diff web                     # Show filesystem changes
docker commit web myimage:v1        # Create image from container
```

### Container Lifecycle
```
created → running → paused → stopped → removed
```

---

## 8. Dockerfile — Building Custom Images

### Basic Structure
```dockerfile
# Base image
FROM node:20-alpine

# Metadata
LABEL maintainer="you@example.com"

# Set working directory
WORKDIR /app

# Copy dependency files first (better caching)
COPY package*.json ./

# Install dependencies
RUN npm install --production

# Copy rest of the application
COPY . .

# Expose port
EXPOSE 3000

# Environment variable
ENV NODE_ENV=production

# Define non-root user (security best practice)
USER node

# Start command
CMD ["node", "server.js"]
```

### Key Instructions
| Instruction | Purpose |
|---|---|
| `FROM` | Base image |
| `WORKDIR` | Set working directory |
| `COPY` | Copy files from host to image |
| `ADD` | Like COPY, but supports URL/tar extraction |
| `RUN` | Execute command during build |
| `CMD` | Default command when container starts |
| `ENTRYPOINT` | Fixed executable for the container |
| `EXPOSE` | Documents the port the app listens on |
| `ENV` | Set environment variable |
| `ARG` | Build-time variable |
| `VOLUME` | Declare mount point |
| `USER` | Set default user |
| `HEALTHCHECK` | Define container health check |

### CMD vs ENTRYPOINT
```dockerfile
ENTRYPOINT ["python3"]
CMD ["app.py"]
# docker run image           -> python3 app.py
# docker run image other.py  -> python3 other.py
```

### Build the Image
```bash
docker build -t myapp:1.0 .
docker build -t myapp:1.0 -f Dockerfile.prod .
docker build --no-cache -t myapp:1.0 .
```

### .dockerignore
Just like `.gitignore`, prevents unnecessary files from being copied into the image, keeping it smaller and builds faster.
```
node_modules
.git
*.md
.env
dist
```

---

## 9. Docker Volumes & Data Persistence

Containers are ephemeral — data is lost when a container is removed unless persisted using **volumes**.

### Types of Storage
| Type | Description |
|---|---|
| **Volumes** | Managed by Docker, stored in `/var/lib/docker/volumes` (recommended) |
| **Bind Mounts** | Maps a specific host path into the container |
| **tmpfs Mounts** | Stored in memory only (Linux) |

### Commands
```bash
docker volume create mydata
docker volume ls
docker volume inspect mydata
docker volume rm mydata

# Using a volume
docker run -d -v mydata:/app/data myapp

# Bind mount
docker run -d -v /host/path:/container/path myapp
```

### Named Volume Example (Database Persistence)
```bash
docker run -d \
  --name postgres-db \
  -e POSTGRES_PASSWORD=secret \
  -v pgdata:/var/lib/postgresql/data \
  postgres:16
```

---

## 10. Docker Networking

### Network Drivers
| Driver | Use Case |
|---|---|
| `bridge` | Default; isolated network on a single host |
| `host` | Container shares host's network stack |
| `none` | No networking |
| `overlay` | Multi-host networking (Swarm/Kubernetes) |
| `macvlan` | Assign a MAC address, appears as physical device |

### Commands
```bash
docker network ls
docker network create mynetwork
docker network inspect mynetwork
docker network connect mynetwork mycontainer
docker network rm mynetwork

# Run container on custom network
docker run -d --network=mynetwork --name app1 myimage
```

### Container-to-Container Communication
Containers on the same user-defined bridge network can communicate using **container name as hostname**:
```bash
docker network create app-net
docker run -d --name db --network app-net postgres
docker run -d --name backend --network app-net myapp
# backend can reach db via hostname "db"
```

---

## 11. Docker Compose

Docker Compose lets you define and run **multi-container applications** using a single YAML file.

### Example: `docker-compose.yml`
```yaml
version: "3.9"

services:
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    depends_on:
      - db
    networks:
      - app-net

  frontend:
    build: ./frontend
    ports:
      - "3000:80"
    depends_on:
      - backend
    networks:
      - app-net

  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - app-net

volumes:
  pgdata:

networks:
  app-net:
    driver: bridge
```

### Common Commands
```bash
docker compose up -d              # Start all services
docker compose down                # Stop and remove containers/networks
docker compose down -v             # Also remove volumes
docker compose ps                  # List running services
docker compose logs -f             # Follow logs
docker compose build               # Build images
docker compose exec backend sh     # Shell into a service
docker compose restart backend
```

---

## 12. Multi-Stage Builds

Used in production to keep final images **small and secure** by separating build tools from runtime.

```dockerfile
# ---- Build Stage ----
FROM node:20 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# ---- Production Stage ----
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

**Benefits:**
- Final image excludes build tools/dev dependencies.
- Smaller image size → faster deployment, smaller attack surface.

### Example: Go application (extremely small final image)
```dockerfile
FROM golang:1.22 AS builder
WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o app .

FROM scratch
COPY --from=builder /src/app /app
ENTRYPOINT ["/app"]
```

---

## 13. Docker Registry & Docker Hub

### Public Registry (Docker Hub)
```bash
docker login
docker tag myapp:1.0 username/myapp:1.0
docker push username/myapp:1.0
docker pull username/myapp:1.0
```

### Private Registry (self-hosted)
```bash
docker run -d -p 5000:5000 --name registry registry:2
docker tag myapp:1.0 localhost:5000/myapp:1.0
docker push localhost:5000/myapp:1.0
```

### Cloud Registries
- **AWS ECR** (Elastic Container Registry)
- **Google GCR / Artifact Registry**
- **Azure ACR** (Azure Container Registry)
- **GitHub Container Registry (ghcr.io)**

---

## 14. Environment Variables & Secrets

### Passing Environment Variables
```bash
docker run -e DB_HOST=localhost -e DB_PASS=secret myapp
```

### Using an `.env` file
```
# .env
DB_HOST=localhost
DB_PASS=secret123
```
```bash
docker run --env-file .env myapp
```

In `docker-compose.yml`:
```yaml
services:
  app:
    env_file:
      - .env
```

### Docker Secrets (Swarm mode — production-grade secret management)
```bash
echo "mysecretpassword" | docker secret create db_password -
docker service create --name db --secret db_password postgres
```
> **Never hardcode secrets inside a Dockerfile or commit `.env` files to version control.**

---

## 15. Docker in CI/CD Pipelines

### Typical Flow
```
Code Push → CI Build → Docker Build → Test → Push to Registry → Deploy
```

### Example: GitHub Actions Workflow
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: username/myapp:latest
```

### Jenkins Example (Declarative Pipeline)
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push myrepo/myapp:${BUILD_NUMBER}'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
```

---

## 16. Security Best Practices

1. **Use official/minimal base images** (`alpine`, `distroless`) to reduce attack surface.
2. **Run as non-root user**:
   ```dockerfile
   RUN addgroup -S app && adduser -S app -G app
   USER app
   ```
3. **Scan images for vulnerabilities**:
   ```bash
   docker scout cves myapp:latest
   trivy image myapp:latest
   ```
4. **Never store secrets in images** — use secret managers or environment injection.
5. **Use multi-stage builds** to exclude build tools from the final image.
6. **Pin image versions** (`node:20.11.1-alpine`, not `node:latest`).
7. **Limit container resources**:
   ```bash
   docker run --memory=512m --cpus=1 myapp
   ```
8. **Use read-only filesystems** where possible:
   ```bash
   docker run --read-only myapp
   ```
9. **Regularly update base images** to patch CVEs.
10. **Sign and verify images** using Docker Content Trust:
    ```bash
    export DOCKER_CONTENT_TRUST=1
    ```
11. **Avoid `--privileged` mode** unless absolutely required.
12. **Set `HEALTHCHECK`** in Dockerfiles to detect failing containers.

---

## 17. Performance Optimization

### Reduce Image Size
- Use `alpine` or `distroless` base images.
- Combine `RUN` commands to reduce layers:
  ```dockerfile
  RUN apt-get update && apt-get install -y \
      curl \
      git \
      && rm -rf /var/lib/apt/lists/*
  ```
- Use `.dockerignore` to avoid copying unnecessary files.
- Use multi-stage builds.

### Build Caching
- Order Dockerfile instructions from **least to most frequently changing**:
  1. Base image
  2. Dependency installation (`package.json`, `requirements.txt`)
  3. Application source code
- Use BuildKit for faster, parallel builds:
  ```bash
  DOCKER_BUILDKIT=1 docker build -t myapp .
  ```

### Resource Limits
```bash
docker run -d --memory=256m --memory-swap=512m --cpus="1.5" myapp
```

---

## 18. Docker Swarm (Orchestration)

Docker's native clustering/orchestration tool for managing multiple containers across multiple hosts.

```bash
docker swarm init                          # Initialize swarm
docker swarm join-token worker             # Get join token for workers
docker node ls                             # List nodes

# Deploy a service
docker service create --name web --replicas 3 -p 80:80 nginx
docker service ls
docker service scale web=5
docker service update --image nginx:latest web

# Deploy a stack (like Compose, but for Swarm)
docker stack deploy -c docker-compose.yml mystack
docker stack services mystack
docker stack rm mystack
```

---

## 19. Docker with Kubernetes

While Docker builds and runs individual containers, **Kubernetes (K8s)** orchestrates containers at scale across clusters — the industry standard for production deployments.

```
Docker → builds the image
Kubernetes → schedules, scales, and manages containers using that image
```

### Basic Kubernetes Deployment (using a Docker image)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: username/myapp:1.0
          ports:
            - containerPort: 3000
```
```bash
kubectl apply -f deployment.yaml
kubectl get pods
kubectl logs <pod-name>
```

> Note: Modern Kubernetes uses **containerd** as the default runtime (not Docker directly), but Docker-built images remain fully compatible since both follow the **OCI (Open Container Initiative)** standard.

---

## 20. Monitoring & Logging

### Built-in Tools
```bash
docker stats                     # Real-time resource usage
docker logs -f --tail 100 <container>
docker events                    # Real-time Docker events
```

### Industry-Standard Stack
- **Logging:** Fluentd / Filebeat → Elasticsearch → Kibana (ELK/EFK stack)
- **Metrics:** cAdvisor → Prometheus → Grafana
- **APM:** Datadog, New Relic, Dynatrace

### Example: Prometheus + Grafana with Docker Compose
```yaml
services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

---

## 21. Debugging & Troubleshooting

```bash
docker logs <container>                     # Check container logs
docker inspect <container>                  # Full metadata (network, mounts, etc.)
docker exec -it <container> sh               # Shell into running container
docker events --since '1h'                   # Recent Docker events
docker system df                              # Disk usage breakdown
docker system prune -a                        # Clean unused images/containers/networks
docker container prune                        # Remove stopped containers
docker image prune -a                         # Remove unused images
docker volume prune                            # Remove unused volumes
```

### Common Issues
| Issue | Fix |
|---|---|
| Port already in use | Change host port mapping or stop conflicting service |
| Container exits immediately | Check `CMD`/`ENTRYPOINT`, inspect logs |
| Out of disk space | Run `docker system prune -a` |
| Cannot connect between containers | Ensure same custom network, check hostname |
| Permission denied | Check `USER` in Dockerfile / volume permissions |
| Build cache not working | Reorder Dockerfile layers, avoid `--no-cache` unless needed |

---

## 22. Industry Best Practices Checklist

- [ ] Use specific image version tags, never `latest` in production
- [ ] Keep images small (alpine/distroless + multi-stage builds)
- [ ] Run containers as non-root users
- [ ] Use `.dockerignore` to reduce build context
- [ ] Store secrets outside the image (vaults, secret managers)
- [ ] Scan images for vulnerabilities in CI/CD (Trivy, Docker Scout, Snyk)
- [ ] Set resource limits (CPU/memory) for every container
- [ ] Implement health checks (`HEALTHCHECK` in Dockerfile)
- [ ] Use structured logging and centralized log aggregation
- [ ] Automate builds/deployments via CI/CD pipelines
- [ ] Tag images with commit SHA or semantic version for traceability
- [ ] Regularly update base images to patch security vulnerabilities
- [ ] Use orchestration (Kubernetes/Swarm) for production scaling
- [ ] Separate environments using Compose profiles or override files

---

## 23. Interview Questions & Answers

**Q1: What is the difference between an image and a container?**
An image is a static, read-only template; a container is a running (or stopped) instance of that image with a writable layer on top.

**Q2: What is the difference between COPY and ADD?**
`COPY` simply copies files/directories. `ADD` additionally supports remote URLs and automatic extraction of compressed archives — `COPY` is preferred unless those extra features are needed.

**Q3: How does Docker achieve isolation?**
Through Linux kernel features: **namespaces** (isolate process, network, filesystem views) and **cgroups** (limit and account for resource usage like CPU/memory).

**Q4: What is the difference between a Docker volume and a bind mount?**
Volumes are managed entirely by Docker and stored in Docker's storage area — portable and safer. Bind mounts map a specific host directory into the container — more flexible but tightly coupled to the host filesystem.

**Q5: What happens when you run `docker run`?**
Docker checks for the image locally; if absent, pulls it from the registry, creates a new container from the image, allocates a filesystem/network, and starts the defined process.

**Q6: How do you reduce Docker image size?**
Use minimal base images, multi-stage builds, combine RUN layers, remove build dependencies, and use `.dockerignore`.

**Q7: What is the difference between Docker Compose and Docker Swarm?**
Compose defines and runs multi-container apps on a **single host**. Swarm orchestrates containers across a **cluster of multiple hosts**, providing scaling, load balancing, and failover.

---

## 24. Cheat Sheet

```bash
# Images
docker build -t name:tag .
docker images
docker rmi <image>
docker pull <image>
docker push <image>
docker tag <image> <newtag>

# Containers
docker run -d -p host:container --name X image
docker ps -a
docker start/stop/restart <container>
docker rm <container>
docker exec -it <container> sh
docker logs -f <container>

# Compose
docker compose up -d
docker compose down
docker compose logs -f
docker compose build

# Volumes & Networks
docker volume create/ls/rm
docker network create/ls/rm

# Cleanup
docker system prune -a
docker container prune
docker image prune -a
docker volume prune

# Debug
docker inspect <container>
docker stats
docker top <container>
```

---

## Summary

Docker is foundational to modern software delivery. Mastering it means understanding not just individual commands, but the underlying principles: **image layering, isolation, networking, storage, orchestration, and security** — all of which are essential for building resilient, production-grade systems used across the industry today.

*Document generated as a complete Docker learning and reference guide — from fundamentals to production/industry-level practices.*
