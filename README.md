# 🎵 Spotify Proxy

A personal Spotify API proxy that you can deploy to your own Cloudflare Workers account. This allows you to poll your Spotify listening data and expose simple endpoints like `/now-playing` without having to worry about CORS issues or managing your own server.

## 🚀 Deploy Your Spotify Proxy

### Step 1: Deploy to Cloudflare Workers

**One-click deployment:**

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/abersager/spotify-proxy)

> **Note**: Cloudflare will automatically create a new repository for you during deployment

### Step 2: Get Your Cloudflare Credentials

You'll need these during deployment:

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

**💡 Tip**: Our [Setup Assistant](https://abersager.github.io/spotify-proxy/) can help extract your Account ID automatically from the URL!

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

After deployment:

### 1. **🔒 Configure Secrets**
- Visit your deployed worker URL
- You'll be redirected to `/credentials` for guided setup
- Generate a secure API key (or create your own)
- Set up all three secrets via Cloudflare Dashboard:
  - `API_KEY` - Your generated API key
  - `SPOTIFY_CLIENT_ID` - Your Spotify app's Client ID
  - `SPOTIFY_CLIENT_SECRET` - Your Spotify app's Client Secret

### 2. **Create Spotify App**
- Go to [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)
- Create a new app
- Note your **Client ID** and **Client Secret**
- Add callback URL: `https://your-worker-name.your-subdomain.workers.dev/callback`

### 3. **Complete OAuth Setup**
- After setting secrets, visit `/setup` to connect your Spotify account
- Authorize the app to access your Spotify data
- Test your endpoints!

### 4. **Start Using Your Proxy**
- `/now-playing` - Current track
- `/recent` - Recently played tracks
- `/health` - Status check

**🔐 Authentication Required**: All endpoints (except `/health`) require your API key via `Authorization: Bearer YOUR_API_KEY` header.



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
