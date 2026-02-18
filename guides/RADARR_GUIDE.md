# Radarr Setup and Usage Guide

## What is Radarr?

Radarr is an automated movie download manager. It searches for, downloads, organizes, and monitors your movie collection automatically.

## Access Radarr

### Via Tailscale (Recommended for Remote Access)
- URL: `http://media-server/radarr/` or `http://100.x.x.x/radarr/`

### On Local Network
- URL: `http://localhost/radarr/` or `http://your-server-ip/radarr/`

### Direct Port Access
- URL: `http://localhost:7878` (server only)

## Initial Setup

### First Launch

1. Open Radarr in browser
2. Go through the setup wizard

### Media Management

1. Click **Settings** → **Media Management**
2. **Movie Naming**:
   - Check **Rename Movies**
   - **Standard Movie Format**: `{Movie Title} ({Release Year})`
   - **Movie Folder Format**: `{Movie Title} ({Release Year})`
3. **Folders**:
   - **Create empty movie folders**: Yes
   - **Delete empty folders**: Yes
   - **Skip Free Space Check**: No
   - **Minimum Free Space**: 2000 MB (movies are larger)
4. **Importing**:
   - **Use Hardlinks instead of Copy**: Yes (saves space!)
   - **Import Extra Files**: Yes (for subtitles, NFO files)
5. **File Management**:
   - **Unmonitor Deleted Movies**: Yes
   - **Propers and Repacks**: Prefer and Upgrade
6. **Root Folders**:
   - Click **Add Root Folder**
   - Add: `/media/Movies` (your main movie library)
   - Add: `/media/drive/Movies` (read-only backup/additional storage)
7. Click **Save**

### Download Clients

#### Add qBittorrent

1. **Settings** → **Download Clients**
2. Click **+** → **qBittorrent**
3. Configure:
   - **Name**: qBittorrent
   - **Enable**: Yes
   - **Host**: `qbittorrent`
   - **Port**: 8080
   - **Username**: `admin` (or your qBittorrent username)
   - **Password**: Your qBittorrent password
   - **Category**: `movies-radarr`
   - **Priority**: 1
4. **Test** connection
5. Click **Save**

### Indexers

#### Connect to Prowlarr (Recommended)

1. **Settings** → **Apps** (in Prowlarr)
2. Add Radarr:
   - **Radarr Server**: `http://radarr:7878`
   - **API Key**: Get from Radarr (Settings → General → API Key)
   - **Test** and **Save**
3. Prowlarr will sync all indexers to Radarr

#### Add Indexers Manually

1. **Settings** → **Indexers**
2. Click **+** → Choose tracker type
3. Configure with credentials
4. **Test** and **Save**

## Using Radarr

### Adding Movies

#### Search and Add

1. Click **Movies** in sidebar
2. Click **Add New Movie**
3. Search for movie title
4. Click on the movie
5. Configure:
   - **Root Folder**: `/media/Movies`
   - **Monitor**: Yes
   - **Minimum Availability**: Announced (or Released/In Cinemas)
   - **Quality Profile**: HD-1080p (or Ultra-HD/4K)
   - **Tags**: Optional
   - **Search on Add**: Check
6. Click **Add Movie**

#### Add Multiple Movies

1. Click **Add Movies** → **Import Movies**
2. Browse to existing movie folder
3. Radarr will scan and detect movies
4. Match each movie
5. **Import Selected**

#### Bulk Operations

1. Select multiple movies (checkboxes)
2. Use **Movie Editor** to:
   - Change quality profiles
   - Update root folders
   - Add/remove tags
   - Monitor/unmonitor

### Lists

Auto-add movies from lists:

1. **Settings** → **Lists**
2. Add lists:
   - **IMDb Lists**: Popular, Top 250, Watchlist
   - **Trakt Lists**: Trending, Popular, Your Watchlist
   - **TMDb Lists**: Popular, Upcoming, Top Rated
   - **StevenLu List**: Tracks theatrical releases
3. For each list:
   - **Enable Automatic Add**: Yes
   - **Root Folder**: `/media/Movies`
   - **Quality Profile**: Your default
   - **Minimum Availability**: Released
