# Creating and Using Containers Like a Boss

## 1. Check Your Docker Install and Config

Containers are the fundamental building block of the Docker toolkit — this is one of the first things to get comfortable with.

```bash
docker version
```
Verifies the CLI can talk to the Docker Engine (shows client and server version info).

```bash
docker info
```
Shows most configuration values for the engine (system-wide info: containers, images, storage driver, etc.).

---

## 2. Docker Command Line Structure

**New "management commands" format (preferred):**
```
docker <command> <sub-command> (options)
```

**Old way (still works):**
```
docker <command> (options)
```

Example — both do the same thing:
```bash
docker container run   # new way
docker run              # old way
```

---

## 3. Starting an Nginx Web Server

### Image vs. Container
- An **image** is the application we want to run.
- A **container** is an instance of that image running as a process.
- You can have many containers running off the same image.
- In this example, our image is the **Nginx** web server.
- Docker's default image registry is called **Docker Hub** — [hub.docker.com](https://hub.docker.com)

### Run Nginx
```bash
docker container run --publish 80:80 nginx
```
This command:
- Downloaded the image `nginx` from Docker Hub
- Started a new container from that image
- Opened port 80 on the host IP
- Routes that traffic to the container's IP on port 80

Press `Ctrl+C` to stop it in the foreground.

### Run It Detached (in the Background)
```bash
docker container run --publish 80:80 --detach nginx
```

---

## 4. What Actually Happens with `docker container run`

1. Looks for that image locally in the image cache — doesn't find anything.
2. Looks in the remote image repository (defaults to Docker Hub).
3. Downloads the latest version of the image (`nginx:latest` by default).
4. Creates a new container based on that image and prepares to start it.
5. Gives it a virtual IP on a private network inside the Docker Engine.
6. Opens up a port on the host and forwards it to the matching port in the container.
7. Starts the container using the `CMD` defined in the image's Dockerfile.

### Example with More Options
```bash
docker container run --publish 8080:80 --name webhost -d nginx:1.11 nginx -T
```

---

## 5. Containers vs. VMs — It's Just a Process

```bash
docker run --name mongo -d mongo
```

```bash
docker ps
```
Lists running containers.

```bash
docker top mongo
```
Lists the processes running **inside** that container.

```bash
docker stop mongo
docker ps
```
Confirms the container is no longer running.

Compare with host-level process listing:
```bash
ps aux
ps aux | grep mongo
```

Restart it:
```bash
docker start mongo
docker top mongo
ps aux | grep mongo
```

This proves a container is really just an isolated **process** running on the host — not a separate machine like a VM.

---

## 6. Assignment: Manage Multiple Containers

> `docs.docker.com` and `--help` are your friends.

**Task:**
Run three containers:
- **nginx** → listening on `80:80`
- **httpd** (Apache) → listening on `8080:80`
- **mysql** → listening on `3306:3306`

When running MySQL, use the `--env` (or `-e`) option to pass in:
```
MYSQL_RANDOM_ROOT_PASSWORD=yes
```

Steps:
1. Use `docker container logs` on the MySQL container to find the random root password it created on startup.
2. Clean everything up with `docker container stop` and `docker container rm` — both accept multiple names or IDs.
3. Use `docker container ls` (before and after cleanup) to confirm everything is correct.

---

## 7. Assignment Answer: Manage Multiple Containers

### Start MySQL
```bash
docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
```

### Check the Random Root Password
```bash
docker container logs db
```

### Start the Apache (httpd) Web Server
```bash
docker container run -d --name webserver -p 8080:80 httpd
docker ps
```

### Start the Nginx Proxy
```bash
docker container run -d --name proxy -p 80:80 nginx
docker ps
docker container ls
```

### Test the Servers
```bash
curl localhost
curl localhost:8080
```

### Clean Up
```bash
docker container stop db webserver proxy
docker ps -a
docker container ls -a

docker container rm db webserver proxy
docker image ls
```

---

## Quick Reference — Commands Used

| Command | Purpose |
|---|---|
| `docker version` | Check client/server version |
| `docker info` | Show engine configuration |
| `docker container run` / `docker run` | Create & start a container |
| `--publish` / `-p` | Map host port to container port |
| `--detach` / `-d` | Run container in the background |
| `--name` | Assign a custom container name |
| `--env` / `-e` | Set environment variable in container |
| `docker ps` / `docker container ls` | List running containers |
| `docker ps -a` / `docker container ls -a` | List all containers (including stopped) |
| `docker top <container>` | Show processes running inside a container |
| `docker stop <container>` | Stop a running container |
| `docker start <container>` | Start a stopped container |
| `docker container logs <container>` | View container logs |
| `docker container rm <container>` | Remove a container |
| `docker image ls` | List local images |
