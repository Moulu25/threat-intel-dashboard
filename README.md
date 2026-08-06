# Threat Intelligence Dashboard

An automated pipeline that collects, validates, and normalizes cybersecurity
threat data from multiple public sources, stores it in a relational SQL
database, and visualizes it through an interactive Streamlit dashboard —
malicious IPs, phishing campaigns, and malware trends.

## Features

- **Automated ingestion pipeline** — pulls indicators of compromise (IOCs)
  from public threat intel feeds (URLhaus, Feodo Tracker), validates and
  normalizes them into a common schema, and deduplicates on re-run.
- **Relational SQL database** — SQLite schema (`indicators`, `ingestion_log`)
  designed for efficient querying and reporting of IOCs by type, source,
  and time.
- **Interactive dashboard** — Streamlit + Plotly visualizations: threat
  category breakdown, malware family trends, indicator volume over time,
  geographic distribution, and a filterable/searchable data table.

## Tech Stack

Python · Streamlit · SQLite (SQL) · Plotly · Pandas · Requests

## Architecture

```
Public Threat Feeds (URLhaus, Feodo Tracker, phishing sources)
            │
            ▼
   fetch_feeds.py  ── validate & normalize records
            │
            ▼
   database.py  ── SQLite (indicators, ingestion_log tables)
            │
            ▼
   app.py  ── Streamlit dashboard (charts, filters, tables)
```

## Setup

```bash
git clone https://github.com/<your-username>/threat-intel-dashboard.git
cd threat-intel-dashboard
pip install -r requirements.txt
```

## Run

```bash
cd src
streamlit run app.py
```

Open the local URL Streamlit prints (usually `http://localhost:8501`), then
click **"Run ingestion pipeline now"** in the sidebar to populate the
database.

## Notes on data sources

The pipeline is written to pull live data from public feeds (URLhaus,
Feodo Tracker) that require no API key. If a feed is unreachable — rate
limited, offline, or blocked by network restrictions — the pipeline falls
back to a locally generated sample set with a realistic schema, so the
dashboard always has data to demonstrate against. Every ingestion run is
logged (source, record counts, live/sample mode) and viewable in the
dashboard's "Pipeline ingestion log" panel.

## Possible extensions

- Add authenticated feeds (VirusTotal, AlienVault OTX, PhishStats) via API keys
- Move from SQLite to PostgreSQL for concurrent multi-user access
- Add alerting (email/Slack) when high-confidence indicators are ingested
- Add IP geolocation lookup for accurate country-level mapping
