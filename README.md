# Debian-Cloud-Lab


This method uses LinuxServer Webtop to provide a browser-accessible Linux desktop.

## Prerequisites

* GitHub Codespace
* Docker installed and running

Verify Docker:

```bash
docker --version
docker compose version
```

---

## Create Project Directory

```bash
mkdir ~/webtop
cd ~/webtop
```

---

## Create docker-compose.yml

```yaml
services:
  desktop:
    image: lscr.io/linuxserver/webtop:debian-xfce
    container_name: desktop
    privileged: true

    environment:
      - TZ=Asia/Kolkata

    ports:
      - "6080:3000"

    volumes:
      - ./config:/config

    shm_size: "2gb"

    restart: unless-stopped
```

---

## Start Container

```bash
docker compose up -d
```

Verify:

```bash
docker ps
```

Expected:

```text
desktop    Up
```

---

## Forward Port

Open the Codespaces **Ports** tab.

Forward:

```text
6080
```

Open the generated URL.

Example:

```text
https://your-codespace-6080.app.github.dev
```

---

## Access Desktop

The XFCE desktop should load directly in the browser.

No VNC setup is required.

---

## Enter Container

```bash
docker exec -it desktop bash
```

---

## Update System

Inside the container:

```bash
apt update
```

---

## Install Common Tools

```bash
apt install -y \
git \
curl \
wget \
python3 \
python3-pip \
python3-venv \
nmap
```

---

## Install SpiderFoot

```bash
cd /opt

git clone https://github.com/smicallef/spiderfoot.git

cd spiderfoot

python3 -m venv venv

source venv/bin/activate

pip install -r requirements.txt
```

Run SpiderFoot:

```bash
python sf.py -l 0.0.0.0:5001
```

Expose port:

```text
5001
```

and open the forwarded URL.

---

## Stop Container

```bash
docker compose down
```

---

## Start Existing Container

```bash
docker compose up -d
```

---

## View Logs

```bash
docker logs -f desktop
```

---

## Remove Container

```bash
docker compose down

docker rm -f desktop
```

---

## Notes

* The Webtop image is Debian XFCE, not Kali Linux.
* Most Kali tools can be installed manually using apt.
* Configuration stored in ./config persists across container restarts.
* This approach is generally easier and more stable than manually configuring XFCE + VNC + noVNC.
