# ☁️ Nextcloud

## 📌 Overview

This is my deployment of Nextcloud in my homelab.

The goal here was to set up a self-hosted cloud storage system (similar to Google Drive) while learning how to run a **stateful, multi-container application** using Docker.

---

## 🎯 Why I deployed this

I wanted to:

* Work with a real-world application that uses a database
* Understand how persistent storage works in Docker
* Practice troubleshooting when things don’t work as expected
* Build something useful that I can actually use (file storage)

---

## 🧱 Architecture

```text
Nextcloud (app)
   ↓
MariaDB (database)
   ↓
/mnt/appdata (persistent storage)
```

---

## ⚙️ Setup

### 1. Create directories

```bash
sudo mkdir -p /opt/stacks/nextcloud
sudo mkdir -p /mnt/appdata/nextcloud/html
sudo mkdir -p /mnt/appdata/nextcloud/db
```

---

### 2. Create `.env` file

This stores database credentials and admin login info.

```bash
nano /opt/stacks/nextcloud/.env
```

---

### 3. Create docker-compose.yml

* Nextcloud container (web app)
* MariaDB container (database)
* Mounted volumes for persistence

---

### 4. Start containers

```bash
docker compose up -d
```

---

### 5. Access web UI

```text
http://<pi-ip>:8081
```

---

## ⚠️ Issue I ran into

After deployment, I **couldn’t log in** even though I set admin credentials in the `.env` file.

* Login kept redirecting back to the login page
* No clear error in the UI

---

## 🔍 What I found

From logs:

* Database connection failed initially:

  ```text
  SQLSTATE[HY000] [2002] Connection refused
  ```
* Nextcloud retried installation after the database came online
* Admin user was created incorrectly

---

## ✅ Fix

Checked users inside the container:

```bash
docker exec -it nextcloud-app php occ user:list
```

Found an unexpected username.

Reset password:

```bash
docker exec -it nextcloud-app php occ user:resetpassword <username>
```

Then logged in successfully.

---

## 🧼 Cleanup

* Created a new admin user (`anthony`)
* Removed the incorrect user

---

## 🧠 What I learned

* `depends_on` does NOT mean “ready”, just “started”
* Stateful apps are more complex than simple containers
* Logs are critical when troubleshooting Docker issues
* CLI tools like `occ` are very useful for managing Nextcloud

---

## 🚀 Next Steps

* Add reverse proxy (Traefik)
* Set up HTTPS with my domain
* Add Redis for performance
* Implement backups

---

## 🔐 Notes

* `.env` file is stored locally on the Raspberry Pi
* `.env` is excluded from GitHub using `.gitignore`
* `.env.example` is used in the repo for reference

---

## 🏁 Status

✅ Deployment successful
✅ Admin access working
⚙️ Ready for reverse proxy integration
