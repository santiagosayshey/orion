# Gateway

Reverse proxy — the entry point for all services on the network.

DNS resolution for `.orion` domains is handled by dnsmasq on the router (GL-iNet Flint 2) via a wildcard rule (`address=/orion/192.168.8.239`). Traefik handles reverse proxying — routing requests to the correct service based on the `Host` header.

## How It Works

1. Browser asks the router: "what IP is `plex.orion`?" → dnsmasq replies with `192.168.8.239`
2. Browser connects to that IP on port 80
3. Traefik reads the `Host` header (`plex.orion`) and forwards to Plex on port 32400
4. Plex responds through Traefik back to the browser

## Services

### Traefik

Reverse proxy that listens on port 80 and routes requests to backend services based on the `Host` header. Services don't expose ports directly to the network — only Traefik does.

- **Port:** 80 (HTTP)
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

`.orion` is used as the domain suffix (matching the server name). `.local` is avoided as it conflicts with mDNS. All `*.orion` domains resolve to `192.168.8.239` via the router's dnsmasq wildcard rule.

## HTTPS

Not configured. Everything runs over plain HTTP on the local network. The browser shows a "not secure" warning but traffic never leaves the house. Options for later: self-signed certs, or a local CA via `mkcert`.

## Network

Traefik uses a `gateway` network. Other stacks that need Traefik routing should attach to this network.
