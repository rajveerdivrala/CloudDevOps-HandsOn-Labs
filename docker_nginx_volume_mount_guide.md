# Hosting a Static Website Using Docker Nginx & Volume Mounting

This guide explains how to host a static website (such as a digital marketing agency webpage) inside a containerized **Nginx** web server using **Docker port mapping**, **detached mode**, and **volume (bind) mounting** on a Linux VM.

---

## Architecture Overview

- **Host VM Directory:** `/home/ravi/docker-nginx/html` (stores your local `index.html` file)
- **Container Directory:** `/usr/share/nginx/html` (default document root where Nginx looks for web files)
- **Port Mapping:** Host VM port `80` $\rightarrow$ Container port `80`
- **Container Name:** `ravi` (optional `--name` flag for easy identification)
- **Execution Mode:** Detached (`-d`) running in the background

---

## Step-by-Step Guide

### Step 1: Create the Directory Structure on the Host VM
Use `mkdir -p` to create both the parent directory (`docker-nginx`) and the subdirectory (`html`) in a single command. The `-p` flag ensures parent directories are created automatically and prevents errors if the folder already exists.

```bash
mkdir -p /home/ravi/docker-nginx/html
```

Navigate into the newly created directory:

```bash
cd /home/ravi/docker-nginx/html
```

---

### Step 2: Create the `index.html` File Using `vi`
Create and open `index.html` inside `/home/ravi/docker-nginx/html`:

```bash
vi index.html
```

Add your HTML code (for example, a digital marketing agency landing page):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Digital Marketing Agency</title>
</head>
<body>
  <h1>Welcome to Our Digital Marketing Agency</h1>
  <p>Hosted via Nginx inside a Docker container using volume mounting.</p>
</body>
</html>
```

Save and exit `vi`:
1. Press `Esc`
2. Type `:wq` and press `Enter`

---

### Step 3: Run the Nginx Container with Port Mapping and Volume Mount
Run the following command to pull the `nginx` image (if not already available locally) and launch the container:

```bash
docker run --name ravi -d -p 80:80 -v /home/ravi/docker-nginx/html:/usr/share/nginx/html nginx
```

*(Note: If you omit `--name ravi`, Docker automatically assigns a random container name.)*

---

## Command Breakdown

| Flag / Argument | Purpose |
| :--- | :--- |
| `docker run` | Creates and starts a new container from the specified Docker image. |
| `--name ravi` | Assigns a custom name (`ravi`) to the container for easier tracking and management. |
| `-d` | Runs the container in **detached mode** (in the background) so your terminal remains free. |
| `-p 80:80` | **Port mapping** (`<HOST_PORT>:<CONTAINER_PORT>`). Maps port `80` on the Host VM to port `80` inside the Nginx container. |
| `-v /home/ravi/docker-nginx/html:/usr/share/nginx/html` | **Volume (bind) mount**. Mounts the host directory onto Nginx's document root so Nginx serves your host's `index.html` file without modifying the base image. |
| `nginx` | The official Docker image used to create the container. |

---

## Step 4: Verify the Deployment

1. **Confirm the container is running:**
   ```bash
   docker ps
   ```
   Verify that the container status shows `Up` and the ports show `0.0.0.0:80->80/tcp`.

2. **Access the website in a browser:**
   Open your browser and visit:
   ```text
   http://<VM-PUBLIC-IP>:80
   ```
   *(Ensure that inbound traffic on TCP port `80` is allowed in your cloud VM's firewall or security group.)*

---

## Useful Container Management Commands

- **View container logs:**
  ```bash
  docker logs ravi
  ```

- **Stop the background container:**
  ```bash
  docker stop ravi
  ```

- **Start a stopped container again:**
  ```bash
  docker start ravi
  ```

- **Remove the container (after stopping):**
  ```bash
  docker rm ravi
  ```

---

## Key Takeaways

- **Live Updates Without Rebuilding:** Any edits saved to `/home/ravi/docker-nginx/html/index.html` on the Host VM are immediately served by Nginx upon refreshing the browser.
- **Base Image Remains Clean:** Volume mounting overlays the host folder onto `/usr/share/nginx/html` at runtime without permanently altering the underlying `nginx` Docker image.
