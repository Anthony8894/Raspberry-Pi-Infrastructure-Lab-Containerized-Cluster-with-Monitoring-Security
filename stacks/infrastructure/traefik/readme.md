# Traefik Reverse Proxy

This folder contains the Docker Compose setup for Traefik, which I use as the reverse proxy for my homelab.

Traefik allows me to route public subdomains like:

- `grafana.anthonylearchive.com`
- `traefik.anthonylearchive.com`
- future services like `nextcloud.anthonylearchive.com`

to the correct Docker containers running inside my homelab.

## What Traefik Does

Traefik sits in front of my Docker services and handles incoming web traffic on ports `80` and `443`.

Instead of accessing services by IP address and port, like:

```text
http://192.168.50.199:3000

```

I can acess them through: grafana.anthonylearchive.com
In addition to that, login credentials will have to be provided before entering. 

