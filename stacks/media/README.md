# Media

The media automation stack — indexers, downloaders, and the arr suite.

## Services

### Prowlarr

Indexer manager that syncs with Radarr, Sonarr, and other arr apps. Configure indexers once here and they propagate everywhere.

- **Port:** 9696
- **Config:** `/mnt/user/appdata/prowlarr`
- **Web UI:** `http://orion:9696`

### Radarr

Movie collection manager. Integrates with Prowlarr for indexers and your download client for fetching.

- **Port:** 7878
- **Config:** `/mnt/user/appdata/radarr`
- **Media:** `/mnt/user/media`
- **Web UI:** `http://orion:7878`

### Sonarr

TV series collection manager. Integrates with Prowlarr for indexers and your download client for fetching.

- **Port:** 8989
- **Config:** `/mnt/user/appdata/sonarr`
- **Media:** `/mnt/user/media`
- **Web UI:** `http://orion:8989`

### Profilarr

Quality profile manager for the arr suite. Syncs custom formats and quality profiles across Radarr/Sonarr instances. Includes a parser service for CF/QP testing.

- **Port:** 6868
- **Config:** `/mnt/user/appdata/profilarr`
- **Web UI:** `http://orion:6868`
- **Image:** `develop` tag

### Seerr

Media request and discovery tool. Users can browse and request movies/TV shows which get sent to Radarr/Sonarr.

- **Port:** 5055
- **Config:** `/mnt/user/appdata/seerr`
- **Web UI:** `http://orion:5055`
### Tracearr

Real-time monitoring for Plex, Jellyfin, and Emby. Session tracking, sharing detection, stream analytics, and library insights in one dashboard.

- **Port:** 3000
- **Config:** `/mnt/user/appdata/tracearr`
- **Web UI:** `http://orion:3000`
- **Image:** `supervised` (all-in-one with bundled Postgres/Redis)

### qBittorrent

BitTorrent client. Used by Radarr/Sonarr as the download client.

- **Port:** 8080 (Web UI), 6881 (BitTorrent)
- **Config:** `/mnt/user/appdata/qbittorrent`
- **Downloads:** `/mnt/user/media/downloads`
- **Web UI:** `http://orion:8080`

### qui

Modern web interface for qBittorrent with multi-instance support, cross-seed, and automations.

- **Port:** 7476
- **Config:** `/mnt/user/appdata/qui`
- **Web UI:** `http://orion:7476`

### Theme

All services in this stack use [theme.park](https://theme-park.dev) with the **nord** theme via LinuxServer Docker Mods.