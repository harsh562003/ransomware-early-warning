# 📊 Project Metrics — Early-Warning Ransomware Detection

This document defines the **success criteria**, **formulas**, and **evaluation methods** for the project.

## 1. Time-to-Detect (TTD) Pre-Encryption
- **Definition:** The time between the first malicious *pre-encryption indicator* and the alert firing in SIEM/SOAR.
- **Formula:**  
TTD_seconds = Alert_Timestamp_UTC – First_Malicious_Action_Timestamp_UTC

- **Goal:** Median TTD ≤ **120 seconds** (tuneable by environment).
- **Examples of first malicious action:**  
- `vssadmin delete shadows` command  
- Office → PowerShell spawn  
- First high-entropy file write  


## 2. False Positives (FPs)
- **Definition:** Alerts raised that are later confirmed as benign after triage.
- **Formula:**  
FP_per_1k_per_day = (FP_count / Days) / (Hosts / 1000)

 **Goal:** ≤ **1 false positive per 1,000 hosts per day**.


## 3. Kill-Chain Coverage
- **Definition:** Percentage of targeted MITRE ATT&CK techniques detected by this project.
- **Formula:**  
Coverage (%) = (Techniques_Detected / Techniques_Targeted) * 100
**Goal:** ≥ **70% coverage** of simulated ransomware techniques.

## 4. Precision & Recall (optional with labeled test data)
- **Precision:**  
TP / (TP + FP)

- **Recall:**  
TP / (TP + FN)

- **F1 Score:**  
2 * (Precision * Recall) / (Precision + Recall)


## 5. Data Sources
- Sysmon logs (XML → JSON)  
- Windows Event Logs (Security, Application, PowerShell)  
- SIEM alerts (Elastic or Splunk)  
- SOAR logs (isolation & triage actions)  
- Red-team simulations (Atomic Red Team T1486/T1490)  


## 6. Reporting Format
Results should be stored in `tests/results.csv` with the following columns:

```csv
test_id,technique,first_indicator_ts,alert_ts,ttd_seconds,detected,fp,notes

Example:
csv
T001,T1486,2025-09-12T13:02:05Z,2025-09-12T13:03:20Z,75,TRUE,FALSE,"vssadmin + mass rename detected"
