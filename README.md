# 🛡️ Early-Warning Ransomware Detection

Detect ransomware *before* encryption detonates using hardened telemetry (Sysmon + Windows Event Logs + EDR), deterministic rules, and lightweight anomaly detection (EWMA).  
This project demonstrates how SOC teams can reduce ransomware impact by identifying **pre-encryption signals** such as shadow copy deletion, Office → PowerShell child processes, mass renames, and backup tampering.

---

## 📌 Problem Statement
Ransomware often achieves impact by encrypting files and deleting system recovery mechanisms.  
**Traditional detections trigger after encryption begins** — leaving little time for response.  

👉 Goal: Catch ransomware **early** in its kill-chain by fusing:
- **Deterministic detections** (high-confidence rules for pre-encryption actions)
- **Statistical anomaly detection** (baseline of rename/write rates with EWMA)
- **SOAR playbooks** (auto isolate compromised hosts and collect triage bundles)

---

## 🎯 Objectives
- **Telemetry Collection**  
  - Harden Sysmon configuration and collect Windows event logs  
  - Integrate with EDR telemetry and forward to SIEM  

- **Deterministic Rules**  
  - Shadow copy deletion (`vssadmin delete shadows`)  
  - Office applications spawning PowerShell/CMD  
  - Backup service tampering  
  - Unsigned driver loads  
  - Mass file renames & high-entropy writes  

- **Anomaly Detection**  
  - EWMA-based statistical baselines for per-host file rename & write rates  
  - Simple outlier detection with optional ML (IsolationForest)  

- **SOAR Automation**  
  - Auto-isolate suspicious hosts  
  - Collect triage artifacts (Sysmon logs, process trees, hashes, memory snapshots)  
  - Escalate alerts into ticketing systems  

- **Evaluation**  
  - Time-to-Detect (TTD) *before encryption*  
  - False positives per 1,000 hosts/day  
  - Kill-chain coverage (mapped to MITRE ATT&CK)  

---

## 📦 Deliverables
- **Sysmon Config** → `rules/sysmon.xml`  
- **Sigma Rules & SIEM Queries** → `rules/sigma/`  
- **EWMA Anomaly Detector (Python)** → `notebooks/ewma.ipynb`  
- **SOAR Playbooks** → `playbooks/`  
- **Runbooks & Triage Checklist** → `docs/runbooks/`  
- **MITRE ATT&CK Mapping** → `docs/attack_mapping.csv`  
- **Test Movie + Logs** → `tests/media/`  
- **Evaluation Report** → `metrics.md`  

---

## 🏗️ Architecture
![Architecture Diagram](ARCHITECTURE.png)

**Flow:**
1. Sysmon + Windows Event Logs + EDR telemetry → forwarded to SIEM  
2. SIEM runs deterministic rules + anomaly correlation  
3. EWMA anomaly service maintains baselines per host  
4. Alerts → SOAR playbook → auto isolate host + collect triage bundle  
5. Analysts receive enriched tickets for response  

---

## 🧪 Lab Inventory
See [`lab_inventory.md`](lab_inventory.md) for full environment details.  
Example setup:
- `host-01` → Windows 10 endpoint (Sysmon + Defender AV)  
- `siem-01` → Elastic Stack (Ubuntu 22.04)  
- `soar-01` → TheHive + Cortex for automation  
- `attacker-01` → Red-team simulation VM (Atomic Red Team)  

---

## 📊 Metrics & Success Criteria
Defined in [`metrics.md`](metrics.md).  

Key evaluation metrics:
- **TTD Pre-Encryption** ≤ 120 seconds (median)  
- **False Positives** ≤ 1 per 1,000 hosts/day  
- **Coverage** ≥ 70% of targeted MITRE ATT&CK techniques  

Formulas:  
- `TTD = Alert_Timestamp - First_Malicious_Action_Timestamp`  
- `FP/day = (FP_count / days) / (hosts / 1000)`  

---

