# Docker Notes

## Play with Docker
[Play with Docker (PWD)](https://labs.play-with-docker.com/) is a free, browser-based playground for learning and experimenting with Docker.

> ⚠️ **Important:** Play with Docker is for **labs and learning only** — instances are temporary (sessions expire after a few hours) and should **never** be used to run real/production workloads.

### Step 1 — Add a New Instance
On the PWD dashboard, click **"+ ADD NEW INSTANCE"** to spin up a fresh Linux terminal with Docker pre-installed.

Check the Docker version to confirm it's ready:
```bash
docker version
```

---

## How the Docker Client Talks to the Server

Docker uses a client-server architecture:

```
Client  --->  Socket / TCP (TLS) / SSH tunnel  --->  Docker Server (daemon)
```

- **Client**: the `docker` CLI command you type.
- **Transport**: communication happens over a Unix socket (local), or TCP with TLS / SSH tunnel (remote).
- **Server (daemon)**: `dockerd`, which actually creates and manages containers.

---

## Quick Container Run

Run an Apache (`httpd`) web server container, mapped to host port `8800`:

```bash
docker run -d -p 8800:80 httpd
```

- `-d` → run in detached mode (background)
- `-p 8800:80` → map host port `8800` to container port `80`
- `httpd` → the image to use (Apache HTTP Server)

Test it:
```bash
curl localhost:8800
```

List running containers:
```bash
docker ps
```

Run a second instance on a different port:
```bash
docker run -d -p 8801:80 httpd
```

### Multi-Server Example

| Server 1 | Server 2 |
|---|---|
| `docker run httpd` (port 8800) | `docker run httpd` (port 8801) |

Both servers can run the same **image** (Apache web server), each isolated in its own container, listening on a different host port.

**Image contents (example — `httpd`):**
- Apache web server
- Debian-based OS binaries and libraries
- All dependencies required to run Apache

**Environment:** Linux

---

## Why Docker?

- Consistent environments across dev, test, and production
- Lightweight compared to full VMs — containers share the host OS kernel
- Fast startup and teardown
- Easy to package an app with all its dependencies
- Portable — runs the same way on any machine with Docker installed
- Simplifies scaling and orchestration of applications

---

## Learning Roadmap

1. **Gathering Requirements** — understand what you're building/deploying
2. **Docker Install** — set up Docker Engine on your OS
3. **Container Basics** — running, stopping, inspecting containers
4. **Image Basics** — pulling, building, and managing images
5. **Networking** — how containers communicate (bridge, host, overlay networks)
6. **Docker Volumes** — persistent data storage for containers
7. **Docker Compose** — defining and running multi-container applications
8. **Orchestration** — managing containers at scale
9. **Docker Swarm** — Docker's native clustering/orchestration tool
10. **Kubernetes (K8s)** — industry-standard container orchestration platform
11. **Swarm vs K8s** — comparing the two orchestration approaches
