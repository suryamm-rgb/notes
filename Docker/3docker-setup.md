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

## Docker for Windows: Full Install & Setup Guide

### Overview / Checklist
1. Check system requirements
2. Enable WSL 2 + virtualization
3. Install Docker Desktop on Windows 10/11
4. Configure Docker Desktop settings
5. Clone the course GitHub repo (in the Linux/WSL filesystem)
6. Install VS Code (+ Remote/WSL extension)
7. Tweak your terminal and shell

### Step 1: System Requirements
- Windows 10 64-bit (Build 19041+) / Windows 11 — Home, Pro, Enterprise, or Education
- 64-bit processor with **SLAT (Second Level Address Translation)**
- 4GB RAM minimum
- **CPU virtualization enabled in BIOS/UEFI** (Intel VT-x or AMD-V)

### Step 2: Enable WSL 2

Open **PowerShell as Administrator** and run:

```powershell
wsl --install
```

This installs WSL, sets WSL 2 as default, and installs Ubuntu by default. Restart your PC when prompted.

If WSL is already installed, just make sure it's updated:

```powershell
wsl --update
```

Verify the WSL 2 Linux kernel is up to date (Docker Desktop will prompt you if not — there's a direct download link from Microsoft if needed).

Check virtualization is enabled:
```powershell
systeminfo
```
Look for "Virtualization Enabled in Firmware: Yes". If it says No, enable virtualization (VT-x/AMD-V) in your BIOS/UEFI settings.

### Step 3: Install Docker Desktop
1. Download the installer from [docs.docker.com/desktop/install/windows-install](https://docs.docker.com/desktop/install/windows-install/)
2. Run `Docker Desktop Installer.exe`
3. On the configuration screen, make sure **"Use WSL 2 instead of Hyper-V"** is checked (this is the default and recommended)
4. Follow the prompts, then restart if required
5. Add a **desktop shortcut** if not already created
6. Launch Docker Desktop — it will finish setup and start the Docker Engine

> Docker Desktop is always **free for learning** and personal use.

### Step 4: Configure Docker Desktop Settings
- **Settings → General**: confirm "Use the WSL 2 based engine" is enabled
- **Settings → Resources → WSL Integration**: enable integration with your Ubuntu distro
- **Settings → Resources**: adjust CPU/Memory/Disk limits if needed
- Sign in to Docker Hub (optional, but increases pull rate limits)

### Step 5: Use the Right Shell
- Use the **Ubuntu (WSL) shell** for all Docker commands — **not** PowerShell or Command Prompt
- Open it via the Start Menu ("Ubuntu") or by clicking the Docker whale icon → Terminal

### Step 6: Verify the Install
In the Ubuntu shell:
```bash
docker version
docker run hello-world
```

### Step 7: Clone the Course Repo & Install VS Code
```bash
git clone <course-repo-url>
```
- Install [VS Code](https://code.visualstudio.com/) on Windows
- Install the **"WSL"** and **"Dev Containers"** extensions in VS Code so it can edit files directly inside your Ubuntu/WSL environment
- Open your cloned repo folder from inside the WSL shell using:
```bash
code .
```

### Step 8: Tweak Your Terminal (Optional but Recommended)
- Install **Windows Terminal** from the Microsoft Store for tabs, themes, and better WSL integration
- Set Ubuntu as your default profile in Windows Terminal settings

---

## Docker for Mac: Full Install & Setup Guide

### Step 1: System Requirements
- macOS 12 (Monterey) or newer (check current Docker Desktop docs for the latest minimum version)
- Apple Silicon (M1/M2/M3/M4) or Intel chip — download the correct build for your chip
- 4GB RAM minimum

### Step 2: Install Docker Desktop
1. Download from [docs.docker.com/desktop/install/mac-install](https://docs.docker.com/desktop/install/mac-install/)
   - Choose **Apple Silicon** or **Intel chip** installer correctly
2. Drag `Docker.app` into the `Applications` folder
3. Open Docker from Applications (Spotlight: `Cmd + Space` → "Docker")
4. Grant privileged access when prompted (needed for networking/filesystem)
5. Wait for the whale icon in the menu bar to show Docker is running

> Docker Desktop is always **free for learning** and personal use.

### Step 3: Configure Settings
- **Settings → Resources**: adjust CPU/Memory/Disk as needed
- **Settings → General**: enable "Use Virtualization framework" (recommended on modern macOS)
- Sign in to Docker Hub (optional, for higher pull limits)

### Step 4: Verify the Install
Open **Terminal** (or iTerm2) and run:
```bash
docker version
docker run hello-world
```

### Step 5: Terminal & Tools
- Install [VS Code](https://code.visualstudio.com/)
- Optional: install [iTerm2](https://iterm2.com/) and [Oh My Zsh](https://ohmyz.sh/) for a nicer shell experience
- Clone the course repo:
```bash
git clone <course-repo-url>
```

---

## Docker for Linux: Full Install & Setup Guide (Server / Engine Only)

> **Docker Desktop != Docker Engine + CLI.** On Linux servers, you typically install **Docker Engine + CLI directly** — no Desktop GUI needed. (Docker Desktop for Linux does exist for local dev machines with a GUI, but servers use Engine only.)

### Option A — Quick Install Script (recommended for learning/dev)
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### Option B — Manual Install via APT (Ubuntu/Debian) — recommended for production
```bash
# Remove any old versions
sudo apt-get remove docker docker-engine docker.io containerd runc

# Set up Docker's apt repository
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine, CLI, containerd, Buildx, and Compose plugin
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Post-Install: Run Docker Without `sudo`
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Log out and back in for group changes to fully apply.

### Enable Docker to Start on Boot
```bash
sudo systemctl enable docker
sudo systemctl start docker
```

### Verify the Install
```bash
docker version
docker run hello-world
```

- Docker Engine is **open source** (Apache License 2.0).
- Log in to Docker Hub (`docker login`) for higher image pull rate limits.

### Installing the CLI Locally to Connect to a Remote Engine
If you want to run the `docker` CLI on your laptop but talk to Docker Engine running on a remote server:

```bash
export DOCKER_HOST=ssh://user@remote-server-ip
docker version
```

This works because Docker CLI supports SSH as a connection protocol — no extra tunnel setup needed as long as SSH access works and the remote user is in the `docker` group.

### Remote Docker Engine via SSH Tunnel (Manual Method)
```bash
ssh -fNL localhost:23750:/var/run/docker.sock user@remote-server-ip
export DOCKER_HOST=tcp://localhost:23750
docker version
```
- `-f` runs SSH in the background
- `-N` means no remote command is executed
- `-L` sets up the local port forward to the remote Docker socket

### Clone the Course Repo
```bash
git clone <course-repo-url>
```

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
