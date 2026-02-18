# qBittorrent Setup and Usage Guide

## What is qBittorrent?

qBittorrent is a free, open-source BitTorrent client with a feature-rich web interface. It handles all your torrent downloads for movies, TV shows, and other content.

## Access qBittorrent

### Via Tailscale (Recommended for Remote Access)
- URL: `http://media-server:8080` or `http://100.x.x.x:8080`
- Replace `100.x.x.x` with your Tailscale IP

### On Local Network
- URL: `http://localhost:8080` or `http://your-server-ip:8080`

### Direct Access (Server Only)
- URL: `http://127.0.0.1:8080`

## Initial Setup

### First Login

1. Open qBittorrent web UI
2. **Default credentials**:
   - Username: `admin`
   - Password: `adminadmin`
3. **IMPORTANT**: Change password immediately!

### Change Default Password

1. After logging in, click **Tools** → **Options** (or gear icon)
2. Go to **Web UI** tab
3. Under **Authentication**:
   - **Username**: Change if desired (or keep `admin`)
   - **Password**: Enter new strong password
4. Click **Save**
5. You'll be logged out - log back in with new credentials

## Configuration

### Basic Settings

1. Click **Tools** → **Options**
2. **Downloads**:
   - **Save files to location**: `/downloads` (default - correct)
   - **Keep incomplete torrents in**: `/downloads/incomplete`
   - **Auto-delete .torrent files**: Check
   - **Pre-allocate disk space**: Check (prevents fragmentation)
3. **Connection**:
   - **Port used for incoming connections**: 6881 (default)
   - **Use UPnP/NAT-PMP**: Off (not needed with our setup)
4. **Speed**:
   - **Global Rate Limits**:
     - Upload: 0 (unlimited) or set limit
     - Download: 0 (unlimited) or set limit
   - **Alternative Rate Limits**: Set for daytime/nighttime
5. **BitTorrent**:
   - **Privacy**:
     - **Enable DHT**: Yes
     - **Enable PeX**: Yes
     - **Enable Local Peer Discovery**: Yes
   - **Seeding Limits**:
     - **When ratio reaches**: 2.0 (or your preference)
     - **Then**: Pause torrent (to seed fairly but not forever)

### Categories (Important for Automation)

Categories help Sonarr/Radarr manage downloads:

1. Right-click in left sidebar where it says "All"
2. Select **Add category**
3. Create these categories:
   - **tv-sonarr**: Save path `/downloads/tv-sonarr`
   - **movies-radarr**: Save path `/downloads/movies-radarr`
   - **music**: Save path `/downloads/music`
   - **other**: Save path `/downloads/other`
4. Click **OK** for each

### Web UI Settings

1. **Tools** → **Options** → **Web UI**
2. Security:
   - **Enable Cross-Site Request Forgery (CSRF) protection**: Yes
   - **Enable Host header validation**: Yes
   - **Server domains**: `media-server, 100.x.x.x` (your Tailscale IP)
   - **Enable clickjacking protection**: Yes
3. **IP address**: Leave at `*` (all interfaces)
4. **Port**: 8080
5. **Use HTTPS**: Off (nginx handles this if you add SSL)

### Advanced Settings

1. **Advanced** tab:
2. Important settings:
   - **Disk cache**: 64 MB minimum (higher if you have RAM)
   - **Disk cache expiry interval**: 60 seconds
   - **Async I/O threads**: 4 (adjust based on CPU)
   - **File pool size**: 40
   - **Send upload piece suggestions**: Yes

## Using qBittorrent

### Adding Torrents Manually

#### From Torrent File

1. Click **+** (Add Torrent) button
2. Click **Browse** or drag torrent file
3. Configure:
   - **Category**: Select appropriate category
   - **Save path**: Auto-filled based on category
   - **Start torrent**: Check (usually)
   - **Skip hash check**: Uncheck (unless you know file is complete)
4. Click **OK**

#### From Magnet Link

1. Copy magnet link
2. Click **+** (Add Torrent)
3. Paste magnet link
4. Configure same as above
5. Click **OK**

### RSS Feeds (Automatic Downloads)

1. **View** → **RSS Reader** (or RSS icon)
2. Right-click **RSS feeds** → **New subscription**
3. Add feed URL from your favorite tracker
4. Click **OK**
5. Right-click feed → **New rule**
6. Configure:
   - **Must Contain**: Keywords (e.g., "1080p BluRay")
   - **Must Not Contain**: Keywords to exclude (e.g., "CAM", "TS")
   - **Episode Filter**: For TV shows (e.g., `S01E*`)
   - **Assign Category**: Select category
   - **Save to**: Path (auto-filled from category)
7. Click **OK**
8. **Note**: Sonarr/Radarr handle this better - use them instead!

### Managing Downloads

#### Pause/Resume

- Right-click torrent → **Pause** or **Resume**
- Or use toolbar buttons

#### Prioritize Downloads

- Right-click → **Priority**:
  - **Maximum**: Download first
  - **High**: Prioritize
  - **Normal**: Default
  - **Low**: Download last

#### File Selection

For multi-file torrents:

1. Right-click torrent → **Content**
2. Uncheck files you don't want
3. Click **OK**

#### Move Completed Downloads

- Right-click completed torrent
- Select **Set location**
- Choose new location
- Check **Move** (not copy)
- Click **OK**

### Monitoring

#### Transfer Tab

View all torrents with:
- Name, Size, Status
- Download/Upload speeds
- ETA, Seeds/Peers
- Ratio, Progress

