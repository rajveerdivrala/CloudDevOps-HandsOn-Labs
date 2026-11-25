# Docker Mental Model: Images, Containers, Ports, Detached Mode & Volumes

A practical summary of how Docker images, containers, port mapping, detached execution, and volume mounts work together in day-to-day practice.

---

## 1. Docker Image vs. Docker Container

| Concept | Role | Persistence & Lifecycle |
| :--- | :--- | :--- |
| **Docker Image** | Read-only template / blueprint (base OS, runtime/web server binary, default configs) | Stays intact on the host and can be used to create multiple containers |
| **Docker Container** | Isolated running instance created from an image | Has its own writable layer; `docker stop` pauses it, `docker rm` deletes it and its container-layer changes |

- **Container vs. Virtual Machine (VM):** A container provides an isolated environment on top of the host's OS kernel rather than running a full separate guest OS with virtualized hardware like a traditional VM.
- **Image changes:** Working inside a container or deleting a container does not modify the base image itself.

---

## 2. Core Flags at a Glance

- **`-p <host_port>:<container_port>` (Port Mapping):** Maps a port on the host VM to a port inside the container so traffic can reach the service.
- **`-d` (Detached Mode):** Runs the container (or an `exec` command) in the background and returns control of the terminal immediately.
- **`-v <host_path>:<container_path>` (Volume / Bind Mount):** Connects a host VM directory to a directory inside the container.

---

## 3. Why Volume Mounting Matters for Web Files & Data

- **Decoupled from container lifecycle:** Files stored on the host VM (for example, `index.html`) remain safe even if the container is stopped or removed.
- **Faster updates:** Editing files on the host directory is immediately visible to the running container without rebuilding the base image.
- **Right tool for the job:** Use host mounts/volumes for persistent data and fast local iteration; use version-controlled code and built images for repeatable deployments.

---

## 4. Essential Commands Used

```bash
# Create directory structure on the host VM
mkdir -p ~/docker-nginx/html

# Run Nginx in detached mode with port mapping and volume mount
docker run -d --name ravi -p 8080:80 -v ~/docker-nginx/html:/usr/share/nginx/html nginx

# Inspect running containers
docker ps

# Stop and remove a container when finished
docker stop ravi
docker rm ravi