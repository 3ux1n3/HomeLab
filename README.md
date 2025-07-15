# HomeLab

A containerized homelab setup using Docker Compose with Cloudflare Tunnel for secure remote access.

## Services

- **Homepage** - Dashboard for your homelab services
- **Portainer** - Docker container management interface
- **Caddy** - HTTP reverse proxy (SSL handled by Cloudflare)
- **Cloudflare Tunnel** - Secure remote access without port forwarding

## Quick Start

1. Clone this repository
2. Copy `.env.example` to `.env` and fill in your values
3. Run `docker compose up -d`

## Configuration

### Environment Variables

Copy `.env.example` to `.env` and configure:

```env
DOMAIN=yourdomain.com
EMAIL=your-email@example.com
CLOUDFLARE_TUNNEL_TOKEN=your_tunnel_token_here
```

### Cloudflare Tunnel Setup

1. Create a tunnel in Cloudflare Zero Trust dashboard
2. Configure public hostname `home.yourdomain.com` pointing to `http://caddy:80`
3. Copy the tunnel token to your `.env` file

## Services Access

- Homepage: `https://home.yourdomain.com`
- Portainer: `https://home.yourdomain.com/portainer`

## Architecture

```
Internet → Cloudflare (SSL) → Tunnel → Caddy (HTTP) → Homepage/Portainer
```

Cloudflare handles SSL termination, while Caddy serves as an internal HTTP reverse proxy with path-based routing.

## File Structure

```
├── docker-compose.yml          # Main compose file with includes
├── compose/                    # Individual service definitions
│   ├── caddy.yml
│   ├── cloudflared.yml
│   ├── homepage.yml
│   └── portainer.yml
├── data/                       # Service data and configs
│   ├── caddy/
│   │   └── Caddyfile
│   └── homepage/
│       └── config/             # Homepage configuration
└── .env.example               # Environment variables template
```

## Adding New Services

1. Create a new compose file in `compose/` directory
2. Add it to the `include` section in `docker-compose.yml`
3. Configure Caddy to proxy to your new service in `data/caddy/Caddyfile`
4. Update your Cloudflare tunnel configuration if needed

## How to Get Your Cloudflare Tunnel Token

1. Go to [Cloudflare Zero Trust](https://one.dash.cloudflare.com)
2. Navigate to **Networks** → **Tunnels**
3. Click **Create a tunnel** → Choose **Cloudflared** → Give it a name
4. Copy the token from the Docker command (the long string after `--token`)
5. Add a public hostname: `home.yourdomain.com` → `http://caddy:80`
6. Paste the token into your `.env` file

## Features

- **Zero Port Forwarding**: All traffic goes through Cloudflare Tunnel
- **Automatic SSL**: Cloudflare handles certificates
- **Docker Management**: Portainer for remote container management
- **Path-based Routing**: Single domain, multiple services
- **Modular Design**: Easy to add/remove services
- **Environment-based**: Configuration through .env file