# ⚡ Signal Sentinel — Cyber Operations & Telecom Intelligence Suite

<div align="center">

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Python](https://img.shields.io/badge/Python-3.9%20%7C%203.10%20%7C%203.11%20%7C%203.12-cyan.svg)
![Flask](https://img.shields.io/badge/Flask-3.0.3-emerald.svg)
![Pandas](https://img.shields.io/badge/Pandas-2.2.2-amber.svg)
![Three.js](https://img.shields.io/badge/Three.js-3D%20WebGL-purple.svg)
![Leaflet](https://img.shields.io/badge/Leaflet-GPS%20Intel-green.svg)

**A sovereign, full-stack network intelligence and cellular signal forensics web platform.**  
*Analyzes authorized call-log records, RF spectrum power metrics, cellular handovers, and detects mobile network anomalies.*

[Quick Start](#-quick-start) • [Key Features](#-key-features) • [Architecture](#-architecture) • [REST API](#-rest-api) • [Security & Privacy](#-security--privacy)

</div>

---

## 🌟 Key Features

- 🛡️ **Cybersecurity & Network Operations HUD**: Dark-themed Cyber NOC design system with glassmorphism, HUD indicators (`SHIELD ACTIVE · NOC ONLINE`), and high-contrast telemetry meters.
- 🌐 **Interactive 3D WebGL Mesh & Globe**: Powered by Three.js — features an interactive rotating wireframe cyber globe with orbiting particle constellations in the hero console.
- 🎛️ **3D Interactive Perspective Cards**: CSS 3D perspective cards with dynamic tilt and neon hover glare.
- 📈 **RF Spectrum Telemetry Visualizer**: Zoomable Chart.js timeline tracking signal power (RSRP in dBm), jitter, median, standard deviation, and power density bands.
- 🗺️ **CartoDB Dark Matter GPS Map**: Interactive Leaflet.js cyber map plotting drive-test coordinates, cell tower sectors, signal strength pins, and transmission vectors.
- 🚨 **Automated RF Threat Ledger**: Rule-based detection of:
  - `CRITICAL_BLINDSPOT`: Severe RF attenuation (<= -120 dBm)
  - `RAPID_RF_ATTENUATION`: Abrupt signal attenuation drops (>= 20 dB)
  - `INTER_RAT_DOWNGRADE`: Forced 5G/LTE to 2G/3G transitions (Rogue Base Station / IMSI Catcher indicators)
  - `CALL_DROP_RF_FAULT`: Call drop events correlated with low RF coverage
- ⚡ **1-Click Pre-Packaged Scenarios**:
  - `5G NR & GPS Cyber Drive-Test` (CSV with coordinates)
  - `Multi-RAT Transmission Baseline` (CSV)
  - `SQLite Vault Ingestion` (`call_logs` & `network_events` tables)
- 🧪 **Synthetic RF Drive-Test Simulator**: Built-in mathematical generator simulating customizable 5G/LTE drive-tests with multipath fading, shadow loss, and GPS paths.
- 💾 **SQLite Vault Persistence & Snapshots**: Save, archive, and restore named database snapshots directly into embedded SQLite storage.
- 📑 **Executive Security Report Generator**: Generates standalone HTML security audit reports with embedded SVG charts, CSV raw dumps, and JSON logs.
- 📡 **Real-Time Live Telemetry Streamer**: Background thread monitoring local capture files with live rolling charts.

---

## 🚀 Quick Start

### 1. Prerequisites & Installation

```bash
# Clone or download the repository
git clone https://github.com/your-username/signalsentinel.git
cd signalsentinel

# Install dependencies
pip install -r requirements.txt
```

### 2. Launch the Web Application

```bash
python app.py
```

Open your browser and navigate to:
👉 **`http://localhost:5000`** (or `http://127.0.0.1:5000`)

---

## 📂 Project Structure

```text
signalsentinel/
├── app.py                     # Flask web server, routes, and REST APIs
├── core.py                    # Telecom data engine, RF modeling & report generator
├── live_monitor.py            # Real-time background polling thread
├── requirements.txt           # Python dependencies
├── sample_call_data.csv       # Standard sample dataset
├── sample_data/               # Pre-packaged 5G & SQLite test datasets
│   ├── sample_geo_5g.csv
│   ├── sample_geo_5g.json
│   └── sample_geo_5g.sqlite
├── static/
│   └── style.css              # Cyber-Defense 3D Dark Theme Design System
├── templates/                 # Jinja2 HTML Templates
│   ├── base.html              # Core layout + Three.js 3D globe script
│   ├── dashboard.html         # NOC Command Center + 3D cards + Chart.js
│   ├── analyze.html           # Spectrum Intelligence + Leaflet Dark Map
│   ├── records.html           # Telemetry Frame Vault & Filter Grid
│   ├── alerts.html            # Threat Ledger & Anomaly Diagnostics
│   ├── live.html              # Real-Time Telemetry Stream Console
│   ├── reports.html           # Executive Report Studio & SQLite Snapshots
│   ├── import.html            # High-tech Ingestion Dropzone
│   └── import_table_select.html
├── data/                      # Active dataset & SQLite database storage
├── reports/                   # Generated HTML/CSV/JSON security reports
├── logs/                      # Audit trails and activity logs
├── LICENSE                    # MIT License
└── README.md                  # Documentation
```

---

## 🔌 REST API Reference

| Endpoint | Method | Description |
| :--- | :---: | :--- |
| `/api/data` | `GET` | Returns active dataset columns and records in JSON |
| `/api/stats` | `GET` | Returns signal, network, cell, anomaly, and GPS statistics |
| `/api/load-sample/<name>` | `POST` | Loads pre-packaged scenario (`geo_5g`, `basic`, `sqlite_call_logs`) |
| `/api/generate-synthetic` | `POST` | Generates customizable synthetic drive-test data |
| `/api/clear-data` | `POST` | Purges current active workspace |
| `/live/status` | `GET` | Polling endpoint for real-time telemetry stream |
| `/reports/<filename>/delete` | `POST` | Deletes an archived security report |
| `/database/snapshot/restore/<id>` | `POST` | Restores a saved SQLite snapshot |

---

## 🔒 Security & Privacy Notice

> [!IMPORTANT]
> **SignalSentinel is designed strictly for local, sovereign analysis of authorized datasets.**
> - It **does not** intercept live SIM traffic, cellular calls, or access remote third-party devices.
> - All analytics run **100% locally on your machine** without sending data to external cloud servers.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
