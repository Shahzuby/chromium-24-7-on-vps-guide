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


## 🔧 2. System Update

```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

---

## 🐳 3. Install Docker (Official Method)

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

## 🌍 4. Check Your Timezone

```bash
realpath --relative-to /usr/share/zoneinfo /etc/localtime
```

---

## 📁 5. Set Up Chromium Folder

```bash
mkdir chromium
cd chromium
```

---

## 📝 6. Create docker-compose.yaml

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
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER=     #Replace username
      - PASSWORD=    #Replace password
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - CHROME_CLI=https://google.com
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   #Change 3010 to your favorite port if needed
      - 3011:3001   #Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
```

Then press: `CTRL + X`, `Y`, and `ENTER`

---

## 🚀 7. Start Chromium

```bash
cd $HOME && cd chromium

docker compose up -d
```

Then visit in your browser:

- http://your-vps-ip:3010/
- https://your-vps-ip:3011/

---

## 🛠️ 8. Fix Docker Permissions (Optional Error Fix)

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
