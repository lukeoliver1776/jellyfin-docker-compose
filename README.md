# Jellyfin Docker Compose — Bare, Non-GPU, and NVIDIA GPU

This repository provides three Docker Compose configurations for running [Jellyfin](https://jellyfin.org). Choose the one that fits your system:

---

## Files

- `compose/docker-compose.bare.yml`
- `compose/docker-compose.nogpu.yml` (Or use the official Jellyfin compose [here](https://jellyfin.org/docs/general/installation/container).)
- `compose/docker-compose.gpu.yml`
- `.env.example`

---

## Prerequisites

- Docker Engine + docker-compose plugin.
- Your choice of docker compose file.
---

## Quick Start

Clone and enter the repo:
```bash
git clone https://github.com/YOURNAME/jellyfin-docker-compose.git
cd jellyfin-docker-compose
```

### Option A — Bare (minimal)
```bash
docker compose -f compose/docker-compose.bare.yml up -d
```
Access Jellyfin: [http://YOURHOST:<JELLYFIN_PORT>](http://YOURHOST:<JELLYFIN_PORT>)

---

### Option B — Non-GPU (recommended)
```bash
cp .env.example .env
# edit PUID/PGID/JELLYFIN_PORT/TZ/PUBLISHED_URL as needed
docker compose -f compose/docker-compose.nogpu.yml up -d
```

---

### Option C — NVIDIA GPU

> Prerequisite: NVIDIA drivers and [nvidia-container-toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) must be installed on the host.

```bash
cp .env.example .env
# leave NVIDIA_* defaults or set specific GPU IDs
docker compose -f compose/docker-compose.gpu.yml up -d
```

---

### Check Container Status
```bash
docker ps
```

Example:
```
NAME       IMAGE                     SERVICE    STATUS              PORTS
jellyfin   jellyfin/jellyfin:10.9.7  jellyfin   running (healthy)   0.0.0.0:8096->8096/tcp
```

---

## 🔧 Environment Variables

Copy example:
```bash
cp .env.example .env
```

Example `.env`:
```ini
PUID=1000
PGID=1000
JELLYFIN_PORT=8096
TZ=America/New_York
PUBLISHED_URL=
NVIDIA_VISIBLE_DEVICES=all
NVIDIA_DRIVER_CAPABILITIES=all
```

- **PUID/PGID** — run Jellyfin as host user so files aren’t owned by root.  
