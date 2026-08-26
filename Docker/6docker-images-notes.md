# Docker Container Images — Notes

## What Is an Image?

An image contains:
- App binaries and dependencies
- Metadata about the image data and how to run it

**Official definition:** An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime.

Key points:
- Not a complete OS — no kernel or kernel modules (e.g. drivers)
- Can be as small as a single file, like a Golang static binary
- Can be as large as a full Ubuntu distro with `apt` and other tooling

---

## The Docker Hub Registry

### Basics
- Docker Hub is the default public registry for Docker images
- You can find **official** images (maintained by Docker/vendors) and other public images
- Download images using tags to pick specific versions

### Commands
```bash
docker image ls
docker pull nginx
docker pull nginx:1.11
```

---

## Image Layers

Images and their layers use a **union file system**, which lets Docker cache and reuse layers efficiently.

Key concepts:
- **Layers** — each layer represents a filesystem change
- **Union file system** — combines layers into a single view
- **Copy-on-write** — containers only write changes on top of the image, not into it
- **History & inspect commands** — let you examine how an image was built

### Commands
```bash
docker image ls
docker history nginx:latest
docker image inspect nginx:latest
```

### Example: Shared Base Layers
```
Container 1        Container 2 (Top: Apache image)
     |                    |
     +------ Apache Image-+
```

### Key Facts
- Images are made up of filesystem changes **and** metadata
- Each layer is uniquely identified and stored only once on a host
  - Saves storage space on the host
  - Saves transfer time on push/pull
- A **container** is just a single read/write layer on top of an image
- `docker image history` and `docker image inspect` reveal how an image was built

---

## Image Tagging and Pushing to Docker Hub

### Prerequisites
- Understand what a container and an image are
- Understand image layer basics
- Understand Docker Hub basics

### Topics Covered
- Image tags
- Uploading (pushing) to Docker Hub
- Image ID vs. Tag

### Image ID vs. Tag
Run `docker image ls` to see the table:

| REPOSITORY | TAG | IMAGE ID | CREATED | SIZE |
|---|---|---|---|---|

- **Repository** = `username/repository-name` or `organization/repository-name`
- **Tag** = a label/version pointing to a specific image (e.g. `latest`, `1.11`, `mainline`)
- **Image ID** = the unique underlying content hash

### Commands
```bash
docker image tag --help

docker pull mysql/mysql-server
docker pull nginx:mainline
docker image ls

# Tag an existing image under a new repository name
docker image tag nginx bretfisher/nginx
docker image ls

# Push to Docker Hub (uploads changed layers only)
docker login
docker image push bretfisher/nginx

# Tag with a specific version/tag name
docker image tag bretfisher/nginx bretfisher/nginx:testing
docker image push bretfisher/nginx:testing
docker image ls
```

---

## Building Images: The Dockerfile Basics

### Running a Docker Build
```bash
docker image build -t customnginx .
docker image ls
```

### Extending Official Images
Example: extending the official `nginx` image with a `Dockerfile`.

```bash
# Edit the Dockerfile
vim Dockerfile

# Run the base nginx image for reference
docker container run -p 80:80 --rm nginx
```

**Sample Dockerfile:**
```dockerfile
FROM nginx:latest
WORKDIR /usr/share/nginx/html
COPY index.html index.html
```

Build and run:
```bash
docker image build -t nginx-with-html .
docker container run -p 80:80 --rm nginx-with-html
docker image ls
```

### Tagging the Custom Image for Docker Hub
```bash
docker image tag --help
docker image tag nginx-with-html:latest bretfisher/nginx-with-html:latest
```
