# Jellyfin Setup and Usage Guide

## What is Jellyfin?

Jellyfin is a free, open-source media server that lets you organize and stream your personal collection of movies, TV shows, music, photos, and more to any device.

## Access Jellyfin

### Via Tailscale (Recommended for Remote Access)
- URL: `http://media-server/view/` or `http://100.x.x.x/view/`
- Replace `100.x.x.x` with your Tailscale IP

### On Local Network
- URL: `http://localhost/view/` or `http://your-server-ip/view/`

### Direct Port Access
- URL: `http://localhost:8096` (only from the server itself)

## Initial Setup

### First Launch

1. Open Jellyfin in your web browser
2. Select your preferred language
3. Click **Next**

### Create Admin Account

1. Enter a username (e.g., `admin`)
2. Enter a strong password
3. Confirm password
4. Click **Next**

### Add Media Libraries

#### Movies Library
1. Click **Add Media Library**
2. Content type: **Movies**
3. Display name: `Movies`
4. Add folders:
   - Click **+** button
   - Navigate to `/media/Movies` (E: drive)
   - Click **OK**
   - Add `/media/drive/Movies` (F: drive)
5. Check **Enable real-time monitoring**
6. Click **OK**

#### TV Shows Library
1. Click **Add Media Library**
2. Content type: **Shows**
3. Display name: `TV Shows`
4. Add folders:
   - `/media/TV`
   - `/media/drive/TV`
5. Check **Enable real-time monitoring**
6. Click **OK**

#### Personal Library (Optional)
1. Click **Add Media Library**
2. Content type: Choose appropriate type
3. Display name: `Personal`
4. Add folder: `/media/Personal`
5. Click **OK**

### Configure Metadata Settings

1. Preferred metadata language: **English**
2. Country: **United States**
3. Click **Next**

### Remote Access (Optional)

Since we're using Tailscale, you can skip the remote access configuration.
- Uncheck **Allow remote connections**
- Click **Next**

### Finish Setup

1. Review your settings
2. Click **Finish**
3. Log in with your admin credentials

## Configuration

### Transcoding Settings (Important for Streaming)

1. Go to **Dashboard** (icon in top-right)
2. Navigate to **Playback** → **Transcoding**
3. Configure:
   - **Hardware acceleration**: None (or VAAPI/NVENC if you have GPU)
   - **Transcoding thread count**: 4 (adjust based on CPU)
   - **Enable VPP Tone mapping**: Off (unless using GPU)
   - **Allow encoding in HEVC format**: Yes
4. Click **Save**

### User Management

#### Add Family Members

1. Go to **Dashboard** → **Users**
2. Click **+** (Add User)
3. Enter name
4. Set password (or leave blank for guest)
5. Configure permissions:
   - **Enable media playback**: Yes
   - **Enable content deletion**: No (for family members)
   - **Enable live TV access**: As needed
   - **Allow remote access**: Yes
6. Select which libraries they can access
7. Click **Save**

### Network Settings

1. Go to **Dashboard** → **Networking**
2. Settings:
   - **Local network addresses**: `172.18.0.0/16` (Docker network)
   - **Published server URL**: Leave blank (handled by nginx)
   - **Enable automatic port mapping**: Off
3. Click **Save**

### Library Settings

1. Go to **Dashboard** → **Libraries**
2. For each library, configure:
   - **Automatically refresh metadata**: From file (recommended)
   - **Scan interval**: Every 12 hours
   - **Monitor folders for changes**: Yes

## Using Jellyfin

### Web Interface

#### Browsing Content

- **Home**: Shows suggested and recently added content
- **Movies**: Browse all movies
- **TV Shows**: Browse TV series
- **Search**: Search across all content (magnifying glass icon)

#### Playing Content

1. Click on any movie or show
2. Click **Play** button
3. Video will start in web player
4. Controls:
   - Space: Play/Pause
   - F: Fullscreen
   - M: Mute
   - Arrow keys: Seek forward/backward
   - Click progress bar: Jump to position

#### Subtitles

1. While playing, click the **CC** icon
2. Select desired subtitle track
3. To adjust subtitle appearance:
   - Click your profile icon → **Settings**
   - Go to **Subtitles**
   - Customize font, size, color, background

### Mobile Apps

#### iOS (iPhone/iPad)

1. Install **Jellyfin** from App Store
2. Open app
3. Add Server:
   - Server Address: `http://media-server/view/`
   - Or use your Tailscale IP: `http://100.x.x.x/view/`
4. Sign in with your credentials
5. Start browsing and streaming

#### Android

1. Install **Jellyfin** from Google Play Store
2. Open app
3. Add Server:
   - Server Address: `http://media-server/view/`
   - Or use your Tailscale IP: `http://100.x.x.x/view/`
4. Sign in with your credentials
5. Start browsing and streaming

### TV Apps

#### Android TV / Fire TV

1. Install **Jellyfin for Android TV** from respective app store
2. Launch app
3. Enter server address: `http://100.x.x.x/view/`
4. Sign in
5. Navigate using TV remote

#### Apple TV

1. Install **Jellyfin** from App Store
2. Connect to server
3. Sign in
4. Use Siri Remote to navigate

#### Roku

