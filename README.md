# Home Media Server (Isyrr)

<div align="center">
    <img src="image/Isyrr.png" width="300">
</div>

A complete, self-hosted media server solution using Docker Compose. Stream your movies, TV shows, and music from anywhere with automated content management.

> **Note**: [Version française disponible ici](README-fr.md)

## Quick Start

```bash
# 1. Clone repository
git clone https://github.com/AhutGupta/home-media-server.git
cd home-media-server/compose_files

# 2. Configure environment
cp .env.example .env
# Edit .env with your paths

# 3. Start services
docker-compose up -d

# 4. Access dashboard
http://localhost/
```

## What's Included

### Core Services
- **Jellyfin** (8096) - Stream movies, TV shows, and personal media
- **Navidrome** (4533) - Personal music streaming server
- **nginx** (80) - Reverse proxy and dashboard

### Content Management
- **Sonarr** (8989) - Automated TV show downloads
- **Radarr** (7878) - Automated movie downloads
- **Whisparr** (6969) - Adult content management (optional)
- **Bazarr** (6767) - Automatic subtitle downloads

### Download Tools
- **qBittorrent** (8080) - Torrent client
- **Prowlarr** (9696) - Indexer manager for all *arr apps
- **Flaresolverr** (8191) - Cloudflare bypass for indexers

### Requests & Remote Access
- **Jellyseerr** (5055) - User-friendly content requests
- **Tailscale** - Secure VPN for remote access

## Access Methods

### Localhost (Same Machine)
```
http://localhost:8096/    # Jellyfin
http://localhost:7878/    # Radarr
http://localhost:8989/    # Sonarr
http://localhost:8080/    # qBittorrent
```

### Network (WiFi Devices)
```
http://media-server:8096/    # Jellyfin
http://media-server:7878/    # Radarr
http://media-server:8989/    # Sonarr
```

### Tailscale (Remote via VPN)
```
http://media-server/view/      # Jellyfin
http://media-server/movies/    # Radarr
http://media-server/tv/        # Sonarr
http://media-server/music/     # Navidrome
http://media-server/torrent/   # qBittorrent
```

## Installation

### Prerequisites
- Docker & Docker Compose
- Sufficient storage for media files
- (Optional) Tailscale account for remote access

### Setup Steps

1. **Configure Paths** - Edit `compose_files/.env`:
   ```env
   COMMON_PATH=/path/to/your/media
   MEDIA_MOVIES=/path/to/movies
   MEDIA_TV=/path/to/tv
   MEDIA_MUSIC=/path/to/music
   TZ=America/Los_Angeles
   ```

2. **Start Services**:
   ```bash
   cd compose_files
   docker-compose up -d
   ```

3. **Initial Configuration**:
   - qBittorrent (localhost:8080): Change default password (admin/adminadmin)
   - Prowlarr (localhost:9696): Add indexers
   - Sonarr/Radarr: Connect to qBittorrent and Prowlarr
   - Jellyfin (localhost:8096): Set up media libraries
   - Jellyseerr: Connect to Jellyfin, Sonarr, and Radarr

4. **Tailscale Setup** (Optional):
   ```bash
   # Get auth key from https://login.tailscale.com/admin/settings/keys
   # Add to .env file
   TS_AUTHKEY=tskey-auth-xxxxx
   
   # Restart tailscale
   docker-compose restart tailscale
   ```

## Mobile Apps

### Jellyfin
- **Server (Local)**: `http://media-server:8096`
- **Server (Tailscale)**: `http://media-server/view`

### Navidrome (Symfonium, play:Sub, DSub)
- **Server (Local)**: `http://media-server:4533`
- **Server (Tailscale)**: `http://media-server/music`

## Configuration

### qBittorrent
1. Open `http://localhost:8080`
2. Login: admin/adminadmin (change immediately!)
3. Settings → Downloads → Default Save Path: `/downloads`
4. Create categories: movies, tv, music

### Prowlarr (Indexer Manager)
1. Open `http://localhost:9696`
2. Settings → Indexers → Add indexers
3. Settings → Apps → Add Sonarr and Radarr
4. (Optional) Add Flaresolverr: `http://flaresolverr:8191`

### Sonarr (TV Shows)
1. Open `http://localhost:8989`
2. Settings → Media Management → Root Folder: `/media/TV`
3. Settings → Download Clients → Add qBittorrent:
   - Host: `qbittorrent`
   - Port: `8080`
   - Category: `tv`
4. Settings → Indexers → Add Prowlarr (auto-configured)

### Radarr (Movies)
1. Open `http://localhost:7878`
2. Settings → Media Management → Root Folder: `/media/Movies`
3. Settings → Download Clients → Add qBittorrent:
   - Host: `qbittorrent`
   - Port: `8080`
   - Category: `movies`
4. Settings → Indexers → Add Prowlarr (auto-configured)

### Jellyfin
1. Open `http://localhost:8096`
2. Complete initial setup wizard
3. Add libraries:
   - Movies: `/media/Movies`
   - TV Shows: `/media/TV`
   - Music: `/music/library` (if using Navidrome path)

### Jellyseerr (Requests)
1. Open via Tailscale: `http://media-server/request/`
2. Sign in with Jellyfin account
3. Settings → Jellyfin → Connect to Jellyfin
4. Settings → Services → Add Radarr and Sonarr

## Maintenance

### Update Services
```bash
cd compose_files
docker-compose pull
docker-compose up -d
```

### View Logs
```bash
docker-compose logs -f [service_name]
# Example: docker-compose logs -f radarr
```

### Restart Service
```bash
docker-compose restart [service_name]
```

### Backup Configuration
```bash
tar -czf backup-$(date +%Y%m%d).tar.gz appdata/
```

## Troubleshooting

**Services won't start:**
```bash
docker-compose logs [service_name]
docker ps  # Check running containers
```

**Can't access via hostname:**
- Use IP address instead: `http://192.168.1.100:8096/`
- Check hostname resolves: `ping media-server`

**Tailscale routes don't work:**
```bash
docker exec tailscale tailscale status
docker-compose restart nginx
```

**Mobile apps won't connect:**
- Verify URL format (include port for local, path for Tailscale)
- Test in browser first
- Check Tailscale is connected on phone

## Security

- **Local Network**: All services accessible on WiFi
- **Remote Access**: Only via Tailscale VPN (encrypted)
- **Internet**: No direct access without VPN
- **Recommendation**: Change default passwords, use strong WiFi password

## Architecture

```
                    ┌─────────────┐
                    │   Tailscale │ (VPN Access)
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │    nginx    │ (Reverse Proxy)
                    └──────┬──────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
   ┌────▼────┐       ┌─────▼─────┐     ┌────▼────┐
   │Jellyfin │       │ Navidrome │     │Jellyseerr│
   └─────────┘       └───────────┘     └─────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
   ┌────▼────┐       ┌─────▼─────┐     ┌────▼────┐
   │ Sonarr  │       │  Radarr   │     │Whisparr │
   └────┬────┘       └─────┬─────┘     └────┬────┘
        │                  │                 │
        └──────────────────┼─────────────────┘
                           │
                    ┌──────▼──────┐
                    │  Prowlarr   │ (Indexer Manager)
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │ qBittorrent │ (Torrent Client)
                    └─────────────┘
```

## Contributing

Pull requests welcome! Please:
1. Test your changes
2. Update documentation
3. Keep commits focused

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- **Issues**: Use GitHub Issues for bug reports
- **Questions**: Check existing issues first
- **Security**: See [SECURITY.md](SECURITY.md)

---

**Made with ❤️ for self-hosted media lovers**
