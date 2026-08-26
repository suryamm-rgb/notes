# Docker: CLI Process Monitoring, Shells, and Networking

## 1. What's Going On in Containers — CLI Process Monitoring

| Command | Purpose |
|---|---|
| `docker container top` | Process list running inside **one** container |
| `docker container inspect` | Full details of one container's config |
| `docker container stats` | Live performance stats for **all** running containers |

### Example Walkthrough
```bash
docker container run -d --name nginx nginx
docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql

docker container ls

docker container top mysql
docker container top nginx

docker container inspect nginx

docker container stats --help

docker container ls
```

---

## 2. Getting a Shell Inside Containers (No SSH Needed)

| Command | Purpose |
|---|---|
| `docker container run -it` | Start a **new** container interactively |
| `docker container exec -it` | Run an additional command in an **existing** container |

### Interactive New Container
```bash
docker container run -it --help

docker container run -it --name proxy nginx
docker container run -it --name proxy nginx bash
```
Inside the container:
```bash
ls -al
exit
```

### Ubuntu Example
```bash
docker container run -it --name ubuntu ubuntu
apt-get install -y curl
curl google.com
exit
```

### Restarting and Re-attaching to a Stopped Container
```bash
docker container ls -a
docker container start --help
docker container start -ai ubuntu
```

### Exec into a Running Container
```bash
docker container exec -it mysql bash
ps aux
```

---

## 3. Different Linux Distros in Containers

**Alpine Linux** — a small, security-focused distribution.

```bash
docker pull alpine
docker container run -it alpine bash    # note: bash is NOT included by default in Alpine
docker container run -it alpine sh      # use sh instead
apk                                      # Alpine's package manager
```

### Summary — Getting a Shell Inside Containers
- `docker container run -it ...` → new interactive container
- `docker container exec -it ...` → shell into an existing/running container
- Different distros (Ubuntu, Alpine, etc.) behave differently — check what shell/package manager is available (`bash` vs `sh`, `apt-get` vs `apk`)

---

## 4. Docker Networking — Concepts for Private & Public Communication

### Quick Practical Tools
- Review of `docker container run -p` — for local dev/testing, this usually "just works."
- Quick port check: `docker container port <container>`
- Goal: understand the **concepts** of Docker networking and how packets move around Docker.

### Docker Network Defaults
- Each container is connected to a private virtual network called **`bridge`**.
- Each virtual network routes through a **NAT firewall** on the host IP.
- Best practice: create a **new virtual network for each app**.
  - e.g. network `my_web_app` for MySQL + PHP/Apache containers
  - e.g. network `my_api` for Mongo + Node.js containers

### Docker Networks — "Batteries Included, But Removable"
Defaults work well in many cases, but it's easy to swap out parts to customize:
- Create new virtual networks
- Attach containers to more than one virtual network
- Skip the virtual network and use the host's IP directly (`--net=host`)
- Use different Docker network **drivers** to gain new capabilities

### Example: Inspecting Container Networking
```bash
docker container run -p 80:80 --name webhost -d nginx

docker container port webhost

docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost
```

---

## 5. Docker Networks — CLI Management of Virtual Networks

| Command | Purpose |
|---|---|
| `docker network ls` | Show all networks |
| `docker network inspect` | Inspect a specific network |
| `docker network create --driver` | Create a network |
| `docker network connect` | Attach a network to a container |
| `docker network disconnect` | Detach a network from a container |

### Examples
```bash
docker network ls

# Default networks include: bridge, host, none
```

- **`bridge`** — the default private network for containers
- **`host`** — removes network isolation; container uses host's networking directly
- **`none`** — removes networking entirely (no `eth0` interface)

### Creating and Using a Custom Network
```bash
docker network create my_app_net
docker network ls
docker network create --help

docker container run -d --name new_nginx --network my_app_net nginx

docker network connect my_app_net some_other_container
```

---

## 6. Docker Network Default Security

- Design your apps so **frontend and backend containers sit on the same Docker network**.
- Their inter-communication **never leaves the host** — it stays internal.
- Anything meant to be reached externally is explicitly exposed via `-p` (publish port) — this is a better security default (deny by default, allow explicitly).
- This model gets even more powerful later with **Swarm** and **overlay networks** (multi-host networking).

---

## 7. Docker Network DNS — How Containers Find Each Other

Key ideas:
- **DNS is the key** to easy inter-container communication.
- On **custom (user-defined) networks**, Docker provides built-in DNS resolution by container name — no manual IP tracking needed.
- On the **default bridge network**, you'd need the legacy `--link` flag to get similar name resolution; custom networks make this automatic.

### Docker's Built-in DNS
```bash
docker container ls

docker container run -d --name my_nginx --network my_app_net nginx

docker container exec -it new_nginx ping my_nginx
```
Because both `new_nginx` and `my_nginx` are on the same custom network (`my_app_net`), each container can reach the other **by container name** — Docker's internal DNS resolves it automatically.

```bash
docker container create --help
```

---

## Quick Reference — Commands Used

| Command | Purpose |
|---|---|
| `docker container top <name>` | List processes in a container |
| `docker container inspect <name>` | Full container config details |
| `docker container stats` | Live resource usage for all containers |
| `docker container run -it` | Start new container with interactive shell |
| `docker container exec -it <name> <cmd>` | Run a command in a running container |
| `docker container start -ai <name>` | Restart & attach to a stopped container interactively |
| `docker container port <name>` | Show published ports for a container |
| `docker network ls` | List all Docker networks |
| `docker network inspect <name>` | Inspect a network's details |
| `docker network create <name>` | Create a custom virtual network |
| `docker network connect <net> <container>` | Attach a container to a network |
| `docker network disconnect <net> <container>` | Detach a container from a network |
| `--network <name>` | Run a container attached to a specific network |
| `--net=host` | Skip virtual networking, use host's network directly |
| `--net=none` | Remove all networking from a container |

---

## Key Takeaways

1. **Monitoring**: `top`, `inspect`, and `stats` give you visibility into what's happening inside and across containers — no need to SSH in.
2. **Shells**: Use `run -it` for a brand-new interactive container, `exec -it` to hop into one already running. Remember: not every distro has `bash` (Alpine uses `sh` + `apk`).
3. **Networking**: Every container gets a private IP on the default `bridge` network by default. Best practice is creating **one custom network per application** — this enables automatic DNS resolution by container name and keeps inter-service traffic off the host's public interface, exposing only what's needed via `-p`.
