# Ingest Stack

Lightweight CLI ripping and encoding container for optical media.

## Tools

- **MakeMKV** (`makemkvcon`) — rips Blu-ray/DVD to lossless MKV
- **HandBrakeCLI** — encodes with H.265 via Intel QSV hardware acceleration

## Usage

```bash
# Enter the container
docker exec -it ripper bash

# Rip entire disc
makemkvcon mkv disc:0 all /data/rips

# Encode with QSV H.265
HandBrakeCLI -i /data/rips/movie.mkv -o /data/encodes/movie.mkv -e qsv_h265 -q 22 --encoder-preset balanced
```

## Manual Import

After encoding, move files to the appropriate download folder for Sonarr/Radarr:

```bash
mv /data/encodes/movie.mkv /data/downloads/movies/
mv /data/encodes/show.mkv /data/downloads/tv/
```

Then trigger a manual import in Sonarr/Radarr pointing at the downloads folder.

## Volumes

| Container Path | Host Path | Purpose |
|---|---|---|
| `/data` | `/mnt/user/media` | Entire media share |

## Devices

| Device | Purpose |
|---|---|
| `/dev/sr0` | Optical drive |
| `/dev/dri` | Intel QSV hardware encoding |