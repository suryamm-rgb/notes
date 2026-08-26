# DevOps Learning Roadmap (Beginner → Advanced)

> Prerequisite: You already know JavaScript/TypeScript, React, Next.js, Git, and GitLab — so no need to learn another programming language before starting DevOps.

## Learning Path Overview

```
JavaScript / TypeScript
        ↓
      Git ✅
        ↓
    GitLab ✅
        ↓
      Linux
        ↓
      Bash
        ↓
   Networking
        ↓
      Docker
        ↓
 Docker Compose
        ↓
    GitLab CI/CD
        ↓
    AWS Basics
        ↓
      Nginx
        ↓
    Terraform
        ↓
   Kubernetes
        ↓
   Advanced DevOps
```

---

## 1. Linux — FIRST

The most important foundation. Goal: **be comfortable operating an Ubuntu server from the terminal.**

- Filesystem: `/`, `/home`, `/etc`, `/var`, `/opt`
- Navigation & files: `cd`, `ls`, `pwd`, `mkdir`, `rm`, `cp`, `mv`
- Viewing/searching: `cat`, `less`, `grep`, `find`
- Permissions: `chmod`, `chown`
- Processes: `ps`, `top`, `kill`
- Networking: `curl`, `wget`, `ping`
- SSH
- Environment variables
- Package managers
- Services and logs

> **Real-world use:** Every server you deploy to (EC2, DigitalOcean, on-prem) runs Linux. You'll SSH in to check logs, restart services, check disk space, and debug live issues — a daily skill, not optional.

---

## 2. Bash / Shell Scripting

You don't need to become an expert programmer. Goal: **automate repetitive Linux commands.**

```bash
#!/bin/bash

echo "Deploying application"

docker pull my-app:latest
docker compose up -d
```

Understand:

- Variables
- `if` statements
- Loops
- Functions
- Exit codes
- Pipes `|`
- `&&`
- Environment variables
- Command substitution

> **Real-world use:** Real projects have deploy scripts, health-check scripts, backup scripts, and Docker entrypoint scripts written in bash. Almost every CI/CD pipeline calls bash commands under the hood.

---

## 3. Networking Basics

Before Docker/cloud, understand:

- IP address
- Port
- TCP/UDP
- HTTP/HTTPS
- DNS
- localhost
- Common ports: `80`, `443`, `22`, `3000`
- Reverse proxy
- Firewall
- Domain → server IP

**Example flow:**

```
mywebsite.com
      ↓
     DNS
      ↓
  Server IP
      ↓
  Nginx :443
      ↓
Next.js container :3000
```

> **Real-world use:** You'll constantly debug issues like "why can't my container reach the database" or "why isn't my site resolving" — this is 90% networking (DNS, ports, firewalls).

---

## 4. Docker ⭐

Your **first major DevOps technology**.

```
Dockerfile
    ↓
Docker Image
    ↓
Docker Container
```

Learn:

- Images
- Containers
- Dockerfile
- Volumes
- Networks
- Ports
- Environment variables
- Docker Hub / Registry
- Multi-stage builds
- `.dockerignore`
- Container logs
- Container health checks

**Practice with your Next.js application.**

> **Real-world use:** Nearly universal today. Real companies ship apps as Docker images — your Next.js app, your API, your database — each containerized so it runs identically on your laptop and in production.

---

## 5. Docker Compose

Learn how to run multiple services together.

**Example stack:**

```
Next.js
   ↓
Payload CMS
   ↓
PostgreSQL
```

Learn:

- `docker-compose.yml`
- `docker compose up`
- `docker compose down`
- `docker compose ps`
- `docker compose logs`
- `docker compose restart`

> **Real-world use:** Used heavily for local development and small-to-medium production setups where you're running multiple services (app + DB + cache) on a single server without full Kubernetes.

---

## 6. CI/CD — GitLab CI

You already know GitLab, so this should be relatively easy.

Learn `.gitlab-ci.yml`. Start simple:

```
Git Push
   ↓
  Lint
   ↓
  Test
   ↓
 Build
```

Then progress to:

```
 Git Push
    ↓
 GitLab CI
    ↓
Docker Build
    ↓
Docker Registry
    ↓
  Deploy
```

Learn:

- Jobs
- Stages
- Runners
- Artifacts
- Cache
- Variables
- Secrets
- Branch rules
- Environments
- Manual deployments

> **Real-world use:** This is literally how real teams ship code — every push triggers lint → test → build → deploy automatically. Companies rely on this to avoid manual deployments and human error.

---

## 7. Cloud — AWS

Only start AWS **after** Linux + Docker + CI/CD. Don't try to learn all of AWS at once.

Start with:

- EC2
- IAM
- VPC
- Security Groups
- S3
- CloudWatch
- Route 53
- ECR

