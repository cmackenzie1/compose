# servarr

Data should be mounted and organized with the following structure:

```bash
tree /mnt/media/ -L 2
/mnt/media/
├── data
│   ├── movies
│   ├── torrents
│   └── tv
```

```bash
# pull the latest images
docker compose pull
# launch them
docker compose up -d
```
