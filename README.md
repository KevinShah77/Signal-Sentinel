<div align="center">

# 📡 Signal Sentinel

### Mobile Network & Call Log Analysis Tool

A local-first web app for analyzing **your own, authorized** call-log and
mobile-network-signal exports — no login, no cloud, no interception.

[![Python](https://img.shields.io/badge/python-3.9%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/flask-3.0-000000?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![License: MIT](https://img.shields.io/badge/license-MIT-informational)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](CONTRIBUTING.md)
[![Made for authorized use only](https://img.shields.io/badge/use-authorized%20data%20only-critical)](#-authorized-use-only)

[Getting Started](#-getting-started) ·
[Features](#-features) ·
[Screenshots](#-screenshots) ·
[Docs](GUIDE.md) ·
[Contributing](CONTRIBUTING.md)

</div>

---

## 📖 Table of Contents

- [What is this?](#-what-is-this)
- [Features](#-features)
- [Screenshots](#-screenshots)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Supported Data & Column Detection](#-supported-data--column-detection)
- [Anomaly Detection Rules](#-anomaly-detection-rules)
- [Design Decisions](#-design-decisions)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Security](#-security)
- [Authorized Use Only](#-authorized-use-only)
- [License](#-license)

---

## 🧭 What is this?

**SignalSentinel** turns a folder of call-log / mobile-network-signal
exports (CSV, JSON, or SQLite) into a browsable dashboard: signal-strength
trends, network-type breakdowns, top cells, rule-based anomaly alerts, and
exportable reports — all running locally in Flask, with zero setup beyond
`pip install`.

It's built for analysts, students, and hobbyists who already have
authorized access to a dataset (their own phone export, a lab dataset, a
test SIM log, etc.) and want a fast, visual way to explore it — not a tool
for collecting data from a device or network.

> **This app does not intercept calls, communications, SIM traffic, or
> another person's device.** It only reads files you import or point it at.

---

## ✨ Features

| Area | What it does |
|---|---|
| 🗂️ **Generic Import** | CSV, JSON, JSON Lines, or SQLite (`.db`/`.sqlite`/`.sqlite3`) — with a table picker when a database has more than one table |
| 🧩 **Flexible Column Detection** | Recognizes many naming conventions (`msisdn`, `caller`, `enodeb_id`, `rat`, …) automatically — your original columns are never renamed or dropped |
| 📋 **Records** | Full sortable table of every imported row |
| 📊 **Analyze** | Avg / strongest / weakest RSRP, a signal-over-time chart, a signal-quality breakdown, network-type distribution, and top cells by signal |
| 🚨 **Alerts & Anomalies** | Three explainable rules: weak signal, sudden signal swings, and network-technology changes |
| 📤 **Reports** | Export CSV, JSON, or a self-contained HTML report; past exports stay listed for re-download |
| 📡 **Live Monitor** | Points at a file on the server and re-scans it on an interval, refreshing the dashboard automatically |
| 💾 **SQLite Persistence** | Save the current dataset into local SQLite storage on demand |
| 🔓 **No Login** | Single-user, local-first — the dashboard loads directly |
| 📘 **Built-in Guide** | An in-app `/guide` page documents every feature for end users |

---

## 🖼️ Screenshots

> Add screenshots or a short GIF walkthrough here once you have them —
> it's the single highest-impact thing you can do for a repo like this.
> A good set to capture:

| Dashboard | Analyze | Live Monitor |
|---|---|---|
| `docs/screenshots/dashboard.png` | `docs/screenshots/analyze.png` | `docs/screenshots/live.png` |

<sub>Tip: drag images into a GitHub issue/PR comment first to get a hosted
URL, then paste that URL here — no need to commit large binaries if you'd
rather not.</sub>

---

## 🛠️ Tech Stack

- **[Flask](https://flask.palletsprojects.com/)** — routing and templating (Jinja2)
- **[pandas](https://pandas.pydata.org/)** — column detection, analysis, and report generation
- **SQLite** (via Python's built-in `sqlite3`) — optional persistent storage
- **Vanilla CSS** — no build step, no framework, one `static/style.css`
- **Inline SVG** — the signal-over-time chart is hand-rendered, no charting library or `matplotlib` dependency

---

## 🚀 Getting Started

### Prerequisites

- Python **3.9+**
- `pip`

### Install & Run

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/signalsentinel.git
cd signalsentinel

# 2. (Recommended) create a virtual environment
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run it
python app.py
```

Then open **http://localhost:5000** — the dashboard loads directly, no
sign-in required. Import `sample_call_data.csv` (included) from the
**+ Import Data** button to see it populated right away.

👉 New to the project? See **[GUIDE.md](GUIDE.md)** for a full step-by-step
walkthrough, troubleshooting tips, and how to explore each page.

---

## 📁 Project Structure

```
signalsentinel/
├── app.py                 # Flask routes
├── core.py                # Data logic: column detection, analysis, anomalies, reports
├── live_monitor.py        # Background polling thread for the Live Monitor page
├── requirements.txt
├── sample_call_data.csv   # Sample dataset to try the app immediately
├── templates/              # Jinja2 templates
│   ├── base.html
│   ├── dashboard.html
│   ├── import.html
│   ├── import_table_select.html
│   ├── records.html
│   ├── analyze.html
│   ├── alerts.html
│   ├── reports.html
│   ├── live.html
│   └── guide.html
├── static/
│   └── style.css          # Design system (single stylesheet, no build step)
├── data/                  # Working dataset + local SQLite store (git-ignored contents)
├── reports/                # Generated CSV/JSON/HTML reports land here (git-ignored contents)
└── logs/                  # Plain-text internal activity log (git-ignored contents)
```

---

## ⚙️ Configuration

The app runs with zero configuration out of the box. One optional
environment variable is supported:

| Variable | Default | Purpose |
|---|---|---|
| `TRACKER_SECRET_KEY` | `dev-only-secret-change-me` | Flask session secret key. Set a real value if you deploy this beyond `localhost` |

Copy `.env.example` to `.env` and adjust as needed, then export it before
running, e.g.:

```bash
export TRACKER_SECRET_KEY="$(python -c 'import secrets; print(secrets.token_hex(32))')"
python app.py
```

---

## 🔍 Supported Data & Column Detection

Upload any CSV, JSON, JSON Lines, or SQLite file — the app doesn't require
one fixed schema. Common column-naming variants are recognized
automatically, for example:

| Canonical field | Recognized aliases |
|---|---|
| `timestamp` | `time`, `date`, `datetime`, `created_at`, `recorded_at`, … |
| `phone_number` | `phone`, `msisdn`, `caller`, `callee`, `contact`, … |
| `rsrp` | `signal`, `signal_strength`, `signal_dbm`, … |
| `cell_id` | `cell`, `eci`, `enodeb_id`, `gnodeb_id`, … |
| `radio_type` | `network`, `technology`, `rat`, `access_technology`, … |

Your original columns are always preserved alongside the detected mapping —
nothing is renamed or removed from the underlying data.

---

## 🚨 Anomaly Detection Rules

| Rule | Trigger | Severity |
|---|---|---|
| Weak signal | RSRP ≤ ‑110 dBm | `MEDIUM` (or `HIGH` at ≤ ‑120 dBm) |
| Sudden signal change | ΔRSRP ≥ 20 dB between adjacent samples | `MEDIUM` |
| Network change | `radio_type` differs from the previous row | `LOW` |

Rules are intentionally simple and explainable rather than ML-based, so
every alert can be traced back to the exact values that triggered it.

---

## 🧠 Design Decisions

- **No authentication** — this is a local, single-analyst tool by design; there's no account system to configure or misconfigure.
- **One shared working dataset** — importing a file replaces what every page reads from, mirroring a simple desktop-app mental model rather than a multi-tenant service.
- **Live Monitor watches a server-side path**, not a browser file picker — the page polls a `/live/status` JSON endpoint every few seconds to reflect state.
- **No `matplotlib`/`tkinter` dependency** — the signal-over-time chart is rendered as inline SVG, keeping the install lightweight.

---

## 🗺️ Roadmap

Ideas welcome via [issues](../../issues) — a few directions that would be
natural next steps:

- [ ] CSV/JSON export of the alert list on its own (not just the full report)
- [ ] Configurable anomaly thresholds via the UI
- [ ] Multi-file / multi-session comparison view
- [ ] Optional GitHub Actions CI (lint + smoke test on push)
- [ ] Dockerfile for one-command local deployment

---

## 🤝 Contributing

Contributions, bug reports, and feature ideas are welcome! Please read
**[CONTRIBUTING.md](CONTRIBUTING.md)** before opening a pull request, and
note that this project follows a **[Code of Conduct](CODE_OF_CONDUCT.md)**.

---

## 🔒 Security

Found a security issue (e.g. path traversal, injection, or auth bypass)?
Please see **[SECURITY.md](SECURITY.md)** for how to report it responsibly
rather than opening a public issue.

---

## ⚠️ Authorized Use Only

This tool is built for analyzing data you **own** or are **explicitly
authorized** to review — your own device export, a lab/test dataset, data
you're allowed to audit at work, etc. It does not intercept calls,
communications, SIM traffic, or another person's device, and it should
never be pointed at data you don't have permission to access. Users are
responsible for complying with all applicable laws and organizational
policies in their jurisdiction.

---

## 📄 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for the
full text.

<div align="center">

Made with 🧉 and a lot of `pandas.DataFrame`s.

</div>
