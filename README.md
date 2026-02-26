# Orion

Docker Compose stacks for Orion, my Unraid-based home server.

## Structure

Compose files live in `/stacks`, grouped by function rather than individual app. Services that need to communicate share a stack and network, reducing cross-stack networking complexity.
```
stacks/
├── media/          # arr stack, plex, etc.
├── ripping/        # ARM, makemkv
├── monitoring/     # grafana, prometheus, etc.
└── ...
```

Each stack contains a `compose.yml` and a `.env.example` template. Actual `.env` files live on the server only and are gitignored.

## Deployment

Merges to `main` trigger automatic deployment via GitHub Actions → Tailscale → SSH. Only changed stacks are redeployed.

## Updates

Renovate watches all compose files for Docker image updates and opens PRs automatically. Patch updates auto-merge, minor and major updates require manual approval.