# compose

A home for my docker compose stacks. Each stack should be relatively self-contained.

When possible, use volume mounts instead of bind mounts since they are mounted on a large NVMe drive.
Each volume should be named in the format: `<container>-data`.
