# Tailscale Setup Guide

## What is Tailscale?

Tailscale creates a secure, private network (called a "tailnet") between your devices using WireGuard. This eliminates the need for port forwarding and makes your media server accessible from anywhere securely.

## Benefits Over Port Forwarding

- ✅ **No port forwarding needed** - Works from anywhere
- ✅ **End-to-end encryption** - All traffic is encrypted
- ✅ **No firewall changes** - Works through NAT and firewalls
- ✅ **Access control** - Manage which devices can access your server
- ✅ **Easy to use** - Just install and connect

## Prerequisites

1. A Tailscale account (free for up to 100 devices)
2. Docker installed on your media server
3. Admin access to your media server

## Step-by-Step Setup

### 1. Create Tailscale Account

1. Go to [https://login.tailscale.com/start](https://login.tailscale.com/start)
2. Sign up using Google, Microsoft, or GitHub account
3. Verify your email address

### 2. Generate Authentication Key

1. Log in to Tailscale admin console: [https://login.tailscale.com/admin](https://login.tailscale.com/admin)
2. Go to **Settings** → **Keys** → [https://login.tailscale.com/admin/settings/keys](https://login.tailscale.com/admin/settings/keys)
3. Click **Generate auth key**
4. Configure the key:
   - **Reusable**: Check if you want to use the same key multiple times
   - **Ephemeral**: Leave unchecked (you want the node to persist)
   - **Expiration**: 90 days is recommended
   - **Tags**: Optional, leave empty for now
5. Click **Generate key**
6. **Copy the key immediately** - you won't be able to see it again!

### 3. Configure Your Media Server

1. Open `compose_files/.env` in a text editor
2. Find the line `TS_AUTHKEY=`
3. Paste your auth key after the equals sign:
   ```env
   TS_AUTHKEY=tskey-auth-kXXXXXXXXXXXXXXXX
   ```
4. Save the file

### 4. Start Tailscale Container

```bash
cd compose_files
docker-compose up -d tailscale
```

### 5. Verify Connection

1. Check container logs:
   ```bash
   docker logs tailscale
   ```
   You should see "Logged in" or similar success message

2. Go to Tailscale admin console: [https://login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines)
3. You should see your "media-server" listed with a Tailscale IP (100.x.x.x)

### 6. Set Machine Name (Optional but Recommended)

1. In Tailscale admin console, find your media-server
2. Click the three dots (⋮) menu
3. Select **Edit machine name**
4. Change it to something memorable like `media-server` or `beelink-media`
5. Click **Save**

## Installing Tailscale on Client Devices

### Windows

1. Download from [https://tailscale.com/download/windows](https://tailscale.com/download/windows)
2. Run the installer
3. Sign in with the same account you used for the server
4. Tailscale will connect automatically

### macOS

1. Download from [https://tailscale.com/download/mac](https://tailscale.com/download/mac)
2. Open the DMG and drag Tailscale to Applications
3. Launch Tailscale and sign in
4. Click the Tailscale icon in menu bar to verify connection

### Linux

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

### iOS

1. Install from App Store: [Tailscale](https://apps.apple.com/app/tailscale/id1470499037)
2. Open the app and sign in
3. Enable the VPN when prompted

### Android

1. Install from Google Play: [Tailscale](https://play.google.com/store/apps/details?id=com.tailscale.ipn)
2. Open the app and sign in
3. Enable the VPN when prompted

## Accessing Your Media Server

Once Tailscale is installed on both server and client:

### Find Your Server's Tailscale IP

1. On the server, run:
   ```bash
   docker exec tailscale tailscale ip -4
   ```
   This will show your Tailscale IP (e.g., 100.x.x.x)

2. Or check the Tailscale admin console: [https://login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines)

### Access Services

Use your Tailscale IP with the nginx reverse proxy paths:

- **Jellyfin**: `http://100.x.x.x/view/`
- **Jellyseerr**: `http://100.x.x.x/request/`
- **Navidrome**: `http://100.x.x.x/music/`
- **Sonarr**: `http://100.x.x.x/sonarr/`
- **Radarr**: `http://100.x.x.x/radarr/`

Or use direct ports (localhost-only services require Tailscale):

- **qBittorrent**: `http://100.x.x.x:8080`
- **Prowlarr**: `http://100.x.x.x:9696`
- **Portainer**: `https://100.x.x.x:9443`
- **Dozzle**: `http://100.x.x.x:9999`

### Using MagicDNS (Recommended)

Tailscale provides MagicDNS which gives your devices easy-to-remember names:

1. Enable MagicDNS in Tailscale admin: [https://login.tailscale.com/admin/dns](https://login.tailscale.com/admin/dns)
2. Toggle **Enable MagicDNS**
3. Now you can use the machine name instead of IP:
   - `http://media-server/view/` for Jellyfin
   - `http://media-server/music/` for Navidrome
   - etc.

## Security Best Practices

### 1. Use Access Control Lists (ACLs)

Control which devices can access your server:

1. Go to Tailscale admin: [https://login.tailscale.com/admin/acls](https://login.tailscale.com/admin/acls)
2. Define rules to restrict access
3. Example ACL to allow only specific users:

```json
{
  "acls": [
    {
      "action": "accept",
      "users": ["your-email@example.com"],
      "ports": ["media-server:*"]
    }
  ]
}
```

### 2. Enable Key Expiry

- Always set an expiration on auth keys
- Regenerate keys periodically
- Disable unused keys in the admin console

### 3. Enable Two-Factor Authentication

1. Go to account settings: [https://login.tailscale.com/admin/settings/profile](https://login.tailscale.com/admin/settings/profile)
2. Enable 2FA for your Tailscale account

### 4. Monitor Connected Devices

Regularly review connected devices in the admin console and remove any you don't recognize.

## Troubleshooting

### Container won't start

1. Check if `/dev/net/tun` exists:
   ```bash
   ls -l /dev/net/tun
   ```
   If not, create it:
   ```bash
   sudo mkdir -p /dev/net
   sudo mknod /dev/net/tun c 10 200
   sudo chmod 600 /dev/net/tun
   ```

2. Check Docker has proper permissions:
   ```bash
   docker logs tailscale
   ```

### Can't connect from client

1. Verify Tailscale is running on server:
   ```bash
   docker ps | grep tailscale
   ```

2. Check Tailscale status on client:
   - Windows/Mac: Click Tailscale icon
   - Linux: `tailscale status`

3. Verify both devices are on the same tailnet in admin console

### Services not accessible

1. Verify nginx is running:
   ```bash
   docker ps | grep nginx
   ```

2. Test direct access to services using localhost from the server
3. Check nginx logs:
   ```bash
   docker logs nginx
   ```

## Advanced Configuration

### Exit Node (Optional)

The current configuration advertises this server as an exit node. This means you can route all your internet traffic through your home network.

To use it:
1. Enable the exit node in Tailscale admin for your media-server
2. On client device, select media-server as exit node in Tailscale settings

### Subnet Routing (Optional)

If you need to access other devices on your home network (not just the media server):

1. Find your home network subnet (usually 192.168.1.0/24 or similar)
2. Edit `docker-compose.yaml`:
   ```yaml
   - TS_EXTRA_ARGS=--advertise-routes=192.168.1.0/24
   ```
3. Restart Tailscale container
4. Approve the subnet in Tailscale admin console

## Maintenance

### Updating Tailscale

```bash
cd compose_files
docker-compose pull tailscale
docker-compose up -d tailscale
```

### Regenerating Auth Key

When your auth key expires:

1. Generate a new key in Tailscale admin console
2. Update `TS_AUTHKEY` in `.env`
3. Restart container:
   ```bash
   docker-compose restart tailscale
   ```

## Support

- Tailscale Documentation: [https://tailscale.com/kb](https://tailscale.com/kb)
- Tailscale Community: [https://forum.tailscale.com](https://forum.tailscale.com)
- Tailscale Support: [support@tailscale.com](mailto:support@tailscale.com)