4. **Save**
5. Movies from lists auto-add and search

### Monitoring

#### Calendar

- View upcoming releases
- See what's downloading
- Click to search manually

#### Activity

- **Queue**: Currently downloading
- **History**: Past downloads and upgrades
- **Blacklist**: Rejected releases

#### Wanted

- **Missing**: Movies you want but don't have
- **Cutoff Unmet**: Movies below quality threshold
- Click **Search All** to find missing

### Quality Profiles

Create custom quality profiles:

1. **Settings** → **Profiles**
2. **Quality Profiles**:
   - Click **+** to add new
   - Name: "1080p Preferred"
   - Check desired qualities:
     - Bluray-1080p
     - WEB-1080p (WEB-DL and WEBRip)
     - HDTV-1080p
   - Drag to order by preference
   - **Upgrade Until**: Bluray-1080p
   - **Upgrade Until Custom Format Score**: Optional
3. **Custom Formats** (Advanced):
   - Create formats for HDR, IMAX, specific release groups
   - Assign scores to prefer/avoid
4. **Save**

### Search Settings

1. **Settings** → **Indexers**
2. **Options**:
   - **Minimum Seeders**: 1
   - **Retention**: 0 (for torrents)
   - **RSS Sync Interval**: 15 minutes
   - **Maximum Size**: 0 (unlimited) or set limit (e.g., 50 GB for 4K)
   - **Minimum Age**: 0 (download immediately)

## Automation

### Automatic Download

Radarr automatically:
1. Searches RSS feeds every 15 minutes
2. Finds movies matching criteria
3. Sends to qBittorrent
4. Monitors download progress
5. Imports when complete
6. Renames and moves to library
7. Removes torrent

### Quality Upgrading

If better quality appears:
1. Radarr detects upgrade opportunity
2. Downloads automatically
3. Replaces old file
4. Deletes old copy

### Minimum Availability

Control when Radarr searches:
- **Announced**: As soon as announced (may find CAM/TS)
- **In Cinemas**: When theatrical release (may find CAM/TS)
- **Released**: When digital/physical release (recommended)
- **PreDB**: As soon as in PreDB database

## Best Practices

### Quality Recommendations

**For 4K HDR Movies**:
- Quality Profile: Ultra-HD
- Minimum Size: 15 GB
- Upgrade Until: Remux-2160p
- Custom Formats: HDR, DV, IMAX

**For 1080p Movies**:
- Quality Profile: HD-1080p
- Upgrade Until: Bluray-1080p or Remux-1080p
- Custom Formats: Preferred groups

**For Limited Storage**:
- Quality Profile: SD/720p
- Maximum Size: 5 GB
- Upgrade Until: WEB-720p

### Release Restrictions

1. **Settings** → **Indexers** → **Restrictions**
2. **Add Restriction**:
   - **Must Contain**: `1080p, BluRay` (prefer specific)
   - **Must Not Contain**: `CAM, TS, HDCAM, KORSUB, HC` (avoid bad quality)
   - **Tags**: Apply to specific movies only
3. **Save**

### Metadata

Enable metadata for Jellyfin:

1. **Settings** → **Metadata**
2. **Kodi (XBMC) / Emby**:
   - Enable
   - Movie Metadata: Yes
   - Movie Images: Yes
3. **Save**

### Naming Template

For Plex/Jellyfin compatibility:
```
{Movie CleanTitle} ({Release Year}) - {Quality Full}
```

Example: `Inception (2010) - Bluray-1080p.mkv`

## Troubleshooting

### Movie Not Found

1. Try different search terms
2. Use IMDb/TMDb ID:
   - Find movie on IMDb.com or TheMovieDB.org
   - Copy ID
   - Search by `imdb:tt0133093` or `tmdb:603` in Radarr
3. Refresh metadata

### Downloads Not Starting

1. Manual search:
   - Click movie → **Manual Search** icon
   - View available releases
   - Click download icon for preferred release
2. Check indexers:
   - **System** → **Tasks** → **RSS Sync** → **Run**
