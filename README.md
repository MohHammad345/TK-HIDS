
# TK-HIDS

### Modular Python Host Intrusion Detection System

TK-HIDS is a lightweight, modular **Host Intrusion Detection System (HIDS)** written in Python.

It monitors endpoint activity in real time and simulates core behaviors of modern EDR tools:

*  File Integrity Monitoring
*  Process Execution Tracking
*  Network Connection Monitoring
*  MITRE ATT&CK Mapping
*  Risk-Based Alert Scoring
*  Behavioral Anomaly Detection
*  SOC-Style Reporting

Built from scratch with clean architecture and test-driven design.

---

#  Core Capabilities

##  File Integrity Monitoring (FIM)

* SHA-based hashing
* Baseline creation & comparison
* Detects:

  * `file_added`
  * `file_modified`
  * `file_removed`
* Automatic recovery if baseline is missing or corrupted

Baseline stored in:

```text
data/baseline.json
```

---

##  Process Monitoring

* Detects newly spawned processes
* Configurable suspicious process list
* Allow-list filtering
* Deduplication window to prevent alert spam

---

##  Network Monitoring

* Detects new outbound TCP connections
* Suspicious port detection
* Remote port allow-list
* Tracks connection states

---

##  MITRE ATT&CK Enrichment

Each alert can be mapped to MITRE techniques via configuration.

Example:

```json
{
  "type": "integrity",
  "reason": "file_modified",
  "mitre_attack": ["T1565.001"]
}
```

Mapping is defined in:

```text
configs/config.yaml
```

---

##  Risk Scoring Engine

Each alert receives a computed `risk_score` based on:

* Alert type
* Reason
* MITRE technique weight
* Configurable severity thresholds

Severity can automatically escalate:

```
low → medium → high
```

All scoring behavior is defined in `config.yaml`.

---

##  Behavioral Anomaly Engine

Detects burst behavior inside a time window:

* Process spawn bursts
* Network connection bursts
* Cooldown mechanism to prevent alert storms

Fully configurable:

```yaml
anomaly:
  enabled: true
  window_sec: 60
  process_burst_threshold: 5
  network_burst_threshold: 10
```

---

##  SOC Summary Report

Generate a summarized security report:

```bash
python hids_main.py --config configs/config.yaml --report
```

Example output:

```
TK-HIDS Summary Report
===============================
Total alerts: 12

Counts by type:
 - integrity: 4
 - process: 5
 - network: 2
 - anomaly: 1

MITRE ATT&CK techniques observed:
 - T1565.001: 2
 - T1070: 1
```

---

#  Architecture Overview

```
Detection Modules
    ↓
MITRE Mapping
    ↓
Risk Scoring
    ↓
Alert Logger
    ↓
Anomaly Engine
    ↓
Report Generator
```

Each module is isolated and independently testable.

---

#  Project Structure

```
py-hids/
│
├── configs/
│   └── config.yaml
│
├── data/                 # Runtime state (ignored by git)
│   ├── baseline.json
│   └── state.json
│
├── hids/
│   ├── anomaly.py
│   ├── config.py
│   ├── integrity.py
│   ├── logger.py
│   ├── mitre.py
│   ├── netWatch.py
│   ├── processWatch.py
│   ├── report.py
│   ├── runner.py
│   └── scoring.py
│
├── logs/                 # Runtime logs (ignored by git)
│   └── alerts.jsonl
│
├── tests/
│   ├── test_anomaly.py
│   ├── test_integrity.py
│   ├── test_mitre.py
│   ├── test_report.py
│   └── test_scoring.py
│
├── hids_main.py
├── pytest.ini
├── requirements.txt
├── README.md
└── .gitignore
```

---

#  Running TK-HIDS

Activate virtual environment:

```bash
source .venv/bin/activate
```

Start monitoring:

```bash
python hids_main.py --config configs/config.yaml
```

Stop with:

```
Ctrl + C
```

Alerts are written to:

```
logs/alerts.jsonl
```

---

#  Running Tests

```bash
pytest -q
```

All core components are unit-tested:

* MITRE mapping
* Risk scoring
* Anomaly detection
* Integrity diffing
* Reporting

---

#  Configuration-Driven Design

All behavior is controlled via:

```
configs/config.yaml
```

No code modification required to:

* Adjust thresholds
* Modify scoring weights
* Add MITRE mappings
* Tune anomaly detection
* Enable/disable modules

---

#  Design Principles

* Modular architecture
* Clear separation of concerns
* Config-first behavior
* Deterministic unit testing
* Minimal external dependencies
* SOC-style structured logging

---

#  Educational Value

This project demonstrates understanding of:

* Endpoint detection logic
* File integrity monitoring concepts
* MITRE ATT&CK framework
* Risk-based alert prioritization
* Behavioral anomaly detection
* Python modular system design
* Test-driven development

---

#  Future Improvements

* Web dashboard (Flask/FastAPI)
* Multi-agent central server mode
* Syslog forwarding
* Threat intelligence enrichment
* Linux auditd integration
* Windows event log support

---

#  Summary

TK-HIDS is a modular, configuration-driven endpoint detection prototype built entirely in Python.
It models core behaviors found in modern EDR tools while remaining lightweight and extensible.
