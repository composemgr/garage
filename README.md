## 👋 Welcome to garage 🚀

A distributed S3-compatible object storage system written in Rust

## 📋 Description

A distributed S3-compatible object storage system written in Rust

## 🚀 Services

- **app**: cloudlena/s3manager:latest (web management UI)
- **garage-server**: dxflrs/garage:latest (S3 API and cluster node)

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/garage/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/garage" ~/.local/srv/docker/garage
cd ~/.local/srv/docker/garage
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install garage
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
BASE_HOST_NAME=garage.example.com
DB_ADMIN_PASS=replace-with-a-random-password
RPC_SECRET=replace-with-a-random-password
S3_ACCESS_KEY=replace-with-a-random-password
S3_SECRET_KEY=replace-with-a-random-password
```

See `docker-compose.yaml` for complete list of configurable options.

Garage itself is configured by `./config/garage.toml`, which is mounted
read-only into the server container. That file holds the S3 root domain, the
RPC bind address, and the admin and K2V API bind addresses.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, management UI published on `172.17.0.1:64182`, S3 API on `3900` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Garage is object storage, so the default file publishes the S3 API port
directly on `3900`. The admin, RPC, K2V and S3 web ports (3901-3904) stay on
the project network only and are not published. The proxy variants publish no
ports at all; reach Garage over the proxy network instead.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64182
- **S3 API**: `http://<host>:3900`

## 🔐 Admin and internal ports

The Garage admin API (3903), RPC (3901), K2V (3904), and S3 web (3902)
listeners are not published to the host. Reach them from another container on
the `garage` network, or use `docker compose --env-file .env exec garage-server`
to drive `garage` commands against the local admin socket.

## 📂 Volumes

- `./config/garage.toml` - Garage node configuration
- `./volumes/data/garage` - Object data (`data_dir`)
- `./volumes/config/garage` - Cluster metadata (`metadata_dir`)

## 🔐 Security

- Change `DB_ADMIN_PASS` (admin token), `RPC_SECRET` and `S3_SECRET_KEY`
  before the first start
- `GARAGE_RPC_SECRET` is the cluster join secret; never share it
- The S3 API is published on plain HTTP in the default file; terminate TLS in
  front of it or restrict access to the host
- The admin API (3903) is not published; do not bind it to a host port
  without putting authentication in front of it
- S3 API keys are created per-bucket, not in this repo's env files
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