3. Check qBittorrent connection:
   - **Settings** → **Download Clients** → **Test**

### Movie Not Importing

1. Verify qBittorrent category: `movies-radarr`
2. Check files in `/downloads/movies-radarr`
3. Check logs: **System** → **Logs**
4. Manual import:
   - **Activity** → **Manual Import**
   - Select folder
   - Match movies
   - Import

### Wrong Movie Matched

1. **Edit Movie**
2. Click **Search** icon next to title
3. Find correct match by TMDb ID
4. **Refresh & Scan**

### Disk Space Issues

1. Check available space:
   ```bash
   df -h /media/Movies
   ```
2. Set size limits:
   - **Settings** → **Indexers** → **Maximum Size**
3. Delete old/unwatched movies
4. Use 720p profile for less important movies

## Advanced Features

### Custom Formats

Create custom formats to prefer/avoid:

1. **Settings** → **Custom Formats**
2. Examples:
   - **HDR**: Prefer HDR releases
   - **IMAX**: Prefer IMAX versions
   - **Scene**: Avoid scene releases
   - **x265**: Prefer HEVC encoding
3. Assign scores in quality profile
4. Radarr uses scores to pick best release

### Custom Scripts

Run scripts on events:

1. **Settings** → **Connect**
2. Add **Custom Script**
3. Script runs on:
   - Download
   - Import
   - Upgrade
   - Rename
4. Example: Send notification, update external database

### Notifications

Get notified:

1. **Settings** → **Connect**
2. Add connection:
   - **Discord**: Webhook for channel notifications
   - **Telegram**: Bot for mobile notifications
   - **Email**: SMTP for email alerts
   - **Pushbullet/Pushover**: Mobile push
3. Configure triggers:
   - On Download
   - On Import
   - On Upgrade
   - On Health Issue

### API

Radarr REST API:
- Endpoint: `http://media-server/radarr/api/v3/`
- API Key: **Settings** → **General** → **API Key**
- Documentation: [https://radarr.video/docs/api](https://radarr.video/docs/api)
- Use for automation, custom scripts, monitoring

## Integration

### Jellyseerr

Jellyseerr connects to Radarr:
- Users request movies in Jellyseerr
- Requests auto-sent to Radarr
- Radarr downloads and imports
- Jellyfin updates library
- Users get notifications

### Bazarr

Bazarr monitors Radarr:
- Detects new movies
- Downloads subtitles
- Places in movie folder
- Jellyfin picks up automatically

## Security

1. **Authentication**:
   - **Settings** → **General** → **Authentication**
   - **Method**: Basic (Browser popup) or Forms
   - Set username and password
2. **API Key**: Keep secret
3. **Use Tailscale**: Don't expose to internet
4. **Regular Updates**:
   ```bash
   docker-compose pull radarr
   docker-compose up -d radarr
   ```

## Maintenance

### Database Backup

Radarr automatically backs up database:
- Location: `/config/Backups/`
- Frequency: Daily
- Keep 7 days
- Manual backup: **System** → **Backup**

### Housekeeping

1. **Clear blacklist** periodically:
   - **Activity** → **Blacklist**
   - Remove old entries
2. **Check health**:
   - **System** → **Health**
   - Fix any warnings
3. **Update**:
   - **System** → **Updates**
   - Check for updates regularly

## Tips

1. **Use Lists**: Auto-add popular/upcoming movies
2. **Set Availability Properly**: Avoid CAM/TS by waiting for release
3. **Custom Formats**: Fine-tune quality preferences
4. **Monitor Disk Space**: Movies are large!
5. **Hardlinks**: Save space by not duplicating files
6. **Tags**: Organize by genre, priority, family-friendly
7. **RSS Sync**: Keep at 15 minutes for quick downloads

## Support & Resources

- Official Website: [https://radarr.video](https://radarr.video)
- Wiki: [https://wiki.servarr.com/radarr](https://wiki.servarr.com/radarr)
- Discord: [Radarr Discord](https://discord.gg/AD3UP37x8R)
- Reddit: [r/radarr](https://reddit.com/r/radarr)
