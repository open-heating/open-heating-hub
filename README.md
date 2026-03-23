# open-heating/hub

Raspberry Pi monitoring and configuration hub for open-heating.

## Overview

This Docker stack runs on a Raspberry Pi and provides:

- **Mosquitto** — MQTT broker for communication with ESP32 controllers
- **InfluxDB** — Time-series database for temperature and valve position data
- **Telegraf** — MQTT-to-InfluxDB bridge
- **Grafana** — Dashboards for monitoring heating circuits
- **Web UI** — Configuration interface (planned)

The hub is **optional** — ESP32 controllers operate independently. The hub adds monitoring, logging, and remote configuration.

## Quick start

```bash
# Clone
git clone https://github.com/open-heating/hub.git
cd hub

# Start all services
docker compose up -d

# Check status
docker compose ps
```

### Access

| Service   | URL                         | Default credentials |
|-----------|---------------------------- |---------------------|
| Grafana   | http://raspberrypi:3000     | admin / admin       |
| InfluxDB  | http://raspberrypi:8086     | (setup on first run)|
| Mosquitto | mqtt://raspberrypi:1883     | (no auth by default)|

## Architecture

```
ESP32 controllers
    │
    │ MQTT (WiFi)
    ▼
┌─────────────────────────────┐
│  Raspberry Pi               │
│                             │
│  Mosquitto (:1883)          │
│    │                        │
│    ├── Telegraf → InfluxDB  │
│    │              (:8086)   │
│    │                        │
│    └── Web UI → ESP32 config│
│                             │
│  Grafana (:3000)            │
│    └── reads from InfluxDB  │
└─────────────────────────────┘
```

## Configuration

### Mosquitto

Edit `mosquitto/mosquitto.conf` to add authentication or TLS.

### Grafana

Pre-provisioned dashboards are in `grafana/dashboards/`. The InfluxDB datasource is auto-configured via `grafana/provisioning/`.

## Requirements

- Raspberry Pi 3B+ or 4 (2GB+ RAM recommended)
- Docker and Docker Compose
- Network access to ESP32 controllers

## License

Apache License 2.0
