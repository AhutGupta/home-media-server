# Quick Start Guide

## 🚀 What You Get

Your home media server with:
- **Jellyfin** - Stream movies/TV shows
- **Navidrome** - Stream your music library
- **Sonarr/Radarr** - Automatic TV/movie downloads
- **qBittorrent** - Torrent client
- **Tailscale** - Secure remote access (no port forwarding!)
- **nginx Dashboard** - Beautiful landing page for all services

## 📱 Mobile Apps - YES, They Work!

### With Tailscale, Your Apps Work From Anywhere

**Jellyfin App** (Android/iOS)
- Install from App Store or Google Play
- Server URL: `http://100.x.x.x/view/`
- Works perfectly with Tailscale!

**Navidrome Music Apps**
- **Android**: Symfonium (recommended), Subtracks, DSub
- **iOS**: play:Sub (recommended), Amperfy
- Server URL: `http://100.x.x.x/music`
- Type: Subsonic/Navidrome

**Get Your Tailscale IP**
```bash
docker exec tailscale tailscale ip -4
```
This shows your `100.x.x.x` IP - use this in ALL your apps!

## 🌐 Accessing Services

### Option 1: Beautiful Dashboard (Recommended)
Open your browser to: `http://100.x.x.x/` or `http://media-server/`

You'll see a gorgeous dashboard with:
- All services with descriptions
- Quick links to web UIs
- Mobile app instructions
- Status indicators

### Option 2: Direct Access
- **Jellyfin**: `http://100.x.x.x/view/`
- **Navidrome**: `http://100.x.x.x/music/`
- **Jellyseerr**: `http://100.x.x.x/request/`
- **Sonarr**: `http://100.x.x.x/sonarr/`
- **Radarr**: `http://100.x.x.x/radarr/`
- **qBittorrent**: `http://100.x.x.x:8080`
- **Prowlarr**: `http://100.x.x.x:9696`
- **Portainer**: `https://100.x.x.x:9443`

## 🎯 What nginx Does For You

nginx is your **smart gateway** providing:

✅ **Single Entry Point** - All services through port 80
✅ **Beautiful Dashboard** - Landing page at `/`
✅ **Security Headers** - XSS, clickjacking protection
✅ **Rate Limiting** - Prevents abuse
✅ **Caching** - Faster load times for images/CSS
✅ **WebSocket Support** - Real-time features for Jellyfin/Navidrome
✅ **Compression** - Faster page loads with gzip
✅ **Health Checks** - Monitor service status at `/health`

### What Makes This Special

- **No configuration needed** - Works out of the box
- **Mobile-friendly** - Responsive design
- **Fast** - Cached assets, compression enabled
- **Secure** - Rate limiting, security headers, no version disclosure
- **Professional** - Clean URLs, organized layout

## 🛠️ First Time Setup

### 1. Get Tailscale Running
```bash
# Get auth key from: https://login.tailscale.com/admin/settings/keys
# Add to compose_files/.env:
TS_AUTHKEY=tskey-auth-YOUR-KEY-HERE

# Start services
cd compose_files
docker-compose up -d
```

### 2. Find Your IP
```bash
docker exec tailscale tailscale ip -4
```

### 3. Open Dashboard
Open browser: `http://100.x.x.x/`

### 4. Setup Each Service
Follow the dashboard links to set up:
- Jellyfin admin account
- Navidrome admin account  
- qBittorrent password (change from admin/adminadmin!)
- Connect Sonarr/Radarr to qBittorrent
- Add indexers via Prowlarr

## 📲 Mobile App Setup Examples

### Jellyfin on iPhone
1. Install "Jellyfin" from App Store
2. Open app, tap "Add Server"
3. Enter: `http://100.64.x.x/view/`
4. Sign in with your credentials
5. Done! Stream from anywhere

### Symfonium for Music (Android)
1. Install "Symfonium" from Play Store
2. Open app, go to Settings
3. Add Server:
   - Name: Home Server
   - Type: Navidrome
   - URL: `http://100.64.x.x/music`
   - Username/Password: Your Navidrome credentials
4. Sync and enjoy!

### qBittorrent Remote
1. Open any mobile browser
2. Go to: `http://100.64.x.x:8080`
3. Login and manage torrents
4. Or use remote control apps from app stores

## 💡 Pro Tips

**For Family Sharing**
- Install Tailscale on family devices
- Share your Tailscale IP
- They can use Jellyfin/Navidrome apps from anywhere

**For Best Performance**
- Enable hardware transcoding in Jellyfin (if you have GPU)
- Keep media files well-organized
- Use the dashboard to quickly jump between services

**For Security**
- Change ALL default passwords
- Use strong Tailscale account password with 2FA
- Regularly update containers: `docker-compose pull && docker-compose up -d`

**For Convenience**
- Bookmark the dashboard on all devices
- Add home screen shortcuts on mobile
- Enable MagicDNS in Tailscale to use `media-server` instead of IP

## 🆘 Quick Troubleshooting

**Can't access from phone?**
- Make sure Tailscale is running on phone
- Check both devices show as "connected" in Tailscale admin
- Verify you're using correct IP: `docker exec tailscale tailscale ip -4`

**Dashboard not loading?**
- Check nginx is running: `docker ps | grep nginx`
- View logs: `docker logs nginx`
- Restart: `docker-compose restart nginx`

**Services not responding?**
- Check all containers: `docker ps`
- Restart specific service: `docker-compose restart jellyfin`
- View logs: `docker logs <service-name>`

**Mobile app won't connect?**
- Double-check URL format (include `/view/` or `/music`)
- Try browser first to confirm service works
- Check Tailscale is connected on mobile device

## 🎬 What Makes This Setup Awesome

1. **No Port Forwarding** - Secure access via Tailscale VPN
2. **Professional Dashboard** - Not just a list of IPs and ports
3. **Mobile-First** - Apps work seamlessly
4. **Automated** - Sonarr/Radarr handle downloads automatically
5. **Optimized** - Caching, compression, rate limiting built-in
6. **Family-Friendly** - Easy for others to use
7. **Secure** - Multiple layers of protection
8. **Fast** - nginx optimization makes everything snappy

## 📚 Need More Details?

- **Tailscale Setup**: See `TAILSCALE_SETUP.md`
- **Service Guides**: Check `guides/` folder for detailed setup instructions
- **Full README**: See `README.md` for complete documentation

---

**You're all set!** Enjoy your professional home media server. 🍿🎵🎬
