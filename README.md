# Monitoring Lab – Prometheus, Node Exporter & Grafana

## Overview
This project monitors system resources using Prometheus, Node Exporter, Grafana and Docker Compose. It helps visualize CPU, memory, disk, network and system uptime.

## Architecture

```text
           My Computer
                |
         Node Exporter
        (Metrics Collector)
                |
           Prometheus
        (Metrics Storage)
                |
            Grafana
        (Dashboard)
```

## Technologies Used
- Docker Compose
- Prometheus
- Node Exporter
- Grafana

## Five Metrics Monitored

1. **CPU Usage:** Percentage of CPU being used.
2. **Memory Usage:** RAM used in percentage and GB.
3. **Disk Usage:** Percentage of disk space used.
4. **Network Traffic:** Incoming and outgoing network data.
5. **System Uptime:** Time since the system started.

## PromQL Queries

### CPU Usage (%)
```promql
100 - (avg by (cpu) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

### Memory Usage (%)
```promql
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))
```

### Memory Used (GB)
```promql
(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / 1024 / 1024 / 1024
```

### Disk Usage (%)
```promql
100 * (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"} / node_filesystem_size_bytes{fstype!~"tmpfs|overlay"}))
```

### Network In
```promql
rate(node_network_receive_bytes_total[5m])
```

### Network Out
```promql
rate(node_network_transmit_bytes_total[5m])
```

### System Uptime
```promql
time() - node_boot_time_seconds
```

### Target Status
```promql
up
```

## How to Start

Run this command from the project folder:

```bash
docker compose up -d
```

## Access Services
- Prometheus: http://localhost:9090
- Node Exporter: http://localhost:9100
- Grafana: http://localhost:3000

## Stop Services

```bash
docker compose down
```
