# Sonarr Setup and Usage Guide

## What is Sonarr?

Sonarr is an automated TV show download manager. It searches for, downloads, organizes, and monitors your TV show collection automatically.

## Access Sonarr

### Via Tailscale (Recommended for Remote Access)
- URL: `http://media-server/sonarr/` or `http://100.x.x.x/sonarr/`

### On Local Network
- URL: `http://localhost/sonarr/` or `http://your-server-ip/sonarr/`

### Direct Port Access
- URL: `http://localhost:8989` (server only)

## Initial Setup

### First Launch

1. Open Sonarr in browser
2. Go through the setup wizard

### Media Management

1. Click **Settings** → **Media Management**
2. **Episode Naming**:
   - Check **Rename Episodes**
   - **Standard Episode Format**: `{Series Title} - S{season:00}E{episode:00} - {Episode Title}`
   - **Daily Episode Format**: `{Series Title} - {Air-Date} - {Episode Title}`
   - **Anime Episode Format**: `{Series Title} - S{season:00}E{episode:00} - {Episode Title}`
3. **Folders**:
   - **Create empty series folders**: Yes
   - **Delete empty folders**: Yes
4. **Importing**:
   - **Skip Free Space Check**: No
   - **Minimum Free Space**: 1000 MB
   - **Use Hardlinks instead of Copy**: Yes (important!)
5. **File Management**:
   - **Unmonitor Deleted Episodes**: Yes
   - **Propers and Repacks**: Prefer and Upgrade
6. **Root Folders**:
   - Click **Add Root Folder**
   - Add: `/media/TV` (your main TV library)
   - Add: `/media/personal` (if you store personal shows)
7. Click **Save**

### Download Clients

#### Add qBittorrent

1. **Settings** → **Download Clients**
2. Click **+** → **qBittorrent**
3. Configure:
   - **Name**: qBittorrent
   - **Enable**: Yes
   - **Host**: `qbittorrent` (Docker container name)
   - **Port**: 8080
   - **Username**: `admin` (or your qBittorrent username)
   - **Password**: Your qBittorrent password
   - **Category**: `tv-sonarr`
   - **Priority**: 1
4. **Test** connection
5. Click **Save**

### Indexers

#### Add Prowlarr (Recommended)

1. **Settings** → **Indexers**
2. Prowlarr will sync automatically if configured (see Prowlarr guide)

#### Add Manually

1. Click **+** → Choose your tracker type
2. Configure with tracker credentials
3. **Test** and **Save**

### Connect to Prowlarr

1. **Settings** → **Apps**
2. Click **+** → **Prowlarr**
3. Configure:
   - **Prowlarr Server**: `http://prowlarr:9696`
   - **API Key**: Get from Prowlarr (Settings → General → API Key)
4. **Test** and **Save**
5. Prowlarr will now sync all indexers automatically

## Using Sonarr

### Adding TV Shows

#### Search and Add

1. Click **Series** in sidebar
2. Click **Add New**
3. Search for show name
4. Click on the show
5. Configure:
   - **Root Folder**: `/media/TV`
   - **Monitor**: All Episodes (or Latest Season)
   - **Quality Profile**: HD-1080p (or your preference)
   - **Series Type**: Standard (or Anime/Daily)
   - **Season Folder**: Yes
   - **Tags**: Optional
   - **Start search for missing episodes**: Check
6. Click **Add Series**

#### Bulk Import Existing

If you have existing TV shows:

1. **Library Import** from menu
2. Select root folder with existing shows
3. Sonarr will scan and match shows
4. Confirm matches
5. Click **Import**

### Monitoring

#### Calendar

- View upcoming episodes
- See download schedule
- Click any episode to search manually

#### Activity

- **Queue**: Currently downloading episodes
- **History**: Past downloads and upgrades
- **Blacklist**: Failed/rejected downloads

#### Wanted

- **Missing**: Episodes you want but don't have
- **Cutoff Unmet**: Episodes below your quality target
- Click **Search All** to find missing episodes

### Quality Profiles

Create custom quality profiles:

1. **Settings** → **Profiles**
2. Click **+** to add new profile
3. Name it (e.g., "1080p Preferred")
4. Check qualities to allow (e.g., HDTV-1080p, Bluray-1080p)
5. Drag to order by preference
6. Set **Upgrade Until**: Bluray-1080p
7. **Save**

### Tags and Categories

Organize shows with tags:

