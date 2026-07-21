# Docker - Complete Beginner Notes

## What is Docker?

Docker is an open-source containerization platform that packages
applications and all their dependencies into portable **Docker Images**,
which run as **Docker Containers** consistently on any machine with
Docker installed.

## Why Docker?

Docker solves the classic problem:

> "It works on my machine."

By packaging the application, runtime, libraries, and configuration
together, Docker ensures the same behavior across development, testing,
and production.

## Three Docker Innovations

### 1. Docker Image (Universal Application Package)

A Docker Image is a read-only package containing: - Application code -
Runtime - Libraries - Dependencies - Configuration

### 2. Docker Registry (Universal Application Distribution)

A Docker Registry stores and distributes Docker Images.

Common registries: - Docker Hub - GitHub Container Registry - AWS ECR -
Azure Container Registry

Commands:

``` bash
docker push username/my-app:v1
docker pull username/my-app:v1
```

### 3. Docker Container (Identical Runtime Environment)

A Docker Container is a running instance of a Docker Image.

## Dockerfile

A Dockerfile is a **recipe** used to create a Docker Image.

``` dockerfile
FROM node:20
WORKDIR /app
COPY package*.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm","start"]
```

Build:

``` bash
docker build -t my-app .
```

## Docker Workflow

1.  Write application
2.  Create Dockerfile
3.  Build Docker Image
4.  Push Image to Registry
5.  Pull Image
6.  Run Docker Container

## Common Commands

``` bash
docker --version
docker images
docker ps
docker ps -a
docker pull nginx
docker build -t my-app .
docker run -p 3000:3000 my-app
docker stop <container_id>
docker rm <container_id>
docker rmi <image_name>
docker push username/my-app:v1
docker pull username/my-app:v1
```

## Where Docker is Used

-   Web applications
-   APIs
-   Microservices
-   DevOps
-   CI/CD
-   Cloud deployments
-   Testing
-   Machine Learning

## Image vs Container

  Docker Image                 Docker Container
  ---------------------------- --------------------------
  Blueprint                    Running application
  Read-only                    Running process
  Can create many containers   Executes the application

## Summary

-   Docker is a containerization platform.
-   Dockerfile is a recipe for creating an Image.
-   Docker Image is a universal application package.
-   Docker Registry is for storing and sharing Images.
-   Docker Container is the running application.
-   Use `docker build` to build, `docker push` to upload, `docker pull`
    to download, and `docker run` to start a container.
