# 📝 TO-DO — GenLayer Monitoring Stack

This file tracks tasks, progress and next steps for the GenLayer Monitoring MVP.

---

## ✅ Phase 1 — MVP Backend (Local / AWS)

### Completed
- [x] Create repository structure  
- [x] Add docker-compose with Prometheus, Loki, Grafana and NGINX  
- [x] Add Prometheus config template  
- [x] Add Loki config template  
- [x] Add Grafana datasource provisioning  
- [x] Add NGINX reverse proxy config  
- [x] Create `.env.example`  
- [x] Prepare README  
- [x] Provide directory explanations  

### In Progress
- [ ] Spin up local sandbox  
- [ ] Validate Prometheus scraping from GenLayer node  
- [ ] Validate Loki ingestion from Alloy  
- [ ] Import Validatrium dashboard  
- [ ] Build initial global dashboard draft  

### Pending
- [ ] Define standard labels (`validator_name`, `node_id`, `network`)  
- [ ] Decide on backend location (AWS / Grafana Cloud / Foundation infra)  
- [ ] Configure endpoint formats for Alloy remote write  
- [ ] Add security hardening for NGINX  
- [ ] Add log parsing rules (if needed)  

---

## 🧩 Phase 2 — Working Group Alignment

- [ ] Present MVP architecture to @ras and @Agustin  
- [ ] Collect feedback from WG members  
- [ ] Agree on backend strategy (short-term)  
- [ ] Publish per-validator dashboard template  
- [ ] Publish global (network-level) dashboard  

---

## 🚀 Phase 3 — Centralization / Future Work

- [ ] Migrate stack to chosen long-term backend  
- [ ] Add alerting (Discord / email / Slack)  
- [ ] Add retention policies per WG specs  
- [ ] Add per-validator access tokens (Grafana)  
- [ ] Add documentation for validators to onboard  
- [ ] Add CI/CD for config validation  
