# Commands Used - Nextcloud

## 📁 Navigate to project directory

```bash
cd /opt/stacks/nextcloud
```

---

## 📦 Create required directories

```bash
sudo mkdir -p /opt/stacks/nextcloud
sudo mkdir -p /mnt/appdata/nextcloud/html
sudo mkdir -p /mnt/appdata/nextcloud/db
```

---

## ✏️ Create/Edit files

### docker-compose.yml

```bash
nano docker-compose.yml
```

### .env file

```bash
nano .env
```

---

## 🚀 Start Nextcloud stack

```bash
docker compose up -d
```

---

## 📊 Check running containers

```bash
docker ps
```

---

## 📜 View logs

### Nextcloud app logs

```bash
docker compose logs app
```

### Database logs

```bash
docker compose logs db
```

### Tail recent logs

```bash
docker compose logs app --tail=80
docker compose logs db --tail=80
```

---

## 👤 User management (Nextcloud CLI)

### List users

```bash
docker exec -it nextcloud-app php occ user:list
```

### Reset password

```bash
docker exec -it nextcloud-app php occ user:resetpassword <username>
```

---

## 🔍 Troubleshooting

### Check environment variables inside container

```bash
docker exec -it nextcloud-app printenv | grep NEXTCLOUD
```

### Render full docker-compose config

```bash
docker compose config
```

---

## 🔄 Restart / Stop services

### Restart containers

```bash
docker compose restart
```

### Stop containers

```bash
docker compose down
```

### Start again

```bash
docker compose up -d
```

---

## 🧪 Verify access

```text
http://<pi-ip>:8081
```

---

## 📌 Git commands (before pushing)

```bash
git status
```

---

## 🔐 Notes

* `.env` file is stored locally on the Raspberry Pi
* `.env` is excluded from GitHub using `.gitignore`
* `.env.example` is used in the repo for reference
