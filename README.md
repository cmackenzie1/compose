# compose

A home for my docker compose stacks. Each stack should be relatively self-contained.

When possible, use volume mounts instead of bind mounts since they are mounted on a large NVMe drive.
Each volume should be named in the format: `<container>-data`.

## Maintenance

Every now and again the cruft leftover from upgrades and adhoc containers should be cleaned up. These commands can also be run with
the `-f` flag to force removal without confirmation.

```bash
docker system prune -a
docker image prune -a
docker system prune -a --volumes
docker system df
```

Finally, **and be very careful with this one**, delete volumes that are not actively in use. If for some reason a stack isn't running it could deletes its volumes!

```bash
docker volume prune -a
```
