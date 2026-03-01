# Gateway

DNS and reverse proxy — the entry point for all services on the network.

Pi-hole handles DNS resolution (translating `plex.orion` to the server's IP), and Traefik handles reverse proxying (routing that request to the correct service and port). Together they replace raw `IP:port` access with clean domain names.

## How It Works

1. Browser asks Pi-hole: "what IP is `plex.orion`?" → Pi-hole replies with `192.168.8.239`
2. Browser connects to that IP on port 80
3. Traefik reads the `Host` header (`plex.orion`) and forwards to Plex on port 32400
4. Plex responds through Traefik back to the browser

## Services

### Pi-hole

DNS server and ad blocker. Every device on the network uses Pi-hole for DNS once the router's DNS setting points here.

- **Port:** 53 (DNS), 8053 (Web UI)
- **Config:** `/mnt/user/appdata/pihole`
- **Web UI:** `pihole.orion`

#### Local DNS Records

All custom domains are configured in Pi-hole's Local DNS section, pointing at the unRAID server IP (`192.168.8.239`):

| Domain | Service |
|---|---|
| `home.orion` | Glance dashboard |
| `unraid.orion` | unRAID web UI |
| `plex.orion` | Plex |
| `radarr.orion` | Radarr |
| `sonarr.orion` | Sonarr |
| `pihole.orion` | Pi-hole admin |
| `traefik.orion` | Traefik dashboard |

All records point to the same IP — Traefik differentiates by hostname.

#### Ad Blocking

Uses default blocklists only. If something breaks, check the query log for blocked (red) entries around the time of the issue and whitelist the domain.

### Traefik

Reverse proxy that listens on port 80 and routes requests to backend services based on the `Host` header. Services don't expose ports directly to the network — only Traefik does.

- **Port:** 80 (HTTP), 443 (HTTPS)
- **Config:** `/mnt/user/appdata/traefik`
- **Web UI:** `traefik.orion` (read-only dashboard for debugging)

#### Configuration

Traefik uses two config layers:

- **Static config** (`traefik.yml`) — entrypoints, Docker provider settings, dashboard toggle. Set once at startup, rarely changes.
- **Dynamic config** — routing rules from two sources:
  - **Docker labels** on containers (auto-discovered, appear/disappear with the container)
  - **File provider** (`dynamic.yml`) for non-Docker services like the unRAID web UI or the router admin page

#### Docker Labels

Other stacks route through Traefik by adding labels to their compose services:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.plex.rule=Host(`plex.orion`)"
  - "traefik.http.services.plex.loadbalancer.server.port=32400"
```

Traefik watches the Docker socket and creates/removes routes as containers start and stop.

## Domain Scheme

`.orion` is used as the domain suffix (matching the server name). `.local` is avoided as it conflicts with mDNS.

## HTTPS

Not configured. Everything runs over plain HTTP on the local network. The browser shows a "not secure" warning but traffic never leaves the house. Options for later: self-signed certs, or a local CA via `mkcert`.

## Network

Both services share a `gateway` network. Other stacks that need Traefik routing should attach to this network.
