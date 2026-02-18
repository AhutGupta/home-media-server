# Navidrome Setup and Usage Guide

## What is Navidrome?

Navidrome is a modern music server and streamer compatible with Subsonic/Airsonic clients. It allows you to enjoy your music collection from anywhere, with a beautiful web interface and support for all major platforms.

## Access Navidrome

### Via Tailscale (Recommended for Remote Access)
- URL: `http://media-server/music/` or `http://100.x.x.x/music/`
- Replace `100.x.x.x` with your Tailscale IP

### On Local Network
- URL: `http://localhost/music/` or `http://your-server-ip/music/`

### Direct Port Access
- URL: `http://localhost:4533` (only from the server itself)

## Initial Setup

### First Launch

1. Open Navidrome in your web browser
2. You'll see the welcome screen

### Create Admin Account

1. **Username**: Choose your admin username (e.g., `admin`)
2. **Password**: Enter a strong password
3. **Confirm Password**: Re-enter password
4. Click **Create Admin**
5. You'll be automatically logged in

### First-Time Configuration

The system will automatically scan your music library located at:
- `/music/library` (E: drive - primary music folder)
- `/music/drive` (F: drive - additional music storage)

Initial scan may take a few minutes depending on library size.

## Configuration

### Settings

Access settings by clicking the ⚙️ (gear) icon in top-right corner.

#### General Settings

1. **Language**: English (or your preference)
2. **Theme**: Auto (follows system), Light, or Dark
3. **Notifications**: Enable to see scan progress

#### Playback Settings

1. **Audio Quality**:
   - **Max bitrate**: Unlimited (for local) or 320kbps (for mobile)
   - **Transcoding format**: MP3 (best compatibility) or OPUS (better quality)
2. **Gapless playback**: Enable for albums
3. **ReplayGain**: Auto (normalizes volume across tracks)

#### Advanced Settings

1. **Scrobbling**:
   - Connect Last.fm or ListenBrainz to track listening history
   - Go to Settings → **Integrations**
   - Enter API credentials
2. **Download folder**: Default is fine

## Using Navidrome

### Web Interface

#### Navigation

- **Albums**: Browse by album (default view)
- **Artists**: Browse by artist
- **Songs**: View all songs
- **Playlists**: Your custom playlists
- **Genres**: Filter by genre
- **Folders**: Browse file structure
- **Radios**: Internet radio stations (if configured)

#### Playing Music

1. **Browse** to any album, artist, or song
2. **Click play** button or double-click item
3. Player controls appear at bottom:
   - ⏮️ Previous track
   - ⏯️ Play/Pause
   - ⏭️ Next track
   - 🔀 Shuffle
   - 🔁 Repeat (off/all/one)
   - 🔊 Volume
   - ♡ Favorite current track

#### Queue Management

- **Current Queue**: Click queue icon (≡) to view
- **Add to Queue**: Right-click any item → **Add to queue**
- **Play Next**: Right-click → **Play next**
- **Clear Queue**: Click queue icon → Clear all
- **Shuffle Queue**: Click shuffle icon
- **Reorder**: Drag and drop in queue view

#### Creating Playlists

1. Click **Playlists** in sidebar
2. Click **+** (New Playlist)
3. Name your playlist
4. Click **Create**
5. Add songs:
   - Right-click any song/album/artist
   - Select **Add to playlist**
   - Choose your playlist

#### Search

1. Click 🔍 (search icon) in top bar
2. Type artist, album, or song name
3. Results appear as you type
4. Click any result to play or view details

#### Favorites

- Click ♡ (heart) icon on any item to favorite
- View favorites by clicking filter icon and selecting "Starred"

### Desktop Apps

#### Sonixd (Windows, Mac, Linux)

Best Subsonic client with modern UI:

