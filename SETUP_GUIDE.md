# 📘 Setup Guide - Home Media Server

This guide helps you configure all services after initial installation. Each section links to official documentation to reduce maintenance overhead.

## Table of Contents

1. [Initial Setup](#initial-setup)
2. [Quick Wiring Checklist](#quick-wiring-checklist)
3. [Jellyfin Configuration](#jellyfin-configuration)
4. [Download Client (qBittorrent)](#download-client-qbittorrent)
5. [Indexer Manager (Prowlarr)](#indexer-manager-prowlarr)
6. [TV Shows (Sonarr)](#tv-shows-sonarr)
7. [Movies (Radarr)](#movies-radarr)
8. [Music (Lidarr)](#music-lidarr)
9. [Content Requests (Jellyseerr)](#content-requests-jellyseerr)
10. [Subtitles (Bazarr)](#subtitles-bazarr)
11. [Container Management (Portainer)](#container-management-portainer)
12. [Podcasts & Audiobooks (Audiobookshelf)](#podcasts--audiobooks-audiobookshelf)
13. [Music Discovery (Slskd)](#music-discovery-slskd)
14. [Music Tagging (Beets)](#music-tagging-beets)

---

## Initial Setup

### 1. Configure Environment Variables

```bash
cd compose_files
cp .env.example .env
nano .env  # or use your preferred editor
```

Update these critical values:
- `COMMON_PATH` - Where configs/data will be stored
- `TZ` - Your timezone
- `MEDIA_*` paths - Where your media files are located
- `TS_AUTHKEY` - Tailscale auth key (optional, for remote access)

### 2. Start Services

```bash
docker-compose up -d
```

### 3. Access Dashboard

Open `http://localhost/` to see all services.

---

## Quick Wiring Checklist

Use this sequence after the containers are running to connect everything end-to-end:

1. **qBittorrent**: Log in at `http://localhost:8080`, change the default password, and keep downloads in `/downloads`.
2. **Prowlarr**: Add your indexers, then add a download client pointing to `http://qbittorrent:8080` with your new qBittorrent credentials.
3. **Sonarr / Radarr / Lidarr**:  
   - Add Prowlarr as the indexer source (`http://prowlarr:9696` with your Prowlarr API key).  
   - Add qBittorrent as the download client (`http://qbittorrent:8080`).  
   - Set root folders to match your mounts (e.g., `/media/TV`, `/media/Movies`, `/media/Music`).
4. **Jellyseerr**: Connect to Jellyfin, then to Sonarr/Radarr using their internal URLs (`http://sonarr:8989`, `http://radarr:7878`).
5. **Jellyfin**: Add libraries that match the same paths used above so completed downloads are imported automatically.
6. **Verify**: Request one test movie/episode in Jellyseerr → confirm it appears in Sonarr/Radarr → downloaded by qBittorrent → imported into Jellyfin.

---

## Jellyfin Configuration

**Access:** `http://localhost:8096`

### Initial Setup Wizard

1. **Language & User:**
   - Select language
   - Create admin account (username/password)

2. **Add Media Libraries:**
   - Click "Add Media Library"
   - **Movies:** Type: Movies, Path: `/data/movies`
   - **TV Shows:** Type: Shows, Path: `/data/tvshows`
   - **Music:** Type: Music, Paths: `/media/Music` and `/media/drive/Music`
   - **Personal Videos:** Type: Home Videos, Path: `/media/Personal`

3. **Metadata Settings:**
   - Enable preferred metadata providers
   - Recommended: TMDb, TheTVDB, MusicBrainz

4. **Remote Access:**
   - Configure for LAN access
   - Use Tailscale for secure remote access

### Official Documentation

- **Complete Setup Guide:** https://jellyfin.org/docs/general/quick-start
- **Library Management:** https://jellyfin.org/docs/general/server/libraries
- **User Management:** https://jellyfin.org/docs/general/server/users/
- **Mobile Apps:** https://jellyfin.org/downloads/clients
- **Hardware Acceleration:** https://jellyfin.org/docs/general/administration/hardware-acceleration

### Mobile Apps

- **Android:** [Google Play Store](https://play.google.com/store/apps/details?id=org.jellyfin.mobile)
- **iOS:** [App Store](https://apps.apple.com/app/jellyfin-mobile/id1480192618)
- **Server URL:** `http://media-server:8096` (via Tailscale) or `http://192.168.x.x:8096` (local)

---

## Download Client (qBittorrent)

**Access:** `http://localhost:8080`

### Initial Setup

1. **Default Credentials:**
   - Username: `admin`
   - Password: `adminadmin`
   - **⚠️ Change this immediately!**

2. **Change Password:**
   - Tools → Options → Web UI → Authentication
   - Set new password

3. **Configure Paths:**
   - Options → Downloads
   - Default Save Path: `/downloads`
   - Keep incomplete torrents in: `/downloads/incomplete`

4. **Network Settings:**
   - Options → Connection
   - Port: 6881 (already configured)
   - Use UPnP/NAT-PMP: Enabled

### Official Documentation

- **qBittorrent Wiki:** https://github.com/qbittorrent/qBittorrent/wiki
- **Settings Guide:** https://github.com/qbittorrent/qBittorrent/wiki/Frequently-Asked-Questions

---

## Indexer Manager (Prowlarr)

**Access:** `http://localhost:9696`

### Initial Setup

1. **Authentication:**
   - Settings → General → Security
   - Set authentication method (recommended: Forms with login page)
   - Create username/password

2. **Add Indexers:**
   - Indexers → Add Indexer
   - Search for public trackers (1337x, RARBG, etc.)
   - For private trackers, add API keys
   - Test each indexer after adding

3. **Connect to Apps:**
   - Settings → Apps → Add Application
   - Add Sonarr, Radarr, Lidarr
   - Use `http://sonarr:8989`, `http://radarr:7878`, `http://lidarr:8686`
   - Copy API keys from each app (Settings → General → Security → API Key)
   - Tags: Optional, for organizing indexers per app

4. **Sync Indexers:**
   - After adding apps, click "Sync App Indexers"
   - Indexers will automatically propagate to all connected apps

### Official Documentation

- **Prowlarr Wiki:** https://wiki.servarr.com/prowlarr
- **Quick Start Guide:** https://wiki.servarr.com/prowlarr/quick-start-guide
- **Indexer Setup:** https://wiki.servarr.com/prowlarr/indexers
- **App Integration:** https://wiki.servarr.com/prowlarr/settings#applications

---

## TV Shows (Sonarr)

**Access:** `http://localhost:8989`

### Initial Setup

1. **Media Management:**
   - Settings → Media Management
   - **Root Folders:** Add `/tv` (this maps to your TV media path)
   - **Episode Naming:** Enable "Rename Episodes"
   - **Standard Episode Format:** `{Series Title} - S{season:00}E{episode:00} - {Episode Title}`

2. **Connect Download Client:**
   - Settings → Download Clients → Add → qBittorrent
   - Host: `qbittorrent`
   - Port: `8080`
   - Username/Password: Your qBittorrent credentials
   - Category: `tv-sonarr`

3. **Indexers (via Prowlarr):**
   - Will auto-populate once Prowlarr is configured
   - Or manually add: Settings → Indexers → Add

4. **Quality Profiles:**
   - Settings → Profiles
   - Create/edit profiles based on your preferences
   - HD-1080p is a good default

5. **Add TV Shows:**
   - Series → Add New
   - Search for show
   - Select quality profile and root folder
   - Choose monitoring options (all episodes, future episodes, etc.)
   - Add and search

### Official Documentation

- **Sonarr Wiki:** https://wiki.servarr.com/sonarr
- **Quick Start Guide:** https://wiki.servarr.com/sonarr/quick-start-guide
- **Library Setup:** https://wiki.servarr.com/sonarr/library
- **Settings Explained:** https://wiki.servarr.com/sonarr/settings

---

## Movies (Radarr)

**Access:** `http://localhost:7878`

### Initial Setup

(Nearly identical to Sonarr)

1. **Media Management:**
   - Settings → Media Management
   - **Root Folders:** Add `/movies`
   - **Movie Naming:** Enable "Rename Movies"
   - **Standard Movie Format:** `{Movie Title} ({Release Year})`

2. **Connect Download Client:**
   - Settings → Download Clients → Add → qBittorrent
   - Host: `qbittorrent`
   - Port: `8080`
   - Category: `movies-radarr`

3. **Quality Profiles:**
   - Settings → Profiles
   - HD-1080p or higher recommended

4. **Add Movies:**
   - Movies → Add New
   - Search and add

### Official Documentation

- **Radarr Wiki:** https://wiki.servarr.com/radarr
- **Quick Start Guide:** https://wiki.servarr.com/radarr/quick-start-guide
- **Settings Guide:** https://wiki.servarr.com/radarr/settings

---

## Music (Lidarr)

**Access:** `http://localhost:8686`

### Initial Setup

1. **Media Management:**
   - Settings → Media Management
   - **Root Folders:** Add `/music`
   - **Track Naming:** Enable renaming
   - **Standard Track Format:** `{Artist Name} - {Album Title}/{track:00} - {Track Title}`

2. **Connect Download Client:**
   - Settings → Download Clients → Add → qBittorrent
   - Category: `music-lidarr`

3. **Metadata:**
   - Uses MusicBrainz for metadata
   - Tag quality settings to ensure proper metadata

4. **Add Artists:**
   - Artist → Add New
   - Search and add

### Official Documentation

- **Lidarr Wiki:** https://wiki.servarr.com/lidarr
- **Quick Start:** https://wiki.servarr.com/lidarr/quick-start-guide

---

## Content Requests (Jellyseerr)

**Access:** `http://localhost:5055`

### Initial Setup

1. **Sign In with Jellyfin:**
   - Use Jellyfin URL: `http://jellyfin:8096`
   - Sign in with Jellyfin admin account
   - This links Jellyseerr to your Jellyfin server

2. **Connect Sonarr:**
   - Settings → Services → Sonarr → Add Server
   - Server Name: `Sonarr`
   - Hostname/IP: `sonarr`
   - Port: `8989`
   - API Key: (from Sonarr → Settings → General → API Key)
   - Root Folder: `/tv`
   - Quality Profile: Your preferred profile

3. **Connect Radarr:**
   - Settings → Services → Radarr → Add Server
   - Similar to Sonarr setup above
   - Root Folder: `/movies`

4. **User Permissions:**
   - Settings → Users
   - Configure what users can request
   - Set request limits if desired

5. **Share with Users:**
   - Users can now request content via Jellyseerr
   - Approvals can be automatic or manual

### Official Documentation

- **Jellyseerr Docs:** https://docs.jellyseerr.dev/
- **Setup Guide:** https://docs.jellyseerr.dev/getting-started/installation

---

## Subtitles (Bazarr)

**Access:** `http://localhost:6767`

### Initial Setup

1. **Languages:**
   - Settings → Languages
   - Add languages you want subtitles for
   - Set defaults

2. **Connect Sonarr:**
   - Settings → Sonarr
   - Enable: Yes
   - Address: `http://sonarr:8989`
   - API Key: (from Sonarr)

3. **Connect Radarr:**
   - Settings → Radarr
   - Similar to Sonarr

4. **Subtitle Providers:**
   - Settings → Providers
   - Enable OpenSubtitles, Subscene, etc.
   - Add API keys if required

5. **Auto-Download:**
   - Settings → Subtitles
   - Configure automatic download settings

### Official Documentation

- **Bazarr Wiki:** https://wiki.bazarr.media/
- **Getting Started:** https://wiki.bazarr.media/Getting-Started/

---

## Container Management (Portainer)

**Access:** `https://localhost:9443`

### Initial Setup

1. **First-Time Setup:**
   - Create admin account (username/password)
   - Select "Docker" environment
   - Connect to local Docker

2. **View Containers:**
   - Home → local → Containers
   - See all running services
   - Start/stop/restart containers
   - View logs
   - Monitor resource usage

3. **Useful Features:**
   - **Logs:** Click container → Logs tab
   - **Stats:** View CPU, memory, network usage
   - **Console:** Access container terminal
   - **Recreate:** Update containers easily

### Official Documentation

- **Portainer Docs:** https://docs.portainer.io/
- **User Guide:** https://docs.portainer.io/user/docker

---

## Podcasts & Audiobooks (Audiobookshelf)

**Access:** `http://localhost:13378`  
**Setup:** Create account, add library folders for podcasts/audiobooks. Configure RSS feeds.  
**Full Guide:** [Audiobookshelf Docs](https://www.audiobookshelf.org/docs)

---

## Music Discovery (Slskd)

**Access:** `http://localhost:5030`  
**Setup:** Create password on first visit. Search for rare music. Can integrate with Lidarr.  
**Full Guide:** [Slskd GitHub](https://github.com/slskd/slskd)

---

## Music Tagging (Beets)

**Access:** `http://localhost:8337`  
**Setup:** Configure to watch Lidarr downloads. Auto-tags with MusicBrainz metadata.  
**Full Guide:** [Beets Documentation](https://beets.readthedocs.io/)

---

## Quick Reference: Service URLs

| Service | Port | URL | Purpose |
|---------|------|-----|---------|
| Dashboard | 80 | http://localhost/ | Central hub |
| Jellyfin | 8096 | http://localhost:8096 | Media streaming |
| Jellyseerr | 5055 | http://localhost:5055 | Content requests |
| Audiobookshelf | 13378 | http://localhost:13378 | Podcasts & audiobooks |
| Sonarr | 8989 | http://localhost:8989 | TV automation |
| Radarr | 7878 | http://localhost:7878 | Movie automation |
| Lidarr | 8686 | http://localhost:8686 | Music automation |
| Beets | 8337 | http://localhost:8337 | Music tagging |
| Bazarr | 6767 | http://localhost:6767 | Subtitles |
| qBittorrent | 8080 | http://localhost:8080 | Torrent client |
| Prowlarr | 9696 | http://localhost:9696 | Indexer manager |
| Slskd | 5030 | http://localhost:5030 | Soulseek music |
| Portainer | 9443 | https://localhost:9443 | Container management |

---

## Typical Workflow

1. **Add Indexers** in Prowlarr → Auto-sync to all *arr apps
2. **Add Content** in Sonarr/Radarr/Lidarr → Searches indexers
3. **Downloads** via qBittorrent → Auto-imported to libraries
4. **Jellyfin** scans and adds to library → Ready to watch
5. **Users** can request content via Jellyseerr

---

## Need Help?

- **Check Logs:** Use Portainer to view container logs
- **Official Discord/Forums:** Each app has community support
- **GitHub Issues:** Report bugs specific to this project
- **Documentation:** Links provided above for each service

---

## Tips

1. **Start Small:** Configure one service at a time
2. **Test Downloads:** Use a test file to verify the full pipeline
3. **Backup Configs:** The `configs/` folder contains all settings
4. **Monitor Resources:** Use Portainer to check CPU/RAM usage
5. **Update Regularly:** `docker-compose pull && docker-compose up -d`

---

*Last Updated: 2026-02-19*
