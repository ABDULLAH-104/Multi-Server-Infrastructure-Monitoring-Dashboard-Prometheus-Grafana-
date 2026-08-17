# Multi-Server-Infrastructure-Monitoring-Dashboard-Prometheus-Grafana-

# 🖥️ Multi-Server Infrastructure Monitoring Dashboard (Prometheus + Grafana)

A real-time infrastructure monitoring system simulating a multi-server environment — Web Server, Database Server, and App Server — built using the industry-standard DevOps monitoring stack: **Prometheus** and **Grafana**.

---

## 📌 Project Overview

Companies rely on constant visibility into their server health — CPU usage, memory consumption, and overall system load — to catch problems before they cause downtime. This project simulates that exact setup: multiple "servers" (via Docker), each continuously monitored, with all metrics flowing into a single live Grafana dashboard.

It answers key operational questions:
- How much CPU and RAM is each server currently using?
- How does resource usage trend over time?
- Which server is under the most load right now?
- Are any servers approaching critical thresholds?

---

## 🛠️ Tech Stack & Why Each Tool Was Used

| Tool | Purpose |
|---|---|
| **Docker** | Used to simulate three independent "servers" (Web, DB, App) on a single machine, each running in its own isolated container. |
| **Node Exporter** | A lightweight agent that runs on each simulated server and continuously exposes system-level metrics — CPU, memory, disk, and more. |
| **Prometheus** | Scrapes (pulls) metrics from all three Node Exporter instances every 15 seconds and stores them as time-series data, building a historical record. |
| **Grafana** | Connects to Prometheus to visualize the collected metrics through live, auto-refreshing dashboards — trend graphs, stat panels, and color-coded thresholds. |
| **PromQL** | Prometheus's query language, used to calculate usage percentages and aggregate metrics per server label. |

---

## ⚙️ How It Works (Pipeline)

```
[Docker]           →  simulates 3 servers: Web Server, DB Server, App Server
        ↓
[Node Exporter]     →  runs on each server, continuously exposing CPU/RAM metrics
        ↓
[Prometheus]        →  scrapes all 3 exporters every 15s, stores metrics with
                        server labels, building a time-series history
        ↓
[Grafana]           →  queries Prometheus via PromQL, renders live dashboards
                        that auto-refresh with the latest data
```

This mirrors how real companies monitor production infrastructure — continuous metric collection feeding dashboards that stay current without manual intervention.

---

## 📊 Dashboard Panels

- **CPU Usage Trend** — time-series line chart comparing CPU usage across all three servers over time
- **RAM Usage Trend** — time-series line chart comparing memory usage across all three servers
- **Current CPU Usage** — color-coded stat panel per server (green/yellow/red thresholds)
- **Current RAM Usage** — color-coded stat panel per server (green/yellow/red thresholds)

---

## 🚀 Key Skills Demonstrated

- Docker-based infrastructure simulation
- Metrics exposition using Node Exporter
- Prometheus configuration and scrape job setup
- Writing PromQL queries for real-world metrics (CPU/RAM percentage calculations)
- Grafana dashboard design (time-series panels, stat panels, thresholds, labels)
- Understanding of the Prometheus + Grafana stack used industry-wide for DevOps and SRE monitoring

---

## 📷 Preview

*(Add dashboard screenshots here)*

---

## 🔧 Setup / How to Run

1. Create a Docker network for all services to communicate:
   ```
   docker network create monitoring
   ```
2. Start three Node Exporter containers (one per simulated server):
   ```
   docker run -d --name web-server-exporter --network monitoring -p 9101:9100 prom/node-exporter
   docker run -d --name db-server-exporter --network monitoring -p 9102:9100 prom/node-exporter
   docker run -d --name app-server-exporter --network monitoring -p 9103:9100 prom/node-exporter
   ```
3. Start Prometheus, mounting the included `prometheus.yml` config:
   ```
   docker run -d --name prometheus --network monitoring -p 9090:9090 -v <path-to>/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
   ```
4. Start Grafana and connect it to the same network:
   ```
   docker run -d --name grafana --network monitoring -p 3000:3000 grafana/grafana
   ```
5. In Grafana, add Prometheus as a data source using URL `http://prometheus:9090`, then build or import the dashboard panels.