1. Download from: [https://github.com/jeffvli/sonixd/releases](https://github.com/jeffvli/sonixd/releases)
2. Install and launch
3. Add server:
   - **Server**: `http://media-server/music` or `http://100.x.x.x/music`
   - **Username**: Your Navidrome username
   - **Password**: Your Navidrome password
   - **Type**: Navidrome
4. Click **Add Server**
5. Browse and play music with native desktop app

#### Supersonic (Cross-platform)

Modern Electron-based client:

1. Download from: [https://github.com/dweymouth/supersonic/releases](https://github.com/dweymouth/supersonic/releases)
2. Install and connect using same credentials
3. Enjoy native desktop experience

### Mobile Apps

#### iOS

**play:Sub** (Recommended)

1. Install from App Store
2. Open app
3. Add server:
   - Tap **+** to add server
   - **Server URL**: `http://media-server/music`
   - **Username**: Your username
   - **Password**: Your password
   - **Server Type**: Subsonic
4. Tap **Save**
5. Browse and stream music

**Amperfy** (Alternative)

- Open-source option
- Similar setup process
- Supports offline mode

#### Android

**Subtracks** (Recommended)

1. Install from Google Play Store
2. Open app
3. Settings → **Servers**
4. Add Server:
   - **Name**: Media Server
   - **Address**: `http://100.x.x.x/music`
   - **Username**: Your username
   - **Password**: Your password
   - **Test** connection
5. Save and start streaming

**Symfonium** (Premium)

- Best Android Subsonic client
- Paid app but worth it
- Supports offline caching, Android Auto, gapless playback

**DSub** (Free Alternative)

- Long-standing Subsonic client
- Good feature set
- Free with ads or one-time purchase

### Car Integration

#### Android Auto

1. Use **Symfonium** or **Subtracks**
2. Connect phone to car
3. Android Auto will show the music app
4. Control via car touchscreen or voice

#### Apple CarPlay

1. Use **play:Sub**
2. Connect iPhone to car
3. CarPlay shows music app
4. Browse and play via car interface

### Smart Speakers

#### Alexa

Use skill like "My Media" to connect Subsonic-compatible servers.

#### Google Home

Limited support - best to use Spotify/YouTube Music integration or cast from phone.

## Features

### Smart Playlists

Navidrome supports dynamic playlists based on criteria:

1. Go to **Playlists** → **New Smart Playlist**
2. Set rules:
   - Genre is Rock
   - Year between 1970-1990
   - Rating > 4 stars
   - Limit to 100 songs
3. Save and it auto-updates

### Star Ratings

Rate your music:
- Click on any track/album
- Click stars (1-5) to rate
- Use filters to find highly-rated music

### Recently Played

- View your listening history
- Click **Recently Played** in sidebar
- See what you've been enjoying

### Random Albums/Songs

- Click **Random** in sidebar
- Discover forgotten gems in your library
- Rediscover old favorites

### Sharing

Share playlists or albums with others:

1. Right-click playlist or album
2. Select **Share**
3. Set expiration time (optional)
4. Copy share link
5. Send to friends/family

**Note**: Shared links work even for users without accounts!

## Music Library Organization

### Recommended Folder Structure

```
/music/
  ├── Artist Name/
  │   ├── Album Name (Year)/
  │   │   ├── 01 - Track Name.mp3
  │   │   ├── 02 - Track Name.mp3
  │   │   ├── cover.jpg
```

### File Tags (Important!)

Navidrome relies on embedded metadata. Ensure files are properly tagged:

**Essential Tags**:
- Title
- Artist
- Album
- Album Artist (for compilations)
- Track Number
- Year
- Genre

**Optional but Recommended**:
- Cover Art (embedded)
- Disc Number (for multi-disc albums)
- Comment
- BPM
- Rating

### Supported Formats

- **MP3** - Most compatible
- **FLAC** - Lossless, best quality
- **M4A/AAC** - Good quality, small size
- **OGG/Opus** - Open format, good quality
- **WMA** - Windows Media Audio
- **WAV** - Uncompressed (large files)

### Cover Art

Navidrome looks for cover art in this order:

1. Embedded in audio file
2. `cover.jpg` in album folder
3. `folder.jpg` in album folder
4. Any `.jpg` file in folder

**Recommended**: 500x500 to 1000x1000 pixels, JPG format

## Administration

### User Management

1. Click your profile icon → **Users**
2. Click **New User**
3. Enter:
   - **Username**: User's name
   - **Name**: Display name
   - **Email**: For password recovery (optional)
   - **Password**: User password
   - **Admin**: Check only for admin users
4. Click **Save**

### Scanning Library

**Manual Scan**:
1. Go to Settings (⚙️)
2. Click **Scan Now**
3. Wait for completion notification

**Automatic Scanning**:
- Configured to scan every hour (`@every 1h`)
- Also monitors for real-time changes
- No action needed - just add music and wait

### Activity Monitoring

1. Click profile icon → **Activity**
2. View:
   - Current streams
   - Recent plays
   - User activity
   - Failed login attempts

### Transcoding

Navidrome can transcode on-the-fly:

- Automatically transcodes when needed
- Configured for 150MB cache
- Supports multiple formats
- No configuration needed - works automatically

## Tips & Best Practices

### For Best Performance

1. **Use MP3/FLAC**: Most compatible formats
2. **Proper tagging**: Ensures correct organization
3. **Include cover art**: Makes browsing pleasant
4. **Organize folders**: Keep music well-organized

### For Remote Streaming

1. **Enable transcoding**: Automatically done
2. **Use mobile apps**: Better than web for mobile
3. **Download favorites**: For offline listening
4. **Use lower bitrates**: On mobile data (128-192kbps)

### For Multi-User Setup

1. **Create separate accounts**: Don't share admin account
2. **Disable admin**: For regular users
3. **Monitor activity**: Check who's streaming what
4. **Set limits**: If bandwidth is concern

## Troubleshooting

### Music Not Showing Up

1. Check file formats are supported
2. Verify folders are mounted correctly:
   ```bash
   docker exec navidrome ls -la /music
   ```
3. Force rescan:
   - Settings → **Full Rescan**
4. Check container logs:
   ```bash
   docker logs navidrome
   ```

### Can't Connect from Mobile App

1. Verify you're on Tailscale VPN
2. Use full URL: `http://100.x.x.x/music`
3. Don't forget the `/music` path
4. Check credentials are correct
5. Try web interface first to confirm server works

### Playback Issues

1. **Buffering**: Lower bitrate in app settings
2. **Skipping**: Check network connection
3. **Won't play**: File format may need transcoding
4. Try different app/client

### Album Art Not Showing

1. Check if art is embedded or in folder
2. Verify file name is `cover.jpg` or `folder.jpg`
3. Re-scan library after adding art
4. Clear app cache and re-sync

### Tags Not Correct

1. Use a tag editor like:
   - **Mp3tag** (Windows)
   - **Kid3** (Linux/Mac)
   - **MusicBrainz Picard** (All platforms)
2. Fix tags
3. Re-scan library in Navidrome

## Advanced Features

### Last.fm Integration

Scrobble your plays to Last.fm:

1. Settings → **Integrations**
2. Click **Link with Last.fm**
3. Authorize Navidrome
4. Your plays will sync automatically

### ListenBrainz Integration

Open-source alternative to Last.fm:

1. Get API token from ListenBrainz
2. Settings → **Integrations**
3. Enter token
4. Save

### API Access

Navidrome implements Subsonic API:
- Endpoint: `http://media-server/music/rest/`
- Authentication: Username + token
- Documentation: [https://www.subsonic.org/pages/api.jsp](https://www.subsonic.org/pages/api.jsp)

### Jukebox Mode

Control music playback on the server:

1. Requires MPD or similar on server
2. Configure in Navidrome settings
3. Control server audio from any client

## Security Recommendations

1. **Strong passwords**: For all accounts
2. **Regular users**: Don't give everyone admin
3. **Session timeout**: Configured to 24 hours
4. **Use Tailscale**: Don't expose directly to internet
5. **Monitor activity**: Check for unusual access
6. **Update regularly**:
   ```bash
   docker-compose pull navidrome
   docker-compose up -d navidrome
   ```

## Support & Resources

- Official Website: [https://www.navidrome.org](https://www.navidrome.org)
- Documentation: [https://www.navidrome.org/docs](https://www.navidrome.org/docs)
- Reddit: [r/navidrome](https://reddit.com/r/navidrome)
- GitHub: [https://github.com/navidrome/navidrome](https://github.com/navidrome/navidrome)
- Discord: [Navidrome Discord](https://discord.gg/xh7j7yF)
