# 🏠 Homelab Infrastructure

Self-hosted infrastructure stack running on a Kali Linux ThinkPad server. All services are open source and managed via Docker Compose.

---

## 📋 Stack Overview

| Category | Service | Purpose |
|---|---|---|
| Management | Portainer | Docker UI |
| Network | Caddy | Reverse proxy + HTTPS |
| Network | Pi-hole | Network-wide ad blocker |
| Monitoring | Grafana | Dashboards |
| Monitoring | Prometheus | Metrics collection |
| Monitoring | Loki | Log aggregation |
| Monitoring | Uptime Kuma | Uptime monitoring |
| Security | Vaultwarden | Password manager |
| Security | CrowdSec | Threat detection |
| Productivity | Gitea | Self-hosted Git |
| Productivity | Wiki.js | Self-hosted wiki |
| Productivity | Nextcloud | Self-hosted cloud storage |
| Productivity | Filebrowser | Web file manager |
| Dev Tools | Woodpecker | CI/CD pipelines |
| Dev Tools | Zot Registry | Docker image registry |
| Dashboard | Homepage | Service dashboard |
| Database | PostgreSQL | Relational database |

---

## 🖥️ Hardware

- **Server:** ThinkPad — Kali Linux, i9-13950HX, 31GB RAM, NVIDIA RTX 2000 Ada
- **Access:** Windows Acer via Tailscale SSH

---

## 📁 Folder Structure

```
homelab/
├── management/        → Portainer
├── network/           → Caddy, Pi-hole
├── monitoring/        → Grafana, Prometheus, Loki, Uptime Kuma
├── security/          → Vaultwarden, CrowdSec
├── productivity/      → Gitea, Wiki.js, Nextcloud, Filebrowser
├── devtools/          → Woodpecker, Zot Registry
├── dashboard/         → Homepage
├── database/          → PostgreSQL
├── scripts/           → Setup and maintenance scripts
└── docs/              → Documentation
```

---

## 🚀 Deployment Roadmap

- **Phase 1** — DuckDNS + Docker + Caddy (go live)
- **Phase 2** — PostgreSQL + secrets management
- **Phase 3** — Gitea + Woodpecker CI/CD
- **Phase 4** — Grafana + Prometheus + Loki observability

---

## ⚙️ Prerequisites

- Kali Linux (Debian-based)
- Docker Engine + Docker Compose
- Tailscale (mesh VPN)
- DuckDNS domain

---

## 🛠️ Setup

### 1. Install Docker

```bash
bash scripts/install-docker.sh
```

### 2. Clone repo

```bash
git clone https://github.com/yourusername/homelab.git
cd homelab
```

### 3. Configure secrets

For each service, copy `.example.env` to `.env` and fill in values:

```bash
cp management/portainer/.example.env management/portainer/.env
```

### 4. Deploy a service

```bash
cd management/portainer
docker compose up -d
```

---

## 🔒 Security Notes

- All secrets stored in `.env` files — never committed to Git
- `.example.env` files contain dummy values only
- Internal services accessible via Tailscale only
- Public services protected by Caddy + HTTPS

---

## 📡 Networking

All services connected via Tailscale mesh VPN:

```
Public internet → Caddy (DuckDNS) → app services
Tailscale only  → Portainer, Gitea, Grafana, Woodpecker
```

---

## 📄 License

MIT — see [LICENSE](LICENSE)

---

## 🤝 Contributing

See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
