# The Best Way to Set Up Docker for Your OS

## Overview

There are lots of ways to run Docker. Docker Inc. is now focused on local Docker Desktop. Docker Desktop is best for running local containers on Windows, Linux, and macOS.

- **Docker Desktop is NOT for servers.**
- **Docker Desktop is free for learning.**

### Docker Desktop (DD) Includes
- Engine
- CLI
- Compose
- BuildKit
- Kubernetes
- Scan
- SBOM
- ...and more

> Linux containers require a Linux kernel (OS).

### 3 Major Ways to Run Containers
1. **Locally** — Docker Desktop, Rancher Desktop (RD)
2. **Servers** — Docker Engine, Kubernetes (K8s)
3. **PaaS** — Cloud Run, Fargate

> **Docker Engine** = OCI container runtime

This course assumes **Docker CLI + Engine**.

---

## Docker for Windows: Setup and Tips

1. Install Docker Desktop on Windows 10+
2. Tweak Docker Desktop settings
3. Clone the course GitHub repo in Linux
4. Install VS Code
5. Tweak your terminal and shell

### Docker Desktop Setup
- Install from [docs.docker.com](https://docs.docker.com)
- In configuration, click **Use WSL 2**
- Add a shortcut

> Docker Desktop is always free for learning.

### WSL2 Requirements
- Linux kernel update
- CPU virtualization extension enabled

### Usage Tip
- Click the Docker symbol, then use the **Ubuntu shell** — not PowerShell or Command Prompt.

---

## Docker for Mac

> Docker Desktop is always free for learning.

---

## Docker for Linux: Server Setup and Tips

1. Install Docker Engine on Linux
2. Install CLI locally, connecting to a remote engine
3. Clone the course repo locally

> **Docker Desktop != Docker Engine + CLI**

### Install Docker Engine on Ubuntu

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
docker version
```

- Docker Engine is open source (Apache License).
- Log in to Docker Desktop/Docker Hub for more pulls.

### Remote Docker Engine via SSH Tunnel
- Connect to a remote Docker Engine via an SSH tunnel.

---

## Docker Version and Product Changes

Over the years, Docker Inc. has changed products, versions, product names, licensing, and even company focus. Key changes:

- Docker CLI is backward compatible — the latest version works with all commands shown in this course, regardless of version.

**2022** — [Docker Desktop for Linux](https://docs.docker.com/desktop/linux/) launches. Docker Desktop also gets **Extensions** for adding 3rd-party features to the GUI.

**2021** — Docker Desktop licensing changes to [require a subscription](https://www.docker.com/pricing/) for use in large companies (more than 250 employees OR more than $10 million USD in annual revenue). Docker Desktop remains 100% free for learning Docker and Kubernetes in this course — personally verified with the Docker Inc. team as 100% free for personal use and learning setups. See the [pricing FAQ](https://www.docker.com/pricing/faq/) for more.

**2021** — [Docker Compose](https://github.com/docker/compose) is rebuilt as a Docker CLI plugin, called **Compose V2**, rather than a standalone Python binary. It's faster and better. The command `docker compose` (with a space) supersedes `docker-compose` (auto-installed with Docker Desktop). Both commands currently work the same, but V2 is starting to get new features like `docker compose ls`.

**2020** — Docker Machine and Docker Toolbox are deprecated and archived. They may still work but receive no updates. Docker Desktop now works on Windows WSL2 and Home editions, so all Windows 10/11 editions work best with Docker Desktop. As a backup plan or for local multi-VM management, use [Multipass](https://multipass.run/) to quickly spin up multiple Ubuntu VMs and manually install Docker as you would [on a Linux server](https://docs.docker.com/engine/install/).

**2019** — Docker Inc.'s paid server products (Docker Datacenter, DTR, Docker for Windows Server, and Docker Enterprise) were [sold to Mirantis](https://www.mirantis.com/software/mirantis-kubernetes-engine/). Nothing changes in Docker's desktop products or open source. Docker Inc. now focuses on developer tools and exits the paid server product market.

**2019** — Many product name changes:
- "Docker for Mac" and "Docker for Windows" → **Docker Desktop**
- Docker CE and Docker EE → **Docker Engine** and **Docker CLI**
- No more Edge, Beta, or Community versions. See [release channels of Docker Engine](https://docs.docker.com/engine/install/#release-channels).

**2017** — Versions become `YY.MM`-based (like Ubuntu). Earlier mentions of `1.12` and `1.13` refer to the two versions before the date-based versioning began.
