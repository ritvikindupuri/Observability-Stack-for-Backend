# Boilerexams - Full Observability Stack

## Overview

This project details the design and implementation of a full observability stack for Boilerexams. The system provides real-time monitoring across core backend services using **Prometheus, Grafana, Node Exporter, Telegraf, and Docker**.

The primary goal was to provide the team with clear, actionable visibility into service health, performance trends, and resource usage. This was achieved by configuring Prometheus scrapers, designing time-series dashboards, and validating exporter reliability. The entire stack is deployed via Coolify for consistent service provisioning.

This project successfully strengthened operational reliability for the platform and established a repeatable monitoring foundation that supports debugging, capacity planning, and long-term backend stability.

##  Core Technologies

* **Prometheus:** Time-series database & monitoring (scraping, storage, alerting)
* **Grafana:** Visualization & dashboarding
* **Node Exporter:** Exposes hardware and OS metrics from \*nix hosts
* **Telegraf:** Agent for collecting, processing, and aggregating host-level telemetry
* **Docker:** Containerization for all services
* **Coolify:** Deployment and service provisioning

---

##  System Architecture

The architecture is built on a standard, highly effective "scrape" model.

* **Agents (Node Exporter & Telegraf):** Deployed to target servers, they collect metrics and expose them on HTTP endpoints (`:9100` for Node Exporter, `:9273` for Telegraf).
* **Prometheus (Scraper):** The central server (`:9090`) is configured to scrape these endpoints at a regular interval (e.g., every 15 seconds), pulling in the metrics and storing them.
* **Grafana (Visualizer):** The dashboarding layer (`:3000`) connects to Prometheus as a data source. It uses PromQL (Prometheus Query Language) to query the time-series data and render the graphs and panels seen below.

![System Architecture](.assets/System%20Architecture.png)

---

##  Dashboards & Key Metrics

Several dashboards were created to monitor the full stack, from the health of the monitoring system itself to granular host-level performance.

### 1. Prometheus Service Health (Meta-Monitoring)

Before monitoring *other* services, it's crucial to monitor the monitor. This dashboard confirms the Prometheus service itself is healthy and performing well.

* **Scrape Duration:** Shows that scrapes are fast and efficient (e.g., ~11.9ms), well below the timeout threshold.
* **WAL Corruptions:** **"None"** is the perfect state, confirming the Write-Ahead Log is healthy.
* **Memory Profile:** Tracks resident and virtual memory, showing stable usage without leaks.
* **Head Chunks & Blocks:** Confirms data is being ingested, compacted, and stored correctly.

![Prometheus Stats Dashboard](.assets/Prometheus%20Stats%20Dashboard.png)

### 2. Host-Level Monitoring: Firewall (Node Exporter)

Node Exporter was used to gain deep visibility into the performance of a critical firewall host.

#### System Load

This panel tracks the 1-minute, 5-minute, and 15-minute system load averages. As shown, the load consistently remains **below 1.0** (peaking around 0.7), which indicates a healthy system that is not under significant CPU pressure or I/O wait.

![Node Exporter Firewall Load Graph](.assets/Node%20Exporter%20Firewall%20Load%20Graph.png)

#### Network Throughput

This dashboard monitors the real-time incoming (RX) and outgoing (TX) network traffic. The system clearly **captured a significant network event around 15:42**, logging an outgoing spike of **~380KB/s** and an incoming spike of **~350KB/s**. This demonstrates the dashboard's effectiveness in identifying and correlating performance anomalies.

![Network Throughput TXRX](.assets/Network%20Throughput%20TXRX%20(Node%20exporter).png)

### 3. Host-Level Telemetry (Telegraf)

Telegraf was integrated to provide an alternative and highly granular method for collecting system telemetry. This dashboard monitors a host running the Telegraf agent.

* **Memory Usage (Purple):** The most critical metric on this graph, showing memory usage is exceptionally stable, holding steady at **~2.91GB**. This is a key indicator of a healthy service with no memory leaks.
* **CPU Time (Green):** The `cpu-total` metric shows low and consistent CPU activity, correlating with the stable service.

![Telegraf System Overview](.assets/Telegraph%20System%20Overview.png)

---

## Conclusion & Impact

This project successfully implemented a robust, end-to-end observability stack for Boilerexams, moving the platform from a "black box" to a fully monitored system.

By integrating Prometheus, Grafana, and multiple exporters, this stack provides a single pane of glass for backend health. The dashboards have already proven their value by **validating stable memory usage**, identifying normal system load patterns, and **successfully capturing anomalous network events** that were previously invisible.

The result is a more resilient and reliable platform. The team is now equipped with the critical, real-time data needed for proactive debugging, intelligent capacity planning, and ensuring long-term backend stability.
