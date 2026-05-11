## Nextcloud Docker Compose Breakdown

This document break down docker-compose.yml for nextcloud. 

## Overview

This setup runs Nextcloud using Docker Compose.

It has two main containers:

| Container | Purpose |
|---|---|
| `nextcloud-app` | Runs the Nextcloud web application |
| `nextcloud-db` | Runs the MariaDB database used by Nextcloud |

The setup also uses Traefik as a reverse proxy so Nextcloud can be reached through a domain name.

```text
Local access:
http://192.168.50.199:8081

Reverse proxy access:
https://nextcloud.anthonylearchive.com

```

## Architecture example

Internet
   |
   v
Namecheap DNS
   |
   v
Router Port Forwarding 80/443
   |
   v
Traefik Reverse Proxy
   |
   v
proxy Docker Network
   |
   v
Nextcloud App Container
   |
   v
nextcloud Docker Network
   |
   v
MariaDB Database Container


## docker compose breakdown (line by line)

services:

The services section is where Docker Compose defines the containers that belong to this stack.

In this setup, there are two services:

db:
app:

These become the two main containers:

nextcloud-db
nextcloud-app

Simple explanation:

services = the list of containers Docker Compose should run

reference: https://docs.docker.com/reference/compose-file/

## Database service

db
db:

The db service is the MariaDB database container.

The Nextcloud app uses this service name when connecting to the database:

MYSQL_HOST: db

Simple explanation:

Nextcloud connects to the database by using the Docker Compose service name: db

Reference:
https://docs.docker.com/reference/compose-file/
https://docs.docker.com/engine/network/


## Database Image
image: mariadb:10.11

The image line tells Docker which image to use.

In this case, Docker uses the MariaDB 10.11 image.

MariaDB is the database software used by Nextcloud.

Simple explanation:

image = the template Docker uses to create the container

Reference:
https://hub.docker.com/_/mariadb
https://docs.docker.com/reference/compose-file/



## Database Container Name
container_name: nextcloud-db

This gives the database container a readable name.

Without this, Docker Compose can generate a longer automatic name.

Simple explanation:

container_name = the name I want the running container to have

Reference:https://docs.docker.com/reference/compose-file/



## Restart Policy
restart: unless-stopped

This tells Docker to restart the container automatically if it crashes or if the Raspberry Pi reboots.

The container will only stay stopped if I manually stop it.

Simple explanation:

unless-stopped = keep this container running unless I purposely stop it

Reference:

Docker Compose file reference: https://docs.docker.com/reference/compose-file/


## MariaDB Command
command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW

This passes startup options to MariaDB.

These settings are commonly used for Nextcloud database setups.

Simple explanation:

This gives MariaDB specific settings that help it work properly with Nextcloud.

I do not need to memorize this line, but I should know that it is database tuning for the Nextcloud database.


## Database Volume
volumes:
  - /mnt/appdata/nextcloud/db:/var/lib/mysql

This maps a folder on the Raspberry Pi to a folder inside the MariaDB container.

Left side:

/mnt/appdata/nextcloud/db

This is the folder on the Raspberry Pi.

Right side:

/var/lib/mysql

This is the folder inside the MariaDB container where database files are stored.

Simple explanation:

Save the database files on the Pi, not only inside the container.

This matters because containers can be deleted and recreated. The data stored in the mounted folder stays on the Pi.


## Database Environment Variables
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
  MYSQL_PASSWORD: ${MYSQL_PASSWORD}
  MYSQL_DATABASE: ${MYSQL_DATABASE}
  MYSQL_USER: ${MYSQL_USER}

The environment section passes settings into the MariaDB container.

The ${...} values come from the .env file.

Example:

MYSQL_ROOT_PASSWORD=change_this_root_password
MYSQL_PASSWORD=change_this_database_password
MYSQL_DATABASE=nextcloud
MYSQL_USER=nextcloud

Simple explanation:

The compose file references the variables.
The .env file stores the actual values.

This keeps passwords out of the docker-compose.yml file.

## Database Network
networks:
  - nextcloud

The database only joins the nextcloud Docker network.

Simple explanation:

The database should only talk to the Nextcloud app.
It does not need to talk to Traefik or the internet.

This keeps the database private inside Docker networking.

Reference:

Docker networking overview: https://docs.docker.com/engine/network/


## App Service

app:

The app service is the actual Nextcloud web application container.

This is the container that serves the Nextcloud website.

Simple explanation:

app = the Nextcloud website container

Reference:https://hub.docker.com/_/nextcloud?utm_source=chatgpt.com
https://github.com/nextcloud/docker?utm_source=chatgpt.com


## App Image
image: nextcloud:apache

This tells Docker to use the official Nextcloud image with Apache.

Apache is the web server inside the container.

Simple explanation:

