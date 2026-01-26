## 👋 Welcome to garage 🚀

S3-compatible distributed object storage for self-hosted deployments

## 📋 Description

S3-compatible distributed object storage for self-hosted deployments

## 🚀 Services

- **manager**: cloudlena/s3manager:latest
- **server**: dxflrs/garage:bf4691d98afe348e528ee24e26b06c325cca35d0

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/garage/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/garage" ~/.local/srv/docker/garage
cd ~/.local/srv/docker/garage
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install garage
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
S3_ACCESS_KEY=changeme_s3_access_key
S3_SECRET_KEY=changeme_s3_secret_key
```

See `docker-compose.yaml` for complete list of configurable options.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:59094

## 📂 Volumes

- `./rootfs/config/garage.toml` - Data storage
- `./rootfs/data/garage` - Data storage
- `./rootfs/config/garage` - Data storage

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose logs -f manager
```

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# Update to latest images
docker compose pull && docker compose up -d

# View logs
docker compose logs -f

# Restart services
docker compose restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
