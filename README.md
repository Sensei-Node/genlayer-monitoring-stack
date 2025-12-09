# GenLayer Monitoring Stack

This repository contains a complete **monitoring MVP** for the GenLayer ecosystem, built for the *Monitoring Working Group* (WG).  
It provides a full-stack observability setup using:

- **Prometheus** – metrics collection
- **Loki** – logs ingestion (via Grafana Alloy)
- **Grafana** – visualization
- **NGINX** – reverse proxy layer
- **Docker Compose** – easy local deployment or simple VPS deployment

This stack is intended as a **foundation for a centralized monitoring backend** for validators participating in the **Asimov-4** testnet.

It is designed to be:
- easy to deploy locally  
- easy to deploy on AWS / VPS  
- easy to migrate to Grafana Cloud or a Foundation-hosted solution  
- safe for a public GitHub repository (no secrets included)  

---

# 📦 Repository Structure

```text
genlayer-monitoring-stack/
├─ docker-compose.yml        # Main file that orchestrates all services
├─ .env.example              # Environment variables template
├─ prometheus/               # Prometheus-specific configurations
│  └─ prometheus.yml
├─ loki/                     # Loki-specific configurations
│  └─ loki-config.yml
├─ grafana/                  # Grafana configuration files
│  └─ provisioning/          # Automatic provisioning
│     └─ datasources/        # Preloaded datasources
│        ├─ prometheus.yml
│        └─ loki.yml
└─ nginx/                    # NGINX reverse proxy configuration
   └─ nginx.conf
```

---

# 🚀 Quick Start

## 1. Clone the repository
```bash
git clone https://github.com/<your-username>/genlayer-monitoring-stack
cd genlayer-monitoring-stack
```

2. Create your .env file
```bash
cp .env.example .env
```
Edit the .env file and configure:

- Grafana admin credentials
- Node metrics endpoint (default: host.docker.internal:9153)

Example:
```bash
.env
GRAFANA_ADMIN_USER=admin
GRAFANA_ADMIN_PASSWORD=changeme
NODE_METRICS_URL=http://host.docker.internal:9153/metrics
```

3. Start the full stack
```bash
docker compose up -d
```

4. Access the dashboards
| Component  | URL                                                        |
| ---------- | ---------------------------------------------------------- |
| Grafana    | [http://localhost/grafana](http://localhost/grafana)       |
| Prometheus | [http://localhost/prometheus](http://localhost/prometheus) |
| Loki API   | [http://localhost/loki](http://localhost/loki)             |

🏗 Architecture
```text
          ┌──────────────────┐
          │    GenLayer Node │
          │  (metrics + logs)│
          └───────┬──────────┘
                  │ 9153 /metrics
                  │
          ┌───────▼──────────┐
          │   Grafana Alloy  │
          │ (log + metric fw)│
          └───────┬──────────┘
                  │ remote_write (metrics)
                  │ push logs
┌─────────────────▼────────────────────────────────────────┐
│                   Monitoring Backend                     │
│                                                          │
│      ┌──────────────┐    ┌────────────────────────┐      │
│      │  Prometheus  │    │          Loki          │      │
│      └───────▲──────┘    └─────────────▲──────────┘      │
│              │                         │                 │
│      ┌───────┴───────┐      ┌──────────┴──────────┐      │
│      │     Grafana   │◄─────┤  Dashboards + Logs  │      │
│      └───────────────┘      └─────────────────────┘      │
└──────────────────────────────────────────────────────────┘

                  │
                  ▼
          ┌──────────────────┐
          │      NGINX       │
          │  (reverse proxy) │
          └──────────────────┘
```

The design ensures:
- simple local deployment
- clear path to centralization
- minimal dependencies
- compatibility with the existing GenLayer node telemetry (config.yaml)
- plug-and-play with Alloy on validator nodes

🔧 Components Overview
✔ Prometheus
Collects metrics exposed at /metrics on the validator node.
Compatible with GenLayer collectors:
- node
- genvm
- webdriver
✔ Loki
Receives logs forwarded by Grafana Alloy.
✔ Grafana
Provides dashboards for:
- global network metrics
- per-validator dashboards
- log analysis (Loki)
✔ NGINX
Acts as a secure entry point for all components.

🎯 How Validators Integrate
Validators only need to:

1. Enable telemetry in the config.yaml:
```yaml
ops:
  port: 9153
  endpoints:
    metrics: true
    health: true

metrics:
  interval: "15s"
  collectors:
    node:
      enabled: true
      interval: "30s"
    genvm:
      enabled: true
      interval: "20s"
    webdriver:
      enabled: true
      interval: "60s"
```

```bash
.env
CENTRAL_MONITORING_URL=http://<backend>/prometheus/api/v1/write
CENTRAL_LOKI_URL=http://<backend>/loki/api/v1/push
NODE_ID=<validator-unique-id>
VALIDATOR_NAME=<your-name>
```
That's it.

📊 Dashboards
- Import the Validatrium dashboard JSON
- Add a global dashboard (WIP)
- Add a per-validator template dashboard (WIP)

🛣 Roadmap
Phase 1 – MVP (This repo)
- Prometheus + Loki + Grafana stack
- Node metrics + Alloy logs
- Base dashboard imported
- AWS-compatible
- NGINX proxy

Phase 2 – WG Alignment
- Validate with @ras and @Agustin
- Define labels: validator_name, node_id, network
- Build global dashboard for all validators
- Build per-validator template

Phase 3 – Centralization
- Migrate to Foundation infrastructure or Grafana Cloud
- Switch backends (keep dashboard + Alloy config)
- Implement alerting (Discord / email)

⚠️ Security Notes
This repository must remain public-safe:
- Never commit .env
- Do not expose credentials
- Do not expose private endpoints
- NGINX should handle authentication + HTTPS
- Use firewall rules on AWS (allow trusted IPs if necessary)

🤝 Contributing
This repo is designed to be used by:
- SenseiNode
- Validatrium
- VjnTech
- GenLayer community validators
- GenLayer Foundation
- GenLayer Labs
Contributions are welcome:
- dashboards
- queries
- documentation
- deployment improvements