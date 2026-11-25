# Hosting a Static Website with Docker Nginx and Volume Mounting

This guide covers how to serve a digital marketing agency's `index.html` page using an official **Nginx** Docker container, **port mapping** (`-p`), **detached mode** (`-d`), and **bind/volume mounting** (`-v`).

---

## Architecture Overview

- **Host VM Directory:** `/home/ravi/docker-nginx/html` (stores `index.html`)
- **Container Mount Target:** `/usr/share/nginx/html` (default Nginx document root)
- **Port Mapping:** Host port `8080` $\rightarrow$ Container port `80`
- **Execution Mode:** Detached (`-d`) running in the background

---

## Step-by-Step Implementation

### 1. Create the Project Directory on the Host VM
Use `mkdir -p` to create the parent and child directories in one go (and avoid errors if they already exist):

```bash
mkdir -p /home/ravi/docker-nginx/html
cd /home/ravi/docker-nginx/html