1. Add **Jellyfin** channel from Roku Channel Store
2. Enter server address
3. Sign in

### Desktop Apps

Available for Windows, Mac, and Linux from: [https://jellyfin.org/downloads](https://jellyfin.org/downloads)

## Features

### Collections

Group related movies together:

1. Select multiple movies (checkbox on hover)
2. Click **Group** (menu icon)
3. Create new collection
4. Give it a name (e.g., "Marvel Cinematic Universe")
5. Click **OK**

### Playlists

Create custom playlists:

1. Go to **Playlists** section
2. Click **+** to create new playlist
3. Name it
4. Add items by searching and selecting
5. Click **Save**

### Favorites

Mark items as favorites:
- Click the ♡ (heart) icon on any item
- View all favorites from the home page

### Continue Watching

Jellyfin automatically tracks your progress:
- Partially watched content appears in "Continue Watching"
- Progress syncs across all devices

### Live TV & DVR (Advanced)

If you have TV tuners, Jellyfin can integrate live TV:
1. Go to **Dashboard** → **Live TV**
2. Add tuner device
3. Configure guide source
4. Set up DVR rules for recording

## Tips & Best Practices

### File Naming

For best metadata matching:

**Movies**:
```
/Movies/Movie Name (Year)/Movie Name (Year).ext
Example: /Movies/Inception (2010)/Inception (2010).mkv
```

**TV Shows**:
```
/TV Shows/Show Name/Season XX/Show Name - SxxExx - Episode Name.ext
Example: /TV Shows/Breaking Bad/Season 01/Breaking Bad - S01E01 - Pilot.mkv
```

### Optimal Video Formats

- **Container**: MP4 or MKV
- **Video Codec**: H.264 (best compatibility) or H.265/HEVC (better compression)
- **Audio Codec**: AAC (best compatibility) or AC3/EAC3
- **Subtitles**: SRT (external) or embedded in MKV

### Performance Tips

1. **Enable Hardware Acceleration**: If you have Intel/NVIDIA/AMD GPU
2. **Limit Simultaneous Streams**: Set in user preferences
3. **Use Direct Play when possible**: Less transcoding = better performance
4. **Pre-transcode content**: For devices that always need transcoding

### Bandwidth Management

For remote streaming over Tailscale:

1. Go to user **Settings** → **Playback**
2. Set **Internet streaming quality**: 
   - 2 Mbps for mobile
   - 8 Mbps for tablets
   - 20 Mbps for desktop (if you have good upload speed)

## Troubleshooting

### Can't Connect to Server

1. Verify Jellyfin container is running:
   ```bash
   docker ps | grep jellyfin
   ```
2. Check if you're on Tailscale VPN
3. Try using direct IP: `http://100.x.x.x/view/`

### Video Won't Play

1. Check if file format is supported
2. Look at transcoding logs in Dashboard → Logs
3. Try lowering stream quality in playback settings
4. Ensure server has enough CPU/RAM

### Subtitles Not Appearing

1. Check if subtitle file is present:
   - External: `Movie.srt` in same folder
   - Embedded: Check with MediaInfo
2. Go to Dashboard → Plugins → **Catalog**
3. Install **Open Subtitles** plugin for auto-download
4. Configure in Dashboard → Plugins → **Open Subtitles**

### Metadata Not Matching

1. Ensure file names follow naming conventions
2. Refresh metadata:
   - Right-click item → **Identify**
   - Search by name or IMDb ID
   - Select correct match
3. Force refresh entire library:
   - Dashboard → Libraries → Click library
   - Click **Scan Library** → **Replace All Metadata**

### Buffering Issues

1. Lower stream quality in player settings (click gear icon)
2. Check server CPU usage in Dashboard
3. Enable hardware transcoding if available
4. Check network connection speed

## Advanced Features

### API Access

Jellyfin has a REST API for automation:
- API endpoint: `http://media-server/view/api`
- Get API key: Dashboard → API Keys
- Documentation: [https://api.jellyfin.org](https://api.jellyfin.org)

### Plugins

Extend Jellyfin functionality:

1. Go to **Dashboard** → **Plugins** → **Catalog**
2. Popular plugins:
   - **Trakt**: Sync watch history with Trakt
   - **Open Subtitles**: Auto-download subtitles
   - **TheTVDB**: Additional TV metadata
   - **AniDB**: Anime metadata
3. Install and configure as needed

### Webhooks

Trigger actions when events occur:

1. Install **Webhook** plugin
2. Configure endpoints for events like:
   - New content added
   - Playback started/stopped
   - User authenticated

## Security Recommendations

1. **Use strong passwords** for all accounts
2. **Limit admin accounts** - create regular users for family
3. **Review login activity** regularly in Dashboard → Activity
4. **Keep Jellyfin updated**:
   ```bash
   docker-compose pull jellyfin
   docker-compose up -d jellyfin
   ```
5. **Use Tailscale** instead of exposing to internet directly

## Support & Resources

- Official Documentation: [https://jellyfin.org/docs](https://jellyfin.org/docs)
- Community Forum: [https://forum.jellyfin.org](https://forum.jellyfin.org)
- Reddit: [r/jellyfin](https://reddit.com/r/jellyfin)
- Matrix Chat: #jellyfin:matrix.org
