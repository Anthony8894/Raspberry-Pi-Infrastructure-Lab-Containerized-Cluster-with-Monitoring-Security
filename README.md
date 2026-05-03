# Raspberry Pi Infrastructure Lab – Containerized Cluster with Monitoring & Security

This repository documents my Raspberry Pi home lab running Ubuntu Server.  
The goal of this project is to simulate a small production-style environment using containerization, monitoring, and security tools commonly used in real IT environments.

This lab is hands-on and continuously evolving. I’m using it to learn, test, and document real-world concepts while building something I can also use as a portfolio project.

---

## 💡 Why This Project Matters

I wanted something beyond basic labs where I could actually build and manage systems over time.

Through this project, I’m working on:
- Managing Linux servers
- Running services across multiple nodes
- Setting up monitoring and observability
- Improving my troubleshooting and documentation skills

These are all things I’ll need for roles like system administration or cloud/infrastructure.

---

## 🎯 Goals

- Get more comfortable with **Linux system administration**
- Build a **multi-node containerized environment**
- Learn how monitoring, logging, and alerting work
- Practice documenting everything clearly
- Gain experience relevant to **sysadmin, cloud, and security roles**

---

## 🚧 Current Status

- ✅ Docker Swarm cluster is up and running
- ✅ Portainer installed for management
- ✅ Node Exporter running on both nodes
- ⚙️ Prometheus & Grafana (working on it)
- ⚙️ Reverse proxy with Traefik (in progress)
- 📌 Planning to add logging and SIEM later (Wazuh)

> Still actively working on this and adding more over time.

---

## 🧱 Cluster Overview

- **Nodes:**
  - `pi-main` (Manager)
  - `pi-worker` (Worker)

- **OS:** Ubuntu Server LTS  
- **Architecture:** ARM64 (Raspberry Pi)

The idea here is to simulate a small production setup with:
- Separate roles (manager/worker)
- Container orchestration
- Monitoring across nodes

---

## 🧰 Technologies & Tools

### Infrastructure & Containers
- Docker Engine
- Docker Swarm
- Portainer CE

### Monitoring & Observability
- Prometheus (in progress)
- Grafana (in progress)
- Node Exporter
- cAdvisor
- Uptime Kuma

### Security
- Wazuh SIEM (planned)
- Log collection and alerting (planned)

### Applications
- Nextcloud (self-hosted file service)

### Networking & Access
- SSH (key-based authentication)
- UFW firewall
- Tailscale (secure remote access)

---

## 🌐 Service Access

- all of these are temporary and will change from time to time! 
- I will need to figure out a way to store these

| Service   | Access |
|----------|--------|
| Portainer | http://192.168.50.199:9000 |
| Grafana   | (coming soon via reverse proxy) |
| Traefik   | (in progress) |

---

## 🗺️ Architecture

Diagrams are available in the `architecture/` folder.

Main ideas:
- Multi-node Docker Swarm
- Overlay networking
- Centralized monitoring setup
- Persistent storage for services

---

## 🔮 Planned Infrastructure (Next Phase)

As I keep building this out, I want to expand into a more complete setup:

- 🧠 **Proxmox Host**
  - Run on dedicated hardware (likely repurposed device)
  - Use it to host VMs for testing and services

- 🔥 **Firewall Appliance**
  - pfSense or OPNsense
  - Learn VLANs, segmentation, and network security

- 🌐 **Managed Switch (In Progress)**
  - Already have the hardware
  - Planning to set up VLANs and improve network structure

- 💻 **Windows / Active Directory**
  - Simulate an enterprise environment
  - Practice domain services and group policy

---

## 🧠 What I’m Learning

- Linux administration (Ubuntu Server)
- Docker and container orchestration
- Networking basics (ports, routing, reverse proxy concepts)
- Monitoring and observability
- Security basics (SSH, firewall, VPN)
- Troubleshooting real systems instead of just following tutorials

---

## 📂 Repository Structure

```text
pi-home-cluster/
├── README.md
├── architecture/
│   ├── network-diagram.png
│   ├── service-diagram.png
│   └── cluster-overview.md
├── docs/
│   ├── 01-initial-setup.md
│   ├── 02-networking.md
│   ├── 03-docker-portainer.md
│   ├── 04-docker-swarm.md
│   ├── 05-reverse-proxy.md
│   ├── 06-monitoring.md
│   ├── 07-security.md
│   ├── 08-backups.md
│   └── 09-troubleshooting.md
├── stacks/
│   ├── infrastructure/
│   ├── monitoring/
│   ├── security/
│   └── apps/
├── screenshots/
