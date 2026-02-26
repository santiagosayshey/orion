# Ingest

Physical media ripping and encoding pipeline.

## Services

- **ARM** — Automatic Ripping Machine. Detects disc insertion, rips with MakeMKV, encodes with HandBrake via QuickSync H.265, renames, and drops in Radarr import folder.

## Hardware

- LG GP60NB50 USB DVD writer passed through at `/dev/sr0`
- DVD/CD only, no Blu-ray
- Intel i3-14100 iGPU (UHD 730) passed through at `/dev/dri` for QuickSync H.265 encoding
- USB connected — device path may change if unplugged

## Notes

- ARM web UI at port 8095
- Media output lands in `/mnt/user/media/arm`
- DVD rips are MPEG-2 source, encoding to H.265 recommended to save space

## TODO

- [ ] Configure HandBrake H.265 QSV preset
- [ ] Test disc detection in container