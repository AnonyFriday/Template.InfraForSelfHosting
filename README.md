# Infrastructure and Apps Port Registry

## Network Workflows

### Public Access Flow (Cloudflare Tunnel)

```mermaid
graph LR
    User["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/chrome.png' width='14'/> Public Internet User"] -->|https://blog.anonyfriday.io.vn| CFEdge["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/cloudflare.png' width='14'/> Cloudflare Edge"]
    CFEdge -->|Secure Tunnel Connection| CFTunnel["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/cloudflare.png' width='14'/> Cloudflared Container"]
    CFTunnel -->|Direct Forward| Ghost["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/ghost.png' width='14'/> Ghost Container: Port 9100"]
    Ghost -->|Internal Network| MySQL["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/mysql.png' width='14'/> MySQL Container: Port 9000"]
```

### Private Access Flow (Tailscale VPN + Caddy)

```mermaid
graph TD
    Admin["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/tailscale.png' width='14'/> Tailscale VPN User"] -->|https://*.internal.anonyfriday.io.vn| Caddy["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/caddy.png' width='14'/> Caddy Container: Ports 80/443"]
    Caddy -->|Reverse Proxy| Portainer["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/portainer.png' width='14'/> Portainer: 8001"]
    Caddy -->|Reverse Proxy| Homepage["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/homepage.png' width='14'/> Homepage: 8002"]
    Caddy -->|Reverse Proxy| Filebrowser["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/filebrowser.png' width='14'/> Filebrowser: 8003"]
    Caddy -->|Reverse Proxy| ITTools["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/it-tools.png' width='14'/> IT-Tools: 8004"]
    Caddy -->|Reverse Proxy| TSDProxy["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/tailscale.png' width='14'/> TSDProxy: 8000"]
    Caddy -->|Reverse Proxy| OpenPortFinder["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/docker.png' width='14'/> Open-Port-Finder: 56789"]
    
    %% SSL Verification
    CF_API["<img src='https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/png/cloudflare.png' width='14'/> Cloudflare DNS API"] <-->|ACME DNS Challenge| Caddy
```

## Infra Services (8000+)

| Port | Service | Subdomain | Note |
|:---|:---|:---|:---|
| 8000 | TSDProxy | `tsdproxy.internal.anonyfriday.io.vn` | |
| 8001 | Portainer | `portainer.internal.anonyfriday.io.vn` | |
| 8002 | Homepage | `internal.anonyfriday.io.vn` | Root domain proxy |
| 8003 | Filebrowser | `filebrowser.internal.anonyfriday.io.vn` | |
| 8004 | IT-Tools | `it-tools.internal.anonyfriday.io.vn` | |
| 8006 | Grafana | - | Skip (Empty Config) |
| 8007 | Fail2ban | - | Skip (Empty Config) |

## App Services (9000+)

| Port | Service | Subdomain / Database | Note |
|:---|:---|:---|:---|
| 9000 | MySQL | - | Active (Used by Ghost) |
| 9001 | PostgreSQL | - | Skip (Empty Config) |
| 9002 | Redis | - | Skip (Empty Config) |
| 9003 | SQL Server | - | Skip (Empty Config) |
| 9100 | Ghost | `blog.anonyfriday.io.vn` | Active |
| 9200 | App Frontend | - | Skip (Empty Config) |
| 9300 | App Gateway | - | Skip (Empty Config) |

## Exceptions (Host Network Mode)

| Port | Service | Description |
|:---|:---|:---|
| 80 | Caddy HTTP | External HTTP traffic proxy |
| 443 | Caddy HTTPS | External HTTPS traffic proxy with TLS |
| 56789 | Open-Port-Finder | Scans open host ports |
| - | Cloudflared | Establishes Cloudflare Tunnel connection |

---

## DNS Challenge (Private HTTPS)

DNS-01 challenge allows Caddy to obtain trusted SSL/TLS certificates (Let's Encrypt / ZeroSSL) **without public internet exposure**.

1. Caddy requests certificate for `*.internal.anonyfriday.io.vn`.
2. Caddy API automatically creates temporary `_acme-challenge` TXT record in Cloudflare.
3. Certificate Authority checks TXT record to verify domain ownership.
4. Caddy deletes TXT record and loads verified SSL certificate.
5. Tailscale clients access services securely over HTTPS.

---

## Domain Configuration

### 0. Registrar Setup (Matbao)
Delegate DNS management of `anonyfriday.io.vn` to Cloudflare:
1. Log into **Matbao console**.
2. Change Nameservers (NS) to your **Cloudflare Nameservers** (e.g., `*.ns.cloudflare.com`).
3. Manage all records inside **Cloudflare Dashboard**.

### 1. Public Routing (Cloudflare Tunnel)
Expose public application directly to the internet:
* **Cloudflare Tunnel settings:** Route `blog.anonyfriday.io.vn` → `http://localhost:9100` (Ghost).
* **DNS:** Managed automatically by Cloudflare Tunnel CNAME.

### 2. Private Routing (Tailscale + Caddy)
Route private traffic inside your VPN:
* **A Record:** Point `*.internal` subdomain to your host's Tailscale IP (e.g., `100.x.y.z`).
* **Proxy Status:** **DNS Only (Grey Cloud)**. *Do not proxy (Orange Cloud) internal IPs.*

---

## Service Screenshots

Here are screenshots of the running services (stored in the `images/` directory).

<table>
  <tr>
    <td width="50%" align="center">
      <b>Homepage</b><br>
      <img src="./images/homepage.png" alt="Homepage" width="100%">
    </td>
    <td width="50%" align="center">
      <b>Portainer</b><br>
      <img src="./images/portainer.png" alt="Portainer" width="100%">
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <b>Filebrowser</b><br>
      <img src="./images/filebrowser.png" alt="Filebrowser" width="100%">
    </td>
    <td width="50%" align="center">
      <b>IT-Tools</b><br>
      <img src="./images/it-tools.png" alt="IT-Tools" width="100%">
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <b>Ghost Blog (Public)</b><br>
      <img src="./images/ghost.png" alt="Ghost Blog" width="100%">
    </td>
    <td width="50%" align="center">
      <b>Open-Port-Finder</b><br>
      <img src="./images/open-port-finder.png" alt="Open-Port-Finder" width="100%">
    </td>
  </tr>
</table>
