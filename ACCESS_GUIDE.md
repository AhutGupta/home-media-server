# Media Server Access Guide

## Quick Reference

Your media server now supports three access methods:

| Access Method | URL Format | Works From | Example |
|--------------|------------|------------|---------|
| **Localhost** | `http://localhost:PORT/` | Same machine | `http://localhost:7878/` |
| **Network** | `http://media-server:PORT/` | WiFi devices | `http://media-server:7878/` |
| **Tailscale** | `http://media-server/PATH/` | Remote (VPN) | `http://media-server/movies/` |

## Service Ports and Routes

### Media Streaming

**Jellyfin** (Movies & TV Shows)
- Local: `http://localhost:8096/` or `http://media-server:8096/`
- Tailscale: `http://media-server/view/`
- Port: 8096

**Navidrome** (Music)
- Local: `http://localhost:4533/` or `http://media-server:4533/`
- Tailscale: `http://media-server/music/`
- Port: 4533

### Content Management

**Sonarr** (TV Shows)
- Local: `http://localhost:8989/` or `http://media-server:8989/`
- Tailscale: `http://media-server/tv/`
- Port: 8989

**Radarr** (Movies)
- Local: `http://localhost:7878/` or `http://media-server:7878/`
- Tailscale: `http://media-server/movies/`
- Port: 7878

**Whisparr** (Adult Content)
- Local: `http://localhost:6969/` or `http://media-server:6969/`
- Tailscale: `http://media-server/xxx/`
- Port: 6969

### Download Management

**qBittorrent** (Torrents)
- Local: `http://localhost:8080/` or `http://media-server:8080/`
- Tailscale: `http://media-server/torrent/`
- Port: 8080
- Default login: admin/adminadmin (CHANGE THIS!)

**Prowlarr** (Indexer Manager)
- Local: `http://localhost:9696/` or `http://media-server:9696/`
- Tailscale: Not exposed (management only)
- Port: 9696

### Request System

**Jellyseerr** (Content Requests)
- Tailscale: `http://media-server/request/`
- Port: 5055 (not exposed for direct access)

## Mobile Apps

### Jellyfin App (iOS/Android)

**Server Configuration:**
- **Local Network**: 
  - Server URL: `http://media-server:8096`
  - Works when connected to home WiFi
- **Tailscale (Remote)**:
  - Server URL: `http://media-server/view`
  - Works from anywhere with Tailscale

**Setup:**
1. Install Jellyfin app from App Store/Play Store
2. Open app and select "Add Server"
3. Enter server URL (choose based on your location)
4. Login with your credentials
5. Start streaming!

### Navidrome Apps

**Recommended Apps:**
- **Android**: Symfonium (best), Subtracks, DSub
- **iOS**: play:Sub (best), Amperfy

**Server Configuration:**
- **Local Network**:
  - Server URL: `http://media-server:4533`
  - Server Type: Navidrome/Subsonic
- **Tailscale (Remote)**:
  - Server URL: `http://media-server/music`
  - Server Type: Navidrome/Subsonic

**Setup (Symfonium example):**
1. Install Symfonium from Play Store
2. Open app → Settings → Servers
3. Add Server:
   - Name: Home Music Server
   - Type: Navidrome
   - URL: `http://media-server:4533` (local) or `http://media-server/music` (Tailscale)
   - Username: Your Navidrome username
   - Password: Your Navidrome password
4. Test connection
5. Start listening!

## Usage Scenarios

### Scenario 1: Using Services Locally (Same Machine)

You're sitting at your server machine:

```bash
# Open Radarr
http://localhost:7878/

# Open qBittorrent
http://localhost:8080/

# Open Jellyfin
http://localhost:8096/
```

All direct port access works perfectly.

### Scenario 2: Using from Another Device (Same WiFi)

You're on your phone/laptop connected to home WiFi:

```bash
# Open Radarr on phone browser
http://media-server:7878/

# Use Jellyfin mobile app
Server: http://media-server:8096

# Use Navidrome mobile app
Server: http://media-server:4533
```

Replace `media-server` with your server's IP if hostname doesn't resolve.

### Scenario 3: Remote Access via Tailscale

You're away from home, connected via Tailscale VPN:

```bash
# Open Radarr
http://media-server/movies/

# Open Sonarr
http://media-server/tv/

# Open qBittorrent
http://media-server/torrent/

# Use Jellyfin mobile app
Server: http://media-server/view

# Use Navidrome mobile app
Server: http://media-server/music
```

Only nginx reverse proxy routes work via Tailscale (by design).

## Dashboard

Access the dashboard to see all services:
- Local: `http://localhost/` or `http://media-server/`
- Tailscale: `http://media-server/`

