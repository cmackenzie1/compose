# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Docker Compose stack repository for hosting containerized services on a home server. The repository is organized into separate stacks, each with its own docker-compose.yaml file:

- **servarr/**: Media management stack (Sonarr, Radarr, Prowlarr, qBittorrent with VPN)
- **monitoring/**: Observability stack (Prometheus, Grafana)

## Architecture

### Storage Strategy
- Uses Docker volumes instead of bind mounts (stored on NVMe drive at `/mnt/nvme0/docker`)
- Volume naming convention: `<container>-data`
- Config data stored in `/mnt/nvme0/containers/<service>/`
- Media data mounted at `/mnt/media/data/` with structure:
  ```
  /mnt/media/data/
  ├── movies
  ├── torrents
  └── tv
  ```

### Network Architecture
- **servarr stack**: qBittorrent runs through Gluetun VPN container using `network_mode: "service:gluetun"`
- **monitoring stack**: Standalone services with Grafana configured to use Prometheus as default datasource

## Common Commands

### Managing Stacks
```bash
# Navigate to a specific stack
cd servarr/  # or monitoring/

# Pull latest images and start services
docker compose pull
docker compose up -d

# View logs
docker compose logs -f [service_name]

# Stop services
docker compose down
```

### System Maintenance
```bash
# Clean up Docker resources (run from repository root)
docker system prune -a
docker image prune -a
docker system prune -a
docker system df

# Remove unused volumes (be careful - only run when stacks are down)
docker volume prune -a
```

### Environment Variables
The servarr stack requires these environment variables (typically in `.env` file):
- `PUID` - User ID for file permissions
- `PGID` - Group ID for file permissions
- `TZ` - Timezone
- `VPN_SERVICE_PROVIDER` - VPN provider for Gluetun
- `OPENVPN_USERNAME` - VPN username
- `OPENVPN_PASSWORD` - VPN password

## Service Access

### Default Ports
- **Grafana**: http://localhost:3000 (admin/grafana)
- **Prometheus**: http://localhost:9090
- **Sonarr**: http://localhost:8989
- **Radarr**: http://localhost:7878
- **Prowlarr**: http://localhost:9696
- **qBittorrent**: http://localhost:8080 (via Gluetun)

## Docker Configuration

The repository includes `daemon.json` with optimized settings:
- Data root: `/mnt/nvme0/docker`
- Log rotation: 10MB max, 3 files
- JSON file logging driver
