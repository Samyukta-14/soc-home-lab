# SOC Home Lab

A Security Operations Center home lab built with Wazuh SIEM/XDR for learning threat detection, log analysis, and incident response.

## Overview

This project simulates an enterprise security monitoring environment with:
- Documented baselines defining normal activity
- Custom detection rules for baseline violations
- MITRE ATT&CK mapped detections
- Real attack simulation and triage workflows

### The Scenario

A small business environment with:
- **Windows Workstation** - Business analyst (`jsmith`) using Excel, browser, email, Power BI
- **Linux SSH Bastion** - Secure gateway for administrative access (`admin` user only)
- **Wazuh SIEM** - Centralized monitoring and detection

## Architecture

<p align="center">
  <img src="./images/architecture.png" alt="SOC Lab Architecture" width="800">
</p>

| System | Role |
|--------|------|
| Wazuh Stack | SIEM (Docker Compose) |
| Windows 10/11 | Employee Workstation |
| Kali Linux | SSH Bastion | 

## Detection Coverage

### Custom Rules Summary

| Rule ID | Target | Detection | MITRE ATT&CK |
|---------|--------|-----------|--------------|
| 100002 | Bastion | SSH login outside work hours (8AM-5PM) | T1078.003, T1021.004 |
| 100003 | Bastion | Login with legacy `kali` account | T1078.003 |
| 100004 | Bastion | SSH from Windows workstation (lateral movement) | T1021.004, T1078.003 |
| 100005 | Windows | SSH client executed | T1021.004 |
| 100006 | Bastion | New user account created | T1136.001 |
| 100007 | Bastion | New service installed | T1543.002 |
| 100011 | Windows | Login outside work hours | T1078.003 |
| 100012 | Windows | Administrator account used | T1078.003 |
| 100013 | Windows | PowerShell executed (excludes Wazuh agent) | T1059.001 |
| 100014 | Windows | cmd.exe spawned from Office apps | T1059.003 |
| 100015 | Windows | New user account created | T1136.001 |

### MITRE ATT&CK Mapping

| Tactic | Technique | ID | Detection Rules |
|--------|-----------|-----|-----------------|
| Initial Access | Valid Accounts: Local | T1078.003 | 100002, 100003, 100011, 100012 |
| Execution | PowerShell | T1059.001 | 100013 |
| Execution | Windows Command Shell | T1059.003 | 100014 |
| Persistence | Create Account: Local | T1136.001 | 100006, 100015 |
| Persistence | Systemd Service | T1543.002 | 100007 |
| Lateral Movement | Remote Services: SSH | T1021.004 | 100002, 100004, 100005 |

## Components

| Component | Purpose | Why I Chose It |
|-----------|---------|----------------|
| Wazuh | SIEM/XDR platform | Free, enterprise-grade with built-in rules and MITRE mapping |
| Docker Compose | Container orchestration | Single command deploys manager, indexer, and dashboard |
| Sysmon | Windows telemetry | Captures process creation, network connections, command lines |
| Kali Linux | SSH Bastion | Initially configured as endpoint, repurposed as bastion |
| Windows 10/11 | Employee Workstation | Realistic target for enterprise attacks |

## What Makes This Different
1. **Baseline Documentation** - Defined normal activity before building detections
2. **Custom Rules** - Detections tailored to MY environment, not generic signatures
3. **False Positive Handling** - Tuned rules to exclude legitimate activity 
4. **Attack Simulation** - Tested each detection with realistic scenarios
5. **MITRE Mapping** - Every custom rule mapped to ATT&CK framework

## Project Structure

```
soc-home-lab/
├── README.md
├── LICENSE
├── images/
│   ├── architecture.png
│   ├── wazuh-agents-active.png
│   ├── alert-after-hours-login.png
│   ├── alert-powershell-execution.png
│   ├── alert-ssh-lateral-movement.png
│   ├── alert-admin-login.png
│   ├── alert-new-user-created.png
│   ├── alert-mitre-tags.png
│   └── custom-rules-deployed.png
├── detection-rules/
│   ├── local_rules.xml
│   └── mitre-mapping.md
├── documentation/
│   ├── baselines/
│   │   ├── windows-workstation.md
│   │   └── linux-bastion.md
│   ├── environment-scenario.md
│   ├── phase1-setup.md
│   └── phase2-detections.md
└── configs/
    ├── ossec-windows.conf
    ├── ossec-linux.conf
    └── sysmonconfig.xml
```

## Resources 

- https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html
- https://www.youtube.com/watch?v=QT81wcuoRFY&t=634s
- https://attack.mitre.org/techniques/enterprise/
- https://documentation.wazuh.com/current/user-manual/ruleset/rules/custom.html
- https://smallstep.com/blog/diy-ssh-bastion-host/
