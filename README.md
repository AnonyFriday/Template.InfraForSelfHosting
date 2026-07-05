# Infrastructure and Apps Port Registry

## Network Architecture

## Infra Services (8000+)

| Port | Service     | Subdomain                                | Note                |
| :--- | :---------- | :--------------------------------------- | :------------------ |
| 8000 | TSDProxy    | `tsdproxy.internal.anonyfriday.io.vn`    |                     |
| 8001 | Portainer   | `portainer.internal.anonyfriday.io.vn`   |                     |
| 8002 | Homepage    | `internal.anonyfriday.io.vn`             | Root domain proxy   |
| 8003 | Filebrowser | `filebrowser.internal.anonyfriday.io.vn` |                     |
| 8004 | IT-Tools    | `it-tools.internal.anonyfriday.io.vn`    |                     |
| 8006 | Grafana     | -                                        | Skip (Empty Config) |
| 8007 | Fail2ban    | -                                        | Skip (Empty Config) |

## App Services (9000+)

| Port | Service      | Note                |
| :--- | :----------- | :------------------ |
| 9000 | MySQL        | Skip (Empty Config) |
| 9001 | PostgreSQL   | Skip (Empty Config) |
| 9002 | Redis        | Skip (Empty Config) |
| 9003 | SQL Server   | Skip (Empty Config) |
| 9100 | App Backend  | Skip (Empty Config) |
| 9200 | App Frontend | Skip (Empty Config) |
| 9300 | App Gateway  | Skip (Empty Config) |

## Exceptions (Host Network Mode)

| Port  | Service          | Description                           |
| :---- | :--------------- | :------------------------------------ |
| 80    | Caddy HTTP       | External HTTP traffic proxy           |
| 443   | Caddy HTTPS      | External HTTPS traffic proxy with TLS |
| 56789 | Open-Port-Finder | Scans open host ports                 |
