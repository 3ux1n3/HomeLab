# Minimal Homelab Docker Stack

This repository spins up:

* **Traefik** – reverse proxy (internal only)
* **Homepage** – start‑page dashboard
* **Cloudflare Tunnel** – exposes your services to the internet with HTTPS and no open ports

## Quick start

```bash
git clone <repo> homelab
cd homelab
cp .env.example .env           # fill in your token + domain
docker compose up -d
```

Visit **https://home.DOMAIN**.

## Adding services

1. Put the new service on the `homelab_proxy` network.  
2. Give it Traefik labels (`Host(\`sub.example.com\`)`, `service port`).  
3. Add a *public hostname* for that subdomain in the same Cloudflare Tunnel.

Enjoy!
