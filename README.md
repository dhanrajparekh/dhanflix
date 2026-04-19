# dhanflix

Personal home media server stack running on Docker. Streams to any device via `watch.dhanrajparekh.com`.

## Stack

| Service | Purpose | Port |
|---------|---------|------|
| Jellyfin | Media server / streaming | 8096 |
| Radarr | Movie library manager | 7878 |
| Sonarr | TV show library manager | 8989 |
| Prowlarr | Indexer manager | 9696 |
| qBittorrent | Download client | 8080 |
| Jellyseerr | Request interface | 5055 |
| Cloudflared | Cloudflare tunnel for public access | — |

## How it works

1. Request a movie on Jellyseerr (iOS/Android app or `http://localhost:5055`)
2. Jellyseerr sends it to Radarr
3. Radarr finds and downloads it automatically
4. Jellyfin picks it up and makes it available to stream
5. Watch on any device via `watch.dhanrajparekh.com` or local network

## Setup

### Prerequisites
- Docker Desktop
- Cloudflare account with a domain and tunnel configured

### First time

```bash
git clone https://github.com/dhanrajparekh/dhanflix
cd dhanflix
cp .env.example .env
```

Edit `.env` and fill in your Cloudflare tunnel token:

```
CLOUDFLARE_TUNNEL_TOKEN=your_token_here
```

Then start everything:

```bash
docker compose up -d
```

### Folder structure

```
dhanflix/
  data/
    media/
      movies/
      tv/
    downloads/
  config/
  cache/
  radarr-config/
  sonarr-config/
  prowlarr-config/
  qbittorrent-config/
  jellyseerr-config/
```

## Accessing services

All internal services are bound to `127.0.0.1` — only accessible from the host machine.

| Service | URL |
|---------|-----|
| Jellyfin | http://localhost:8096 |
| Radarr | http://localhost:7878 |
| Sonarr | http://localhost:8989 |
| Prowlarr | http://localhost:9696 |
| qBittorrent | http://localhost:8080 |
| Jellyseerr | http://localhost:5055 |
| Public | https://watch.dhanrajparekh.com |

## Phone apps

- **Jellyseerr** — request movies from your phone (iOS/Android)
- **Ruddarr** — monitor and manage your library (iOS)

Connect using your machine's local IP (e.g. `http://192.168.1.x:7878`) — both devices must be on the same network.

## Moving to a NAS

```bash
git clone https://github.com/dhanrajparekh/dhanflix
cd dhanflix
cp .env.example .env
# fill in Cloudflare token
docker compose up -d
```

Then update the Cloudflare tunnel ingress to point to the new machine's IP.

## Security

- `.env` is gitignored — never committed
- Internal services only accessible from localhost
- Only Jellyfin is publicly exposed via Cloudflare tunnel
- No ports exposed directly to the internet
