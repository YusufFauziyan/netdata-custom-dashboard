# Netdata Custom Dashboard

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Netdata Compatible](https://img.shields.io/badge/Netdata-v1.30%2B-blue.svg)](https://learn.netdata.cloud/)

A lightweight, high-performance, and self-contained custom monitoring dashboard designed specifically for the Netdata REST API (`/api/v1/data`).

This dashboard offers real-time visualization of key system metrics (CPU, Memory, RAPL Power Consumption, Network, Disk I/O, and Docker Container performance) without relying on bloated external dependencies or heavy frontend frameworks.

---

## Key Features

- **Zero External Dependencies**: Pure HTML5, CSS3, and modern Vanilla JavaScript (ES6+). No external Web Fonts, heavy JS frameworks, or CDN reliance required.
- **Adaptive Dark / Light Theme**: Built-in system theme detection (`color-scheme: light dark`) with a color-vision deficiency (CVD) accessible palette.
- **Real-Time Polling & Graphs**: Interactive SVG/Canvas charts tracking live CPU breakdown (User vs System) and Intel RAPL Package Power Consumption (Watts).
- **Docker Cgroup Metrics**: Live resource monitoring (CPU % and Memory usage) for individual Docker containers (e.g., PostgreSQL, Web, API, and Agent services).
- **Responsive Layout**: Designed for seamless viewing across mobile devices, tablets, and desktop workstations.

---

## Dashboard Preview & Metrics Tracked

1. **CPU Utilization**: Breakdown of User vs System load (%) with multi-threaded kernel metrics.
2. **Memory Allocation**: Real-time tracked RAM usage, Cached memory, and Buffer allocation out of system total.
3. **Power Consumption**: Real-time energy tracking via Intel RAPL (`cpu.powercap_intel_rapl_zone_package-0`).
4. **Container Telemetry**: Real-time cgroup statistics per active container instance.

---

## Deployment & Integration

### Option 1: Overwrite Default Netdata UI (Recommended)

You can serve this dashboard directly from your running Netdata container by replacing the default `index.html`:

```bash
# 1. Clone the repository
git clone https://github.com/YusufFauziyan/netdata-custom-dashboard.git

# 2. Copy index.html into your running Netdata container
docker cp netdata-custom-dashboard/index.html netdata:/usr/share/netdata/web/index.html
```

### Option 2: Mount via Docker Compose

To ensure persistence across container updates and rebuilds, mount `index.html` as a read-only volume in your `docker-compose.yml`:

```yaml
services:
  netdata:
    image: netdata/netdata
    container_name: netdata
    ports:
      - "19999:19999"
    volumes:
      - ./index.html:/usr/share/netdata/web/index.html:ro
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
    restart: unless-stopped
```

### Option 3: Standalone Static Web Hosting

Because the dashboard is fully client-side, you can host `index.html` on Nginx, Caddy, or static hosting providers (such as Cloudflare Pages or Vercel). Ensure CORS or a reverse proxy is configured to route `/api/v1/` requests to your Netdata instance.

---

## Customization

To add or modify monitored Docker containers, open `index.html` and edit the container definitions:

```javascript
const containers = [
  { name: 'Web Service', chartCpu: 'cgroup_web.cpu', chartMem: 'cgroup_web.mem_usage', role: 'Frontend UI' },
  { name: 'API Service', chartCpu: 'cgroup_api.cpu', chartMem: 'cgroup_api.mem_usage', role: 'Backend API' },
  { name: 'Database',    chartCpu: 'cgroup_db.cpu',  chartMem: 'cgroup_db.mem_usage',  role: 'PostgreSQL Engine' }
];
```

---

## License

Distributed under the [MIT License](LICENSE).
