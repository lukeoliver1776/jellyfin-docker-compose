# Jellyfin Docker Compose — Bare, Non-GPU, and NVIDIA GPU

This repository provides three Docker Compose configurations for running [Jellyfin](https://jellyfin.org). Choose the one that fits your system:

- **Bare** — minimal, safe defaults (no `.env`, no healthcheck).  
- **Non-GPU** — recommended default with `.env`, healthcheck, and logging limits.  
- **GPU (NVIDIA)** — same as Non-GPU plus NVIDIA hardware acceleration.  

---

## Files

- `compose/docker-compose.bare.yml` — minimal, pinned image, persistent volumes, restart policy.  
- `compose/docker-compose.nogpu.yml` — safe default (no GPU), `.env` for PUID/PGID/port/TZ, healthcheck, log limits, read-only media.  
- `compose/docker-compose.gpu.yml` — Non-GPU plus NVIDIA runtime/env for hardware acceleration.  
- `.env.example` — sample environment file.  
- `.gitignore` — ignores persistent data and local `.env`.  

---

## Prerequisites

- Docker Engine + Docker Compose Plugin installed.  
- Create these host folders for persistent Jellyfin data:  
  ```bash
  ./config ./cache ./media ./theme ./guide
  ```

**For GPU variant**:
- NVIDIA GPU + drivers  
- [nvidia-container-toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)  

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
Access Jellyfin: [http://YOURHOST:8096](http://YOURHOST:8096)

---

### Option B — Non-GPU (recommended)
```bash
cp .env.example .env
# edit PUID/PGID/JELLYFIN_PORT/TZ/PUBLISHED_URL as needed
docker compose -f compose/docker-compose.nogpu.yml up -d
```

---

### Option C — NVIDIA GPU
```bash
cp .env.example .env
# leave NVIDIA_* defaults or set specific GPU IDs
docker compose -f compose/docker-compose.gpu.yml up -d
```

---

### Check Container Status
```bash
docker compose -f compose/docker-compose.nogpu.yml ps
```

Example:
```
NAME       IMAGE                     SERVICE    STATUS              PORTS
jellyfin   jellyfin/jellyfin:10.9.7  jellyfin   running (healthy)   0.0.0.0:8096->8096/tcp
```

---

## 📑 Compose Variants in Detail

### 1. Bare
- File: `compose/docker-compose.bare.yml`  
- Pinned Jellyfin image  
- Volumes: `./config`, `./cache`, `./media:ro`  
- Port 8096 exposed  
- Restart policy: `unless-stopped`  

---

### 2. Non-GPU
- File: `compose/docker-compose.nogpu.yml`  
- `.env` variables for PUID, PGID, JELLYFIN_PORT, TZ, optional PUBLISHED_URL  
- Healthcheck probes `http://localhost:8096`  
- Log rotation (max-size, max-file)  
- `./media` mounted read-only  

---

### 3. NVIDIA GPU
- File: `compose/docker-compose.gpu.yml`  
- Same as Non-GPU  
- Adds NVIDIA runtime and env vars:
  - `NVIDIA_VISIBLE_DEVICES` (default `all`)  
  - `NVIDIA_DRIVER_CAPABILITIES` (default `all`)  

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