**Most important initially:** EC2 → Docker → Deploy your application

**Example pipeline:**

```
  GitLab
    ↓
  CI/CD
    ↓
Docker Image
    ↓
 AWS ECR
    ↓
 AWS EC2
    ↓
Docker Container
    ↓
Your Website
```

> **Real-world use:** Real infra runs here. EC2 hosts your app, S3 stores files/backups, ECR stores your Docker images, IAM controls who can access what, and Route 53 handles your domain.

---

## 8. Nginx

Learn Nginx after you understand servers and Docker. You'll use it as a **reverse proxy**.

```
   Internet
       ↓
https://example.com
       ↓
  Nginx :443
       ↓
Next.js Container :3000
```

Learn:

- Reverse proxy
- SSL/HTTPS
- Domain configuration
- Load balancing basics
- Nginx logs

> **Real-world use:** Almost every production web app sits behind Nginx (or similar) as a reverse proxy — handling HTTPS/SSL, routing traffic to the right container, and sometimes load balancing across multiple app instances.

---

## 9. Terraform

Once you understand AWS manually, learn Terraform (Infrastructure as Code).

Instead of manually creating EC2, Security Groups, Networks, and S3 buckets, define them as code:

```
Terraform
    ↓
   AWS
    ↓
Infrastructure
```

Learn:

- Providers
- Resources
- Variables
- Outputs
- State
- Modules
- `terraform plan`
- `terraform apply`
- `terraform destroy`

> **Real-world use:** Real teams don't click around the AWS console to create servers — they define infrastructure as code so it's version-controlled, repeatable, and can be destroyed/recreated instantly. Standard at any company with more than a handful of engineers.

---

## 10. Kubernetes — LAST

Don't start Kubernetes until you've mastered everything before it:

```
   Linux
     ↓
   Docker
     ↓
Docker Compose
     ↓
   CI/CD
     ↓
    AWS
     ↓
 Terraform
     ↓
Kubernetes
```

Then learn:

- Pod
- Deployment
- Service
- ConfigMap
- Secret
- Namespace
- Ingress
- PersistentVolume
- Helm
- Kubernetes networking

> **Real-world use:** Used at scale — when you have many services, need auto-scaling, zero-downtime deployments, and self-healing infrastructure. Not every project needs it (smaller apps use just Docker Compose + EC2), but larger companies almost always run on it.

---

## A Note on Python

You don't need Python first — learn **Bash before Python**.

Python becomes useful later for:

- Automation
- AWS scripts
- Tooling
- APIs
- Custom DevOps utilities

Don't pause your Docker/CI/CD learning to pick up Python.

---

## Recommended Hands-On Project

Don't just watch courses — take one of your existing Next.js projects and progressively turn it into a production system.

### Phase 1 — Containerize

```
Next.js → Docker → Container
```

### Phase 2 — Add Services

```
Next.js + PostgreSQL + Payload → Docker Compose
```

### Phase 3 — Basic CI

```
GitLab → GitLab CI → Lint → Test → Build
```

### Phase 4 — Build & Push Images

```
GitLab → CI → Docker Build → GitLab Container Registry
```

### Phase 5 — Deploy to AWS

```
GitLab → CI/CD → Docker Registry → AWS EC2 → Docker Compose → Live Website
```

### Phase 6 — Infrastructure as Code

```
Terraform → Create AWS infrastructure → Deploy application
```

### Phase 7 — Orchestration at Scale

```
Kubernetes → Deploy application → Scale application → Rolling updates
```

---

## Quick Reference: Real-World Usage Summary

| Topic          | Where It's Used in Real Projects                                                             |
| -------------- | -------------------------------------------------------------------------------------------- |
| Linux          | Daily server operations — SSH in, check logs, restart services, debug live                   |
| Bash           | Deploy scripts, health checks, Docker entrypoints, CI/CD pipeline steps                      |
| Networking     | Debugging connectivity, DNS, firewalls, container-to-container communication                 |
| Docker         | Packaging apps so they run identically everywhere (laptop → production)                      |
| Docker Compose | Local dev environments, small/medium production stacks (app + DB + cache)                    |
| GitLab CI/CD   | Automated lint → test → build → deploy on every push                                         |
| AWS            | Hosting (EC2), file storage (S3), image registry (ECR), access control (IAM), DNS (Route 53) |
| Nginx          | Reverse proxy, HTTPS/SSL termination, routing, basic load balancing                          |
| Terraform      | Version-controlled, repeatable infrastructure — no manual console clicking                   |
| Kubernetes     | Large-scale orchestration — auto-scaling, zero-downtime deploys, self-healing                |

**Bottom line:** Steps 1–8 (Linux → AWS/Nginx) are used in essentially every real DevOps job, even at small startups. Terraform and Kubernetes become essential as systems grow larger and more complex.
