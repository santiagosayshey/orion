# Photos

Self-hosted photo and video management with Immich.

## Services

### Immich

Google Photos alternative with face recognition, smart search, and automatic organisation. Hardware-accelerated ML via Intel QuickSync.

- **Port:** 2283
- **Config:** `/mnt/user/appdata/immich`
- **Photos:** `/mnt/user/photos` (array storage)
- **Web UI:** `http://orion:2283`

### Immich Machine Learning

Runs face detection, CLIP embeddings, and smart search models. Caches models locally to avoid re-downloading.

- **Model Cache:** `/mnt/user/appdata/immich/model-cache`

### Immich Redis (Valkey)

Job queue and cache for Immich. Handles background task scheduling (thumbnail generation, ML processing, metadata extraction).

- **Image:** Valkey 9

### Immich Postgres

Database with vector search extensions (pgvectors, vectorchord) for CLIP-based smart search. Stored on cache/SSD for performance.

- **Data:** `/mnt/user/appdata/immich/postgres` (cache drive)
- **SHM:** 128MB
