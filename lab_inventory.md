# 🖥️ Lab Inventory — Early-Warning Ransomware Detection

This file documents the virtual machines, roles, and configurations used in the project.



## Lab Setup Overview
- **Environment:** Isolated VLAN with VM snapshots  
- **Virtualization:** VMware Workstation / VirtualBox / Proxmox  
- **Logging:** Sysmon + Winlogbeat → Elastic Stack (or Splunk)  
- **SOAR:** TheHive + Cortex / Shuffle (open source)  
- **Testing:** Atomic Red Team simulations  


## Inventory Table

| ID        | Hostname       | Role         | OS / Version        | IP (Lab)   | Snapshot Path                  | Sysmon Config   | EDR/Agent        | SIEM Ingest         | Notes |
|-----------|---------------|--------------|---------------------|------------|--------------------------------|-----------------|------------------|---------------------|-------|
| host-01   | win10-client   | Endpoint     | Windows 10 22H2     | 10.0.0.11  | /snapshots/win10-client.vmx   | rules/sysmon.xml | Defender AV + Sysmon | Winlogbeat → ELK    | Main test workstation |
| host-02   | win11-client   | Endpoint     | Windows 11 23H2     | 10.0.0.12  | /snapshots/win11-client.vmx   | rules/sysmon.xml | Sysmon only      | Winlogbeat → ELK    | Secondary endpoint |
| siem-01   | elk-server     | SIEM         | Ubuntu 22.04        | 10.0.0.50  | /snapshots/elk-server.vmx     | N/A             | N/A              | N/A                 | Elastic + Kibana |
| soar-01   | hive-server    | SOAR         | Ubuntu 22.04        | 10.0.0.60  | /snapshots/hive-server.vmx    | N/A             | N/A              | N/A                 | TheHive + Cortex |
| attacker1 | red-team       | Simulation   | Kali Linux 2025.1   | 10.0.0.99  | /snapshots/red-team.vmx       | N/A             | N/A              | N/A                 | Atomic Red Team runner |


## Notes
- **Snapshots**: Each VM has baseline snapshot (`clean-install`) and `pre-test` snapshot to roll back after simulations.  
- **Networking**: All VMs are on isolated NAT network `10.0.0.0/24`. No internet exposure.  
- **Secrets**: Credentials stored in password vault, not in repo.  
- **Logs**: Forwarded centrally to `siem-01` for correlation and anomaly detection.  
