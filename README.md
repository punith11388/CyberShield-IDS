# CyberShield IDS &ndash; Network Intrusion Detection and Threat Monitoring System

## Abstract

CyberShield IDS is an educational network monitoring prototype for systems and networks that the operator owns or is authorized to monitor. It extracts packet metadata such as addresses, protocol, ports, size, and timestamp, then applies simple rules to identify patterns that may deserve review. It does not save packet payloads or collect credentials.

Potential detections can be passed to an Isolation Forest anomaly detector, stored as alerts in SQLite, and viewed through a Flask dashboard bound to the local computer. The rule and ML results are indicators, not proof of an attack; this prototype cannot detect every threat. In the current project environment, the trained ML model is absent and `pandas` is not installed, so ML predictions have not been verified.

## Problem Statement

Small labs and student projects need a simple way to observe basic network activity and review potentially suspicious patterns. Raw packet monitoring can expose more information than necessary, while separate detection, storage, and display tools can make it difficult for a beginner to follow an event from observation to alert. CyberShield IDS demonstrates a metadata-focused workflow that connects packet analysis, simple detection rules, alert storage, and a local dashboard.

## Objectives

- Monitor traffic only on authorized systems and networks.
- Extract packet metadata needed for basic analysis.
- Detect potentially suspicious activity with configurable rules.
- Experiment with Isolation Forest anomaly detection.
- Generate and manage alerts for review.
- Store alert metadata in SQLite.
- Display alerts, severity summaries, protocol summaries, and reports in a local Flask dashboard.
- Provide a simple interface for educational security monitoring.

## Scope

- Use only on your own computer, local network, or an explicitly authorized lab.
- Analyze packet metadata; packet payloads are not collected or stored.
- Do not collect passwords, credentials, or private messages.
- The project is an educational prototype, not a production security product or a guarantee that every attack will be detected.
- A rule match or ML anomaly means potentially suspicious activity, not a confirmed attack.

## Dashboard Features

- Dashboard counters, ML availability status, recent alerts, and charts refresh automatically every five seconds.
- The Alerts page supports text search and filters for severity, protocol, detection rule, source IP, and destination IP. Filter values are passed to SQLite as parameters.
- Alerts have a workflow status: `NEW`, `INVESTIGATING`, or `RESOLVED`. New alerts begin as `NEW`; status changes retain the alert.
- Each alert shows a **CyberShield IDS internal risk indicator** from 0 to 100. This is not an attack probability: the score starts at 20 for LOW, 50 for MEDIUM, or 80 for HIGH, adds 5 or 10 for selected rule indicators and 10 for an ML anomaly, then caps at 100.

## Current Validation Status

The latest system suite ran 17 tests: 16 passed and one model-present test was skipped because `ML/model.pkl` is missing. The standalone ML check also reports the missing model; the current virtual environment is missing `pandas`. The dashboard, reports, charts, polling, filters, and status workflow were checked locally. See [TESTING.md](TESTING.md) for the test cases, measured results, limitations, and security review.

## How to Run CyberShield IDS

Open PowerShell in the project folder.

1. Activate the virtual environment:

   ```powershell
   .\venv\Scripts\Activate.ps1
   ```

2. Install the project requirements:

   ```powershell
   python -m pip install -r requirements.txt
   ```

3. Train the ML model if `ML\model.pkl` does not exist:

   ```powershell
   python ML\train.py
   ```

4. Start packet monitoring only on a network and devices you own or are explicitly authorized to monitor. This captures up to 20 packets and displays metadata only:

   ```powershell
   python packet_capture.py
   ```

5. In a second PowerShell terminal, activate the environment and start the dashboard:

   ```powershell
   .\venv\Scripts\Activate.ps1
   python app.py
   ```

6. Open the local dashboard:

   ```text
   http://127.0.0.1:5000/
   ```

Run the safe system tests with:

```powershell
.\venv\Scripts\python.exe -m unittest discover -s tests -v
```

The standalone database and ML checks can be run with:

```powershell
python test_database.py
python ML\test_ml.py
```

If the ML check reports a missing model, train it with `python ML\train.py` first. Do not monitor networks or devices without permission. The Flask server binds to `127.0.0.1` and should remain local.

## Final Project Submission Checklist

- [x] Project source code and `requirements.txt`
- [x] README with project overview and run instructions
- [x] `TESTING.md` with actual test results and limitations
- [ ] Architecture diagram
- [ ] Presentation slides
- [ ] Viva questions and answers
- [ ] Genuine screenshots or output evidence from the final project run"# CyberShield-IDS" 
