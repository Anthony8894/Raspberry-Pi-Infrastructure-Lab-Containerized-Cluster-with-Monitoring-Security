# Jellyfin Media Server

## Overview

Jellyfin is a self-hosted media server used to organize and stream movies, TV shows, and other media from the homelab. Generally accepts mp4 files.

In this setup, Jellyfin runs as a Docker container on `pi-worker1` and stores its media files under `/mnt/media`.

This project helps me practice:

- Docker Compose
- Linux directory structure
- Bind mounts
- Self-hosted applications
- Media storage planning
- Homelab service documentation

## Folder Structure

```text
/opt/stacks/jellyfin = stores the actual stacl/docker compose file
├── docker-compose.yml

/mnt/appdata/jellyfin = stores cache data and config
├── config
└── cache

/mnt/media = media folder, expands into movies and tv-shows.
├── movies
└── tv-shows