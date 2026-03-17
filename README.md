# 🎵 Lavalink Server

<div align="center">

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge)](LICENSE)

**A production-ready Lavalink audio streaming node for Discord bots with full Render deployment support**

[Features](#-features) • [Quick Deploy](#-quick-deploy-to-render) • [Manual Setup](#-manual-deployment) • [Configuration](#-configuration) • [Usage](#-usage) • [Security](#-security)

</div>

---

## ✨ Features

- 🚀 **One-Click Deploy** - Deploy to Render with a single click
- 📊 **Web Dashboard** - Beautiful real-time monitoring dashboard on the same port
- 🎵 **Multi-Source Support** - YouTube (with OAuth), Spotify, Apple Music, Deezer, SoundCloud, Bandcamp, Twitch, Vimeo
- 🔑 **YouTube OAuth** - Bypass age-restricted and login-required videos using a burner Google account
- 🔌 **Plugin System** - LavaSrc and youtube-plugin pre-configured
- 🎚️ **Audio Filters** - Equalizer, Karaoke, Timescale, Tremolo, Vibrato, Distortion, and more
- 📊 **Monitoring** - Optional Prometheus metrics and Sentry error tracking
- 🔒 **Secure** - Environment variable-based configuration, no hardcoded secrets
- 🐳 **Docker Ready** - Optimized Dockerfile with health checks
- ⚡ **Production Optimized** - JVM tuning and performance settings for Render
- 📝 **Comprehensive Logs** - File rotation and proper log management

## 🚀 Quick Deploy to Render

The easiest way to get started:

1. **Click the Deploy to Render button above** or use this link:
   ```
   https://render.com/deploy?repo=https://github.com/exeptionhandler/lavalink-server
   ```

2. **Configure your environment variables:**
   - `LAVALINK_SERVER_PASSWORD` - Set a strong password
   - Optional: Add Spotify, Apple Music, or Deezer credentials for extended functionality

3. **Deploy!** Render will automatically build and deploy your Lavalink server

4. **Get your connection details:**
   - Host: `your-service-name.onrender.com`
   - Port: `443` (HTTPS) or `80` (HTTP)
   - Password: The one you set in step 2
   - Dashboard: `https://your-service-name.onrender.com/`

## 🔑 YouTube OAuth Setup

This server uses the `TV` client with OAuth to bypass YouTube's login requirements (age-restricted videos, bot detection, etc.).

> ⚠️ **Use a burner Google account — NOT your main account!**

### First-Time Setup

1. **Deploy** the server without setting `YOUTUBE_REFRESH_TOKEN` yet
2. **Watch the logs** — you'll see something like:
   ```
   OAUTH INTEGRATION: To give youtube-source access to your account,
   go to https://www.google.com/device and enter code XXX-XXX-XXX
   ```
3. **Open** `https://www.google.com/device` and enter the code with your burner account — you have ~5 minutes
4. **Copy the refresh token** from the logs:
   ```
   Token retrieved successfully. Store your refresh token as this can be reused.
   (1//xxxxxxxxxxxxxxxxxxxxxxxxx...)
   ```
5. **Add it as an environment variable** in Render:
   - Key: `YOUTUBE_REFRESH_TOKEN`
   - Value: `1//xxxxxxxxxxxxxxxxxxxxxxxxx...`
6. **Redeploy** — the server will now load the token automatically on every start, no code needed again

### YouTube Clients Used

| Client | OAuth | Age-restriction | Playback |
|--------|-------|-----------------|----------|
| `MUSIC` | No | No | Search only (`ytmsearch:`) |
| `TV` | ✅ Yes | ✅ With OAuth | Yes + Livestream |
| `TVHTML5_SIMPLY` | No | No | Yes + Livestream (fallback) |

## 📊 Web Dashboard

This Lavalink server includes a beautiful web dashboard that displays real-time metrics and server information. The dashboard runs on the **same port** as the Lavalink server, making it perfect for Render's single-port requirement.

### Accessing the Dashboard

Simply visit the root URL of your server:
- **Local**: `http://localhost:2333/`
- **Render**: `https://your-service-name.onrender.com/`

### Dashboard Features

The dashboard displays:

- **Server Information**: Version, uptime, build details, git commit info
- **Memory Metrics**: Used, allocated, free, and reservable memory with visual progress bars
- **CPU Usage**: Process and system CPU usage with core count
- **Player Statistics**: Active players, playing status, guild counts
- **Frame Statistics**: Frames sent/lost per minute, loss percentage
- **System Details**: JVM version, OS, thread counts
- **Auto-refresh**: Updates every 5 seconds automatically
- **Status Indicators**: Color-coded health status (green/yellow/red)
- **Mobile Responsive**: Works perfectly on all devices

### Technical Architecture

```
┌─────────────────────────────────────────┐
│  External Request (Port 2333)           │
└─────────────────┬───────────────────────┘
                  │
                  ▼
         ┌────────────────┐
         │  Nginx Proxy   │
         └────────┬───────┘
                  │
         ┌────────┴────────┐
         │                 │
    ┌────▼─────┐    ┌─────▼────────┐
    │   HTML   │    │  Lavalink    │
    │ Dashboard│    │ (Port 8080)  │
    │   (/)    │    │   API/WS     │
    └──────────┘    └──────────────┘
```

- Root path `/` → Static dashboard files
- `/v4/*`, `/stats`, `/info`, `/version`, `/metrics` → Proxied to Lavalink
- WebSocket `/v4/websocket` → Proxied with WebSocket support

## 📦 Manual Deployment

### Using Docker

1. **Clone the repository:**
   ```bash
   git clone https://github.com/exeptionhandler/lavalink-server.git
   cd lavalink-server
   ```

2. **Create environment file:**
   ```bash
   cp .env.example .env
   # Edit .env and set your LAVALINK_SERVER_PASSWORD
   ```

3. **Build and run:**
   ```bash
   docker build -t lavalink-server .
   docker run -d \
     --name lavalink \
     -p 2333:2333 \
     -e LAVALINK_SERVER_PASSWORD=your_secure_password \
     lavalink-server
   ```

4. **Verify it's running:**
   ```bash
   curl http://localhost:2333/version
   ```

### Using Docker Compose

```yaml
version: '3.8'
services:
  lavalink:
    build: .
    ports:
      - "2333:2333"
    environment:
      - LAVALINK_SERVER_PASSWORD=your_secure_password
      - PORT=2333
      - YOUTUBE_REFRESH_TOKEN=your_refresh_token
    restart: unless-stopped
    volumes:
      - ./logs:/opt/Lavalink/logs
```

## ⚙️ Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `PORT` | No | `2333` | Server port |
| `LAVALINK_SERVER_PASSWORD` | **Yes** | - | Server password |
| `YOUTUBE_REFRESH_TOKEN` | No | - | YouTube OAuth refresh token (from burner account — see OAuth setup above) |
| `SPOTIFY_CLIENT_ID` | No | - | Spotify API client ID |
| `SPOTIFY_CLIENT_SECRET` | No | - | Spotify API client secret |
| `SPOTIFY_COUNTRY_CODE` | No | `US` | Spotify country code |
| `APPLE_MUSIC_API_TOKEN` | No | - | Apple Music API token |
| `APPLE_MUSIC_COUNTRY_CODE` | No | `US` | Apple Music country code |
| `DEEZER_MASTER_KEY` | No | - | Deezer master decryption key |
| `METRICS_ENABLED` | No | `false` | Enable Prometheus metrics |
| `SENTRY_DSN` | No | - | Sentry DSN for error tracking |
| `ENVIRONMENT` | No | `production` | Environment name |
| `YOUTUBE_PLAYLIST_LOAD_LIMIT` | No | `10` | Max tracks to load from playlists |
| `OPUS_ENCODING_QUALITY` | No | `10` | Opus encoding quality (0-10) |

### Getting API Credentials

**Spotify:**
1. Go to [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)
2. Create an app
3. Copy Client ID and Client Secret

**Apple Music:**
1. Visit [Apple Music API](https://developer.apple.com/documentation/applemusicapi/getting_keys_and_creating_tokens)
2. Generate a developer token

**Deezer:**
- Requires a master decryption key (advanced users only)

## 💻 Usage

### Connecting with Shoukaku

```javascript
const { Client } = require('discord.js');
const { Shoukaku, Connectors } = require('shoukaku');

const client = new Client({
  intents: ['Guilds', 'GuildVoiceStates']
});

const shoukaku = new Shoukaku(
  new Connectors.DiscordJS(client),
  [{
    name: 'Lavalink',
    url: 'your-service-name.onrender.com:443',
    auth: 'your_lavalink_password',
    secure: true
  }],
  {
    reconnectTries: 3,
    reconnectInterval: 5000,
    restTimeout: 60000
  }
);

shoukaku.on('ready', (name) => console.log(`Lavalink ${name} is ready!`));
client.login('your_discord_bot_token');
```

## 🌐 Available Endpoints

- `GET /` - Web dashboard
- `GET /version` - Lavalink version (health check)
- `GET /info` - Node information
- `GET /stats` - Node statistics
- `GET /metrics` - Prometheus metrics (if enabled)
- `WS /v4/websocket` - WebSocket for bot connections
- `GET /v4/*` - All v4 API endpoints

## 🔒 Security

1. **Strong Password**: Always use a strong, unique password
   ```bash
   openssl rand -base64 32
   ```
2. **Environment Variables**: Never commit `.env` files or hardcode secrets
3. **HTTPS**: Use HTTPS/WSS in production (Render provides this automatically)
4. **OAuth Token**: Store the YouTube refresh token only as an env var, never in code

## 🐛 Troubleshooting

### YouTube tracks fail with "This video requires login"

- OAuth is not configured yet → Follow the [YouTube OAuth Setup](#-youtube-oauth-setup) section
- The `YOUTUBE_REFRESH_TOKEN` env var is empty or invalid → Re-do the OAuth flow with a burner account
- Make sure the `TV` client is listed in your `application.yml`

### API Endpoints Return 502 Bad Gateway

- Lavalink backend is not running or failed to start
- Check logs for errors
- Verify `application.yml` configuration is correct
- Ensure `LAVALINK_SERVER_PASSWORD` is set

### Render Cold Start Timeout (Free Tier)

Render's free tier spins down after 15 minutes of inactivity. Startup can take 30-60 seconds.

```javascript
// Increase timeout in Shoukaku
{
  reconnectTries: 3,
  reconnectInterval: 5000,
  restTimeout: 60000  // 60 seconds
}
```

### Plugin Errors

- Check that environment variables for the specific plugin are set
- Review logs for specific error messages

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Lavalink](https://github.com/lavalink-devs/Lavalink) - The amazing audio streaming node
- [youtube-source](https://github.com/lavalink-devs/youtube-source) - YouTube plugin with OAuth support
- [LavaSrc](https://github.com/topi314/LavaSrc) - Multi-source plugin

## 📞 Support

- [Lavalink Documentation](https://lavalink.dev/)
- [Render Documentation](https://render.com/docs)
- [youtube-source Docs](https://github.com/lavalink-devs/youtube-source)

---

<div align="center">

**Made with ❤️ for the Discord bot community**

[⬆ Back to Top](#-lavalink-server)

</div>
