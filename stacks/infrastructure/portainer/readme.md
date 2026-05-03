# Portainer (Container Management UI)

## 📌 Overview

Portainer is a lightweight management UI that allows me to manage Docker containers, images, volumes, and networks through a web interface instead of using only CLI commands.

In this homelab, Portainer is used to:

* Monitor running containers
* Deploy and manage stacks
* View logs and container stats
* Manage my Docker Swarm cluster

---

## 🎯 Why This Matters

In real-world environments, not everything is managed purely through CLI.

Portainer simulates:

* Centralized container management
* Easier troubleshooting and visibility
* A UI-based approach similar to enterprise tools

This helps bridge the gap between:
👉 learning Docker
👉 operating production-like environments

---

## 🧱 Deployment Details

### Environment

* Platform: Raspberry Pi (ARM64)
* OS: Ubuntu Server
* Container Runtime: Docker Engine
* Orchestration: Docker Swarm

---

## ⚙️ Docker Compose / Stack

```yaml
version: "3.8"

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    ports:
      - "9000:9000"
      - "9443:9443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
    restart: always

volumes:
  portainer_data:
```

---

## 🚀 Commands Used

### Deploy Portainer

```bash
docker volume create portainer_data

docker run -d \
  -p 9000:9000 \
  -p 9443:9443 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

---

### Check running containers

```bash
docker ps
```

---

### View logs

```bash
docker logs portainer
```

---

## 🌐 Access

Portainer is accessible via:

```
http://<server-ip>:9000
```

Example:

```
http://192.168.50.199:9000
```

---

## 🔐 Initial Setup

1. Open the web UI
2. Create admin credentials
3. Select environment:

   * Docker
   * Agent (for Swarm)

---

## 🧠 What I Learned

* How to expose containerized services via ports
* Managing Docker through both CLI and UI
* Understanding container relationships (images, volumes, networks)
* Basics of Docker Swarm visibility

---

## 🔧 Future Improvements

* Expose Portainer via Traefik (HTTPS)
* Add authentication middleware
* Restrict access (internal network or VPN only)
* Integrate with monitoring stack

---

## 📁 Related Services

* Traefik (reverse proxy)
* Docker Swarm
* Jellyfin
* Nextcloud (planned)

---

## 📝 Notes

Portainer is used as a management tool, not a replacement for learning Docker CLI.

All deployments are still validated using CLI commands for deeper understanding.
