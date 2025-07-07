# Minimal Homelab Docker Stack

This repository spins up:

* **Traefik** – reverse proxy (internal only)
* **Homepage** – start‑page dashboard
* **Cloudflare Tunnel** – exposes your services to the internet with HTTPS and no open ports


### How to grab your Cloudflare Tunnel token (GUI method)

Use this once, copy the token, paste it into .env as
CLOUDFLARE_TUNNEL_TOKEN=…, and you’re done.

  Step	What you’ll see / do
  1. Open Zero Trust	Go to https://one.dash.cloudflare.com. If it’s your first visit you’ll be asked to set a team name (e.g. hamza-team).
  2. Find “Tunnels”	In the left sidebar open Networks ▸ Tunnels.
  3. Create Tunnel	Click ➕ Create a tunnel → choose Cloudflared as connector → Next. Give it a friendly name like HomeLabTunnel and click Save tunnel.   ￼
  4. Get the Docker command	Zero Trust now shows an “Install and run connector” page. Pick Docker. It outputs something like:  docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJh...   ￼
  5. Copy only the token	Everything after --token (a long eyJh… base-64 string) is your tunnel token.
  6. (Optional) Add a hostname	Still in the wizard, click Route traffic → Public hostnames → set Subdomain = home and Domain = domain.com, Service HTTP, URL traefik:80 . Save. You can also skip this here and define hostnames later.
  7. Drop token into .env	ini\nCLOUDFLARE_TUNNEL_TOKEN=eyJh...\nHOMEPAGE_DOMAIN=home.3ux1n3.dev\n
  8. Start your stack	bash\ndocker compose up -d\n Cloudflared connects outbound, Traefik listens internally, and https://home.domain.com is live over HTTPS – no ports or certs needed.   ￼
  
  What if you need the token later?
  
  Zero Trust → Networks ▸ Tunnels ▸ HomeLabTunnel ▸ Connector tokens → click the copy icon.

---- 
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
