# 🎵 Spotify Proxy

A personal Spotify API proxy that you can deploy to your own Cloudflare Workers account. This allows you to poll your Spotify listening data and expose simple endpoints like `/now-playing` without having to worry about CORS issues or managing your own server.

## 🚀 Deploy Your Spotify Proxy

### Step 1: Deploy to Cloudflare Workers

**One-click deployment:**

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/abersager/spotify-proxy)

> **Note**: Cloudflare will automatically create a new repository for you during deployment

### Step 2: Complete Setup

After deployment, **visit your worker URL** and you'll be automatically guided through the complete setup process - no additional tools needed!

### Cloudflare Credentials (for deployment)

You'll need these during the initial deployment:

#### 🔑 Cloudflare API Token

1. Go to [https://dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens)
2. Click "Create Token" → "Get started" (Custom token)
3. Configure your token:
   - **Token name**: `spotify-proxy`
   - **Permissions**:
     - Account → **Workers Scripts** → **Edit**
     - Account → **Workers KV Storage** → **Edit**
   - **Account Resources**: Include All accounts
4. Click "Continue to summary" → "Create Token" → **Copy the token**

#### 🆔 Account ID (Easy Method)

1. Go to [https://dash.cloudflare.com](https://dash.cloudflare.com)
2. After login, copy the URL from your browser address bar
3. Extract the Account ID from the URL (e.g., `https://dash.cloudflare.com/abc123.../home`)

**💡 Tip**: You can find your Account ID in the URL after logging into Cloudflare Dashboard

## ✨ Features

- **One-click deployment** - Cloudflare automatically creates your repository
- **No ongoing cost** (free tier is sufficient)
- **🔒 Built-in API key authentication** - secure your proxy with encrypted secrets
- **Web-based setup** - no CLI required, all configuration via Cloudflare Dashboard
- **Secure OAuth flow** for Spotify authentication
- **Cloudflare secrets** for secure credential storage
- **Simple API endpoints** for current track, recent tracks, and more
- **Beautiful setup UI** for easy configuration

## 🎯 Setup Process

After deployment, **simply visit your worker URL** - you'll be automatically guided through the entire setup:

### 1. **🔒 Automated Secret Setup**
- Worker automatically redirects you to `/credentials`
- Generates a secure API key for you (or create your own)
- Provides step-by-step instructions for setting secrets in Cloudflare Dashboard
- All three secrets configured via web interface:
  - `API_KEY` - Your generated API key
  - `SPOTIFY_CLIENT_ID` - Your Spotify app's Client ID
  - `SPOTIFY_CLIENT_SECRET` - Your Spotify app's Client Secret

### 2. **🎵 Spotify App Creation**
- Worker provides direct links to [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)
- Shows you exactly which callback URL to use
- Clear instructions for getting your credentials

### 3. **✅ One-Click OAuth**
- Worker guides you through connecting your Spotify account
- Handles all OAuth complexity automatically
- Immediate confirmation when setup is complete

### 4. **🚀 Ready to Use**
- All endpoints immediately available:
  - `/now-playing` - Current track
  - `/recent` - Recently played tracks
  - `/health` - Status check

**🔐 Authentication**: All endpoints (except `/health`) require your API key via `Authorization: Bearer YOUR_API_KEY` header.

> **✨ No external tools required** - everything is handled through the worker's built-in setup interface!



## 🔧 Development

To run the development server locally:

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

The worker will be available at `http://localhost:8787`

## 🛠 API Endpoints

| Endpoint | Description |
|----------|-------------|
| `/` | Home page with links to setup and endpoints |
| `/setup` | OAuth setup GUI |
| `/callback` | OAuth callback (don't call directly) |
| `/now-playing` | Current track and playback state |
| `/recent` | Recently played tracks (last 10) |
| `/health` | Health check and configuration status |

### Example Responses

#### `/now-playing`
```json
{
  "is_playing": true,
  "item": {
    "name": "Song Name",
    "artists": [{"name": "Artist Name"}],
    "album": {
      "name": "Album Name",
      "images": [{"url": "https://..."}]
    },
    "external_urls": {
      "spotify": "https://open.spotify.com/track/..."
    }
  },
  "progress_ms": 45000,
  "device": {
    "name": "Device Name",
    "type": "Computer"
  }
}
```

#### `/recent`
```json
{
  "items": [
    {
      "track": {
        "name": "Song Name",
        "artists": [{"name": "Artist Name"}],
        "album": {"name": "Album Name"}
      },
      "played_at": "2024-01-01T12:00:00Z"
    }
  ]
}
```

## 🔐 Security

- OAuth tokens are stored temporarily in Cloudflare KV
- All credentials stored as encrypted Cloudflare Workers secrets
- All API calls are server-side to prevent credential exposure
- CORS headers are properly configured for web applications

## 📖 Configuration

The worker uses the following secrets and environment variables:

**Secrets** (configured via Cloudflare Dashboard):
- `API_KEY` - Your secure API key for authentication
- `SPOTIFY_CLIENT_ID` - Your Spotify app's client ID
- `SPOTIFY_CLIENT_SECRET` - Your Spotify app's client secret

**Environment Variables**:
- `ENVIRONMENT` - Environment name (set in wrangler.toml)

## 🤝 Contributing

Feel free to submit issues and pull requests!

## 📝 License

MIT License - see LICENSE file for details

## 🔑 Need Help?

If you encounter issues:

1. **Deployment fails**: Check that your Cloudflare credentials are correct
2. **Worker not accessible**: Wait a few minutes for DNS propagation
3. **OAuth errors**: Make sure your Spotify app callback URL matches your worker URL
4. **Still stuck**: Open an issue in this repository



## 🛟 Troubleshooting

### Deployment Issues

1. **"Invalid API token"** - Regenerate your Cloudflare API token with proper Worker permissions
2. **"Account ID not found"** - Double-check your Account ID from Cloudflare dashboard
3. **"Worker name conflict"** - Choose a different worker name during deployment

### Setup Issues

1. **"No valid tokens found"** - Complete the credential setup at `/credentials` first
2. **"Invalid Client ID format"** - Ensure you're using the correct Spotify Client ID (32+ characters)
3. **"Callback URL mismatch"** - Ensure your Spotify app's redirect URI matches your worker's `/callback` endpoint
4. **"OAuth failed"** - Check your Spotify credentials and callback URL configuration

### Getting Help

- Check the `/health` endpoint for configuration status
- Look at the browser console for any error messages
- Verify your Spotify app settings match your worker URL
- Ensure your worker has the latest code deployed

---

Made with ❤️ for the Spotify community
