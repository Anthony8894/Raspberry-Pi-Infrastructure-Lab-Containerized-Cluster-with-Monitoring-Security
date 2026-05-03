# Jellyfin Media Server

## Overview

What is Jellyfin? it's a self-hosted media server used to organize and stream movies, TV shows, and other media from the homelab.

In this setup, Jellyfin runs as a Docker container on my `pi-worker1` and stores its media files under `/mnt/media`.

## Why This Matters

This project helps me practice:

- Docker Compose
- Linux directory structure
- Bind mounts
- Self-hosted applications
- Media storage planning
- Homelab service documentation

## Folder Structure

```text
/opt/stacks/jellyfin
├── docker-compose.yml

/mnt/appdata/jellyfin
├── config
└── cache

/mnt/media
├── movies
└── tv-shows