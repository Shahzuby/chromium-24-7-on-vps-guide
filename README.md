# 🧭 Chromium in Docker Guide

This guide sets up a Chromium browser inside a Docker container accessible via browser over custom ports. It works on any VPS If you're using Azure, AWS, or any other VPS provider, make sure to open the required ports (3010 and 3011) in your firewall or security group settings.
Without opening these ports, you won’t be able to access Chromium in your browser.

---

## ☁️ 1. Google Cloud Firewall Setup 

If you're using **Google Cloud**, follow these steps:

1. Go to: https://console.cloud.google.com
2. Navigate to: `VPC Network` > `Firewall rules`
3. Click: **"Create Firewall Rule"**
4. Fill in:
   - **Name:** `allow-chromium`
   - **Network:** `default`
   - **Targets:** All instances (or use tags)
   - **Source IP Ranges:** `0.0.0.0/0`
   - **Protocols & Ports:** TCP → `3010,3011`
5. Click: **Create**

---

## 🐳 2. Install Docker (Official Method)

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Docker version check
docker --version
```

---

## 🌍 3. Check Your Timezone

```bash
realpath --relative-to /usr/share/zoneinfo /etc/localtime
```

---

## 📁 4. Set Up Chromium Folder

```bash
mkdir chromium
cd chromium
```

---

## 📝 5. Create docker-compose.yaml

```bash
nano docker-compose.yaml
```

Paste the following configuration (edit `CUSTOM_USER`, `PASSWORD`, `TZ`, `CHROME_CLI` if needed):

```yaml
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined
    environment:
      - CUSTOM_USER=youruser
      - PASSWORD=yourpass
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
      - CHROME_CLI=about:blank
    volumes:
      - /root/chromium/config:/config
    ports:
      - "3011:3001"  # HTTPS only
    shm_size: "2gb"
    restart: unless-stopped
```

Then press: `CTRL + X`, `Y`, and `ENTER`

---

## 🚀 6. Start Chromium

```bash
sudo docker compose up -d
```

Then visit in your browser:

- http://your-vps-ip:3010/
- https://your-vps-ip:3011/

---

## 🛠️ 7. Fix Docker Permissions (Optional Error Fix)

```bash
sudo usermod -aG docker $USER
```

Then **restart your VPS**.

---

## 🧼 Optional: Stop & Remove Chromium

```bash
docker stop chromium
docker rm chromium
docker system prune
```

---

follow: https://t.me/andhiiTGkamaii for more stuffs



## 🔗 Referral Link

Join Orochi Network: [https://onprover.orochi.network/?referralCode=Oo07_magiceden](https://onprover.orochi.network/?referralCode=Oo07_magiceden)