nextcloud:apache = Nextcloud with Apache included


## App Container Name
container_name: nextcloud-app

This gives the Nextcloud app container a readable name.

Simple explanation:

The running Nextcloud container will be called nextcloud-app.

Reference:

Docker Compose file reference:https://docs.docker.com/reference/compose-file/

## Ports
ports:
  - "8081:80"

This exposes Nextcloud locally on the Raspberry Pi.

Left side:

8081

This is the port on the Raspberry Pi.

Right side:

80

This is the port inside the Nextcloud container.

Simple explanation:

Raspberry Pi port 8081 goes to Nextcloud container port 80.

That is why local access works here:

http://192.168.50.199:8081

Reference:

Docker Compose file reference: https://docs.docker.com/reference/compose-file/


## App Volume
volumes:
  - /mnt/appdata/nextcloud/html:/var/www/html

This maps a folder on the Raspberry Pi to a folder inside the Nextcloud container.

Left side:

/mnt/appdata/nextcloud/html

This is the folder on the Raspberry Pi.

Right side:

/var/www/html

This is the folder inside the Nextcloud container.

Simple explanation:

Save the Nextcloud files and data on the Pi, not only inside the container.

This helps keep Nextcloud data persistent if the container is recreated.

## Database Connection Settings
MYSQL_HOST: db
MYSQL_PASSWORD: ${MYSQL_PASSWORD}
MYSQL_DATABASE: ${MYSQL_DATABASE}
MYSQL_USER: ${MYSQL_USER}

These settings tell the Nextcloud app how to connect to the MariaDB database.

MYSQL_HOST: db means the database is the Docker Compose service named db.

Simple explanation:

Nextcloud uses these values to log into MariaDB.

## Traefik Labels
Labels Section
labels:
  - "traefik.enable=true"
  - "traefik.docker.network=proxy"
  - "traefik.http.routers.nextcloud.rule=Host(`nextcloud.anthonylearchive.com`)"
  - "traefik.http.routers.nextcloud.entrypoints=websecure"
  - "traefik.http.routers.nextcloud.tls=true"
  - "traefik.http.routers.nextcloud.tls.certresolver=letsencrypt"
  - "traefik.http.services.nextcloud.loadbalancer.server.port=80"

Labels are instructions attached to the container.

Traefik reads these labels and uses them to create routing rules.

Simple explanation:

Labels tell Traefik how to expose this container through a domain name.




## What I Learned
Docker Compose

Docker Compose is the recipe that tells Docker what containers to run and how they should be configured.

Reference:

Docker Compose documentation: https://docs.docker.com/compose/
Image

An image is the template used to create a container.

Examples:

nextcloud:apache
mariadb:10.11

References:

Nextcloud official Docker image: https://hub.docker.com/_/nextcloud
MariaDB official Docker image: https://hub.docker.com/_/mariadb
Container

A container is the running version of an image.

Examples:

nextcloud-app
nextcloud-db

Reference:

Docker overview: https://docs.docker.com/get-started/docker-overview/
Volume

A volume or bind mount saves data outside the container so the data does not disappear when the container is recreated.

Reference:

Docker storage documentation: https://docs.docker.com/engine/storage/
Network

A Docker network lets containers talk to each other.

In this setup:

nextcloud network = app talks to database
proxy network = Traefik talks to app

Reference:

Docker networking overview: https://docs.docker.com/engine/network/
Environment Variables

Environment variables are settings passed into containers.

Secrets should be stored in .env, not directly in the compose file.

Reference:

Docker Compose environment variables documentation: https://docs.docker.com/compose/environment-variables/
Labels

Labels are metadata attached to containers.

In this setup, Traefik reads Docker labels to create reverse proxy routes.

Reference:

Traefik Docker provider documentation: https://doc.traefik.io/traefik/providers/docker/
Simple Summary

This Docker Compose file does the following:

1. Starts a MariaDB database container.
2. Saves the database files on the Raspberry Pi.
3. Starts a Nextcloud app container.
4. Saves the Nextcloud files on the Raspberry Pi.
5. Lets Nextcloud talk to MariaDB over a private Docker network.
6. Lets Traefik talk to Nextcloud over the proxy network.
7. Exposes Nextcloud locally on port 8081.
8. Exposes Nextcloud publicly through nextcloud.anthonylearchive.com.
9. Uses HTTPS through Traefik and Let's Encrypt.
10. Keeps passwords in a separate .env file.
Final Note

The most important part of this setup was understanding that Nextcloud uses two different networks:

nextcloud = private app-to-database traffic
proxy = Traefik-to-app reverse proxy traffic

The line that fixed the Gateway Timeout was:

- "traefik.docker.network=proxy"

That line tells Traefik:

Use the proxy network to reach Nextcloud. Do not guess.