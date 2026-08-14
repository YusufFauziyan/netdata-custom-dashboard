# Netdata Custom Dashboard

Lightweight, self-contained custom monitoring dashboard for Netdata API.

## Features
- **Zero External Dependencies**: Pure HTML, CSS, and vanilla JS. No CDN or external web fonts needed.
- **Dark/Light Mode Support**: Auto-adapts to system theme with standard CVD-accessible color palette.
- **Real-time Metrics**: Single-page UI polling Netdata's `/api/v1/data` endpoint for CPU, RAM, Power (RAPL), Disk, Network, and Docker Containers.

## How to Deploy / Set Up Custom Dashboard on Netdata

### Option 1: Copy directly into Netdata Docker Container
```bash
# 1. Clone this repository
git clone https://github.com/YusufFauziyan/netdata-custom-dashboard.git

# 2. Replace index.html inside running Netdata container
docker cp netdata-custom-dashboard/index.html netdata:/usr/share/netdata/web/index.html
```

### Option 2: Mount via Docker Compose / Docker Volume
```yaml
services:
  netdata:
    image: netdata/netdata
    container_name: netdata
    volumes:
      - ./index.html:/usr/share/netdata/web/index.html:ro
```

### Option 3: Host as Static Web Page
You can also host `index.html` on Nginx/Caddy/Cloudflare Pages and point the Netdata API endpoint (`/api/v1/...`) to your Netdata host URL.

---
Created for **y-server** (`monitor.fauzyan.my.id`).
