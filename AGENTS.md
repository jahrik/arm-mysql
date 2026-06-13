# AGENTS.md

Multi-arch MariaDB image: pinned `FROM` over official `mariadb` LTS, deployed as the `mysql` swarm stack (database for ghost and grafana).

## Commands

```bash
make build                                  # build jahrik/arm-mysql:latest
docker run -d -e MYSQL_ROOT_PASSWORD=test jahrik/arm-mysql:latest
make deploy                                 # swarm stack deploy (stack: mysql)
```

## CI

`build.yml`: Test (build + `healthcheck.sh --connect --innodb_initialized` poll) on PR; Release (buildx amd64/arm64 push to Docker Hub) on merge to main. Needs `DOCKERHUB_USERNAME`/`DOCKERHUB_TOKEN` secrets. No armv7 — MariaDB is 64-bit only.

## Quirks

- Bump MariaDB via the `FROM` tag; stay on LTS versions.
- `MYSQL_*` env vars still work as aliases for `MARIADB_*`.
- External `ghost` + `monitor` overlay networks and `/mnt/g1/mysql` gluster path — keep that wiring.
- Upgrading across majors (10.2 → 11.x) on existing data: the image auto-runs `mariadb-upgrade`; back up `/mnt/g1/mysql` first.