The dashboard shows:
- All services with their access URLs
- Color-coded access methods (green=local, blue=Tailscale)
- Mobile app configuration instructions
- Quick links to each service

## Troubleshooting

### Can't Access via media-server Hostname

**Problem**: `http://media-server:7878/` doesn't work

**Solutions**:
1. Use IP address instead: `http://192.168.1.100:7878/`
2. Check if hostname resolves: `ping media-server`
3. Add to hosts file:
   - Windows: `C:\Windows\System32\drivers\etc\hosts`
   - Mac/Linux: `/etc/hosts`
   - Add line: `192.168.1.100 media-server`

### Services Not Loading

**Problem**: Services don't respond

**Solutions**:
1. Check containers are running:
   ```bash
   docker ps
   ```
2. Restart specific service:
   ```bash
   docker-compose restart radarr
   ```
3. Check logs:
   ```bash
   docker logs radarr
   ```

### Tailscale Routes Don't Work

**Problem**: `http://media-server/movies/` gives 404

**Solutions**:
1. Verify you're connected to Tailscale VPN
2. Check nginx is running:
   ```bash
   docker ps | grep nginx
   ```
3. Restart nginx:
   ```bash
   docker-compose restart nginx
   ```
4. Check nginx logs:
   ```bash
   docker logs nginx
   ```

### Mobile App Won't Connect

**Problem**: Jellyfin/Navidrome app can't reach server

**Solutions**:
1. Verify URL format (include port for local, path for Tailscale)
2. Check you're on correct network (WiFi for local, Tailscale for remote)
3. Test in browser first with same URL
4. Try IP address instead of hostname
5. Ensure no firewall is blocking the ports

## Security Considerations

### Current Setup

**Local Network Access:**
- All ports open on 0.0.0.0 (accessible from any network device)
- Anyone on your WiFi can access all services
- Easy for family/roommates to use

**Tailscale Access:**
- Only nginx routes exposed
- VPN encryption for all traffic
- Must be authenticated to your Tailscale network

**Internet Access:**
- No direct access possible
- Must connect via Tailscale VPN
- No port forwarding needed

### Recommendations

1. **WiFi Security**: Use strong WPA3 password
2. **Service Passwords**: Change default passwords (especially qBittorrent!)
3. **Tailscale 2FA**: Enable two-factor authentication
4. **Regular Updates**: Keep Docker images updated
   ```bash
   docker-compose pull
   docker-compose up -d
   ```

## Advanced Configuration

### Using Direct IP Instead of Hostname

If `media-server` hostname doesn't work:

1. Find your server's IP:
   ```bash
   hostname -I
   # or
   ip addr show
   ```

2. Use IP in URLs:
   - Local: `http://192.168.1.100:7878/`
   - Mobile apps: `http://192.168.1.100:8096`

### Setting up Hostname Resolution

**Option 1: Router DNS (Best)**
1. Access your router admin panel
2. Find DHCP/DNS settings
3. Add static hostname: `media-server` → `192.168.1.100`

**Option 2: Client Hosts File**
Add to each client's hosts file:
```
192.168.1.100 media-server
```

## Updates and Maintenance

### Updating Services

```bash
cd compose_files

# Pull latest images
docker-compose pull

# Restart services with new images
docker-compose up -d

# Remove old images
docker image prune
```

### Backing Up Configuration

```bash
# Backup appdata folder
tar -czf backup-$(date +%Y%m%d).tar.gz appdata/

# Or use rsync
rsync -av appdata/ /path/to/backup/
```

### Viewing Logs

```bash
# All services
docker-compose logs

# Specific service
docker-compose logs radarr

# Follow logs in real-time
docker-compose logs -f jellyfin

# Last 50 lines
docker-compose logs --tail=50 sonarr
```

## Quick Start Checklist

- [ ] Access dashboard: `http://localhost/` or `http://media-server/`
- [ ] Set up qBittorrent (change default password!)
- [ ] Configure Prowlarr with indexers
- [ ] Connect Sonarr to qBittorrent and Prowlarr
- [ ] Connect Radarr to qBittorrent and Prowlarr
- [ ] Set up Jellyfin libraries
- [ ] Create Navidrome admin account
- [ ] Install Jellyfin mobile app (configure with `http://media-server:8096`)
- [ ] Install Navidrome mobile app (configure with `http://media-server:4533`)
- [ ] Test Tailscale access from remote device

## Support

For issues or questions:
1. Check logs: `docker-compose logs [service]`
2. Restart service: `docker-compose restart [service]`
3. Check dashboard for correct URLs
4. Verify network connectivity
5. Review this guide for troubleshooting steps

---

**Last Updated**: 2026-02-18
**Configuration Version**: docker-compose.yaml v3.9
