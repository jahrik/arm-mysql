# arm-mysql

[![Build](https://github.com/jahrik/arm-mysql/actions/workflows/build.yml/badge.svg)](https://github.com/jahrik/arm-mysql/actions/workflows/build.yml)

Multi-arch MariaDB image backing the swarm stacks (Ghost's database, Grafana's database). Originally `arm64v8/mariadb:10.2`; now a pinned layer over the official `mariadb` LTS image.

## Run

```bash
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=secret jahrik/arm-mysql:latest
```

## Deploy (swarm)

```bash
make deploy   # stack: mysql, on ghost + monitor overlay networks
```

Credentials come from `MYSQL_ROOT_PASSWORD`/`MYSQL_DATABASE`/`MYSQL_USER`/`MYSQL_PASSWORD`; data persists at `/mnt/g1/mysql`.

## Build

```bash
make build
make push
```

CI: PR builds + connection check; merge to main pushes multi-arch (amd64/arm64) to Docker Hub. No armv7: modern MariaDB is 64-bit only.