1. **Settings** → **Tags**
2. Add tags (e.g., "kids", "favorites", "4k")
3. When adding series, assign tags
4. Filter series by tags later

## Automation

### Automatic Search

Sonarr searches automatically:
- When new episode airs (based on schedule)
- Every RSS sync (default: 15 minutes)
- When series is added

### Post-Processing

1. qBittorrent downloads to `/downloads/tv-sonarr`
2. Sonarr monitors completion
3. Episode is:
   - Renamed to your format
   - Moved to `/media/TV/Show Name/Season XX/`
   - Permissions set
   - Torrent removed from qBittorrent

### Upgrading Quality

If better quality becomes available:
1. Sonarr finds upgrade
2. Downloads automatically
3. Replaces old file
4. Old file deleted

## Best Practices

### Quality Settings

**For 1080p HDR TV**:
- Profile: HD-1080p/2160p
- Upgrade Until: Bluray-1080p or Remux-1080p

**For Regular 1080p**:
- Profile: HD-1080p
- Upgrade Until: Bluray-1080p

**For Limited Storage**:
- Profile: SD/720p
- Size limits help control storage

### Release Restrictions

1. **Settings** → **Indexers** → **Restrictions**
2. Add restrictions:
   - **Must Contain**: `1080p` (only 1080p releases)
   - **Must Not Contain**: `CAM, TS, HDCAM` (avoid bad quality)
3. Save

### Metadata

Enable metadata for Jellyfin:

1. **Settings** → **Metadata**
2. **Kodi (XBMC) / Emby**:
   - Enable
   - Series Metadata: Yes
   - Episode Metadata: Yes
   - Series Images: Yes
   - Episode Images: Yes
3. **Save**

## Troubleshooting

### Show Not Found

1. Try different search terms
2. Use TVDb ID:
   - Find show on TheTVDB.com
   - Copy ID from URL
   - Search by `tvdb:12345` in Sonarr

### Downloads Not Starting

1. Check indexers are working:
   - **System** → **Tasks** → **RSS Sync** → **Run**
2. Manual search:
   - Go to series → Season
   - Click magnifying glass icon
   - Check available releases
3. Check qBittorrent connection:
   - **Settings** → **Download Clients** → **Test**

### Episodes Not Importing

1. Check qBittorrent category is `tv-sonarr`
2. Verify files are in `/downloads/tv-sonarr`
3. Check logs: **System** → **Logs**
4. Manual import:
   - **Activity** → **Queue**
   - Click **Manual Import**

### Wrong Episode Metadata

1. **Update Series Info**:
   - Series → Edit → **Update**
2. **Refresh Series**:
   - Forces metadata reload
3. Check TVDb for correct info

## Advanced Features

### Custom Scripts

Run scripts on import:

1. **Settings** → **Connect**
2. Add **Custom Script**
3. Example: Notify when episode downloads
4. Script runs on events (Download, Upgrade, Rename)

### Lists

Auto-add shows from lists:

1. **Settings** → **Lists**
2. Add Trakt List, IMDb List, etc.
3. Shows from list auto-add to Sonarr

### Notifications

Get notified on events:

1. **Settings** → **Connect**
2. Add:
   - **Discord**: Webhook URL
   - **Telegram**: Bot token
   - **Email**: SMTP settings
   - **Pushover**: API key
3. Configure what triggers notifications

### API

Sonarr has REST API:
- Endpoint: `http://media-server/sonarr/api/v3/`
- API Key: **Settings** → **General** → **API Key**
- Documentation: [https://sonarr.tv/docs/api](https://sonarr.tv/docs/api)

## Security

1. **Authentication**:
   - **Settings** → **General** → **Authentication**
   - Method: **Basic (Browser popup)**
   - Username/Password: Set strong credentials
2. **API Key**: Keep secret
3. **Use Tailscale**: Don't expose directly
4. **Update Regularly**:
   ```bash
   docker-compose pull sonarr
   docker-compose up -d sonarr
   ```

## Integration with Other Services

### Jellyseerr

Jellyseerr connects to Sonarr automatically. See Jellyseerr guide.

### Bazarr

Bazarr monitors Sonarr for new episodes and downloads subtitles.

## Support & Resources

- Official Website: [https://sonarr.tv](https://sonarr.tv)
- Wiki: [https://wiki.servarr.com/sonarr](https://wiki.servarr.com/sonarr)
- Discord: [Sonarr Discord](https://discord.gg/M6BvZn5)
- Reddit: [r/sonarr](https://reddit.com/r/sonarr)