#### Statistics

- Click **View** → **Statistics**
- Shows:
  - Session stats (current session)
  - All-time stats
  - Total uploaded/downloaded
  - Ratio

#### Speed Graph

- **View** → **Graphs**
- See real-time speed charts

## Integration with Sonarr/Radarr

### In qBittorrent

1. Ensure categories are set up (see Configuration above)
2. No other qBittorrent config needed

### In Sonarr/Radarr

See respective guides for connecting them to qBittorrent. They will:
- Send torrents to qBittorrent automatically
- Assign proper category
- Monitor download progress
- Move completed files to library
- Delete torrent when done

## Best Practices

### Security

1. **Change default password** - CRITICAL!
2. **Use Tailscale** - Don't expose qBittorrent directly to internet
3. **Limit upload slots** - Prevents resource exhaustion
4. **Enable IP filtering** (optional):
   - Tools → Options → Connection
   - Enable IP Filtering
   - Add lists to block bad peers

### Performance

1. **Connection limits**:
   - Global max connections: 500
   - Max connections per torrent: 100
   - Max uploads per torrent: 15
2. **Disk cache**: Increase if you have RAM
3. **Pre-allocation**: Enable for better performance
4. **Reduce logging**: If disk writes are concern

### Seeding Etiquette

1. **Seed to 2.0 ratio minimum** - Give back what you take
2. **Seed rare torrents longer** - Help the community
3. **Limit concurrent active torrents**: 5-10 (prevents overwhelming)
4. **Set upload limits** - Don't saturate your connection

### Organization

1. **Use categories** - Keep downloads organized
2. **Tag torrents** - Add custom tags for tracking
3. **Delete finished** - Remove after seeding completes
4. **Regular cleanup** - Remove old, inactive torrents

## Troubleshooting

### Can't Connect to Web UI

1. Check container is running:
   ```bash
   docker ps | grep qbittorrent
   ```
2. Check logs:
   ```bash
   docker logs qbittorrent
   ```
3. Verify you're on Tailscale network
4. Try direct localhost access from server

### Slow Download Speeds

1. **Check seeds**: Low seeds = slow speeds
2. **Connection limits**: May be too restrictive
3. **ISP throttling**: Some ISPs throttle BitTorrent
4. **Port forwarding**: Not needed with our setup, but check if you modified
5. **CPU/Disk bottleneck**: Monitor server resources

### Torrents Stuck at 99%

1. **Let it finish**: Sometimes takes time to get rare pieces
2. **More seeds**: Add more trackers
3. **Force recheck**:
   - Right-click → **Force recheck**
   - Verifies already downloaded pieces

### Storage Full

1. Check download folder:
   ```bash
   docker exec qbittorrent df -h /downloads
   ```
2. Remove completed/seeded torrents:
   - Filter by status → "Completed"
   - Right-click → **Delete** → **Also delete files**
3. Adjust seeding limits to auto-remove

### Permission Errors

1. Verify PUID/PGID in docker-compose.yaml
2. Check folder permissions:
   ```bash
   ls -la /path/to/downloads
   ```
3. May need to adjust ownership:
   ```bash
   sudo chown -R 1000:1000 /path/to/downloads
   ```

## Advanced Features

### Search Plugins

qBittorrent can search trackers directly:

1. **View** → **Search Engine**
2. Click **Search plugins**
3. Install plugins for your favorite trackers
4. Search directly from qBittorrent
5. **Note**: Private trackers usually don't allow this

### Sequential Download

Download files in order (useful for video):

1. Right-click torrent
2. Check **Download in sequential order**
3. Check **Download first and last pieces first**
4. Allows watching while downloading

### Super Seeding

For initial seeding of your own torrents:

1. Right-click torrent
2. Check **Super seeding mode**
3. Distributes pieces more efficiently

### IP Binding

Bind to specific network interface:

1. Tools → Options → Advanced
2. **Network Interface**: Select interface
3. Useful if using VPN (see VPN-Only configs)

## Automation

### Watch Folder

Auto-add torrents from folder:

1. Tools → Options → Downloads
2. Check **Run external program on torrent completion**
3. Command example for moving:
   ```bash
   mv "%F" /path/to/completed/
   ```

### API Access

qBittorrent has a Web API:
- Documentation: [https://github.com/qbittorrent/qBittorrent/wiki/WebUI-API-(qBittorrent-4.1)](https://github.com/qbittorrent/qBittorrent/wiki/WebUI-API-(qBittorrent-4.1))
- Endpoint: `http://media-server:8080/api/v2/`
- Used by Sonarr/Radarr automatically

## Security Checklist

- [ ] Changed default password
- [ ] Enabled CSRF protection
- [ ] Enabled Host header validation
- [ ] Access only via Tailscale
- [ ] Regular backups of qBittorrent/config folder
- [ ] Monitor for unusual activity
- [ ] Keep qBittorrent updated

## Update qBittorrent

```bash
cd compose_files
docker-compose pull qbittorrent
docker-compose up -d qbittorrent
```

## Support & Resources

- Official Website: [https://www.qbittorrent.org](https://www.qbittorrent.org)
- Wiki: [https://github.com/qbittorrent/qBittorrent/wiki](https://github.com/qbittorrent/qBittorrent/wiki)
- Forum: [https://qbforums.shiki.hu](https://qbforums.shiki.hu)
- Reddit: [r/qBittorrent](https://reddit.com/r/qBittorrent)
