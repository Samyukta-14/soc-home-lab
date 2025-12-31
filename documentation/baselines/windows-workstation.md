# Windows Workstation Baseline

## System Information

| Attribute | Value |
|-----------|-------|
| Hostname | Windows |
| OS | Windows 10/11 |
| Role | Employee Workstation - Business Analyst |

## Accounts

| Account | Type | Status |
|---------|------|--------|
| jsmith | Standard User | Active |
| Administrator | Local Admin | Enabled (emergency only) |

## Expected Activity

| Attribute | Baseline |
|-----------|----------|
| Working Hours | 8 AM - 5 PM |
| Applications | Excel, Browser, Email, Power BI |
| SSH Connections | None |
| RDP | Disabled |
| Firewall | On |

## What's Suspicious

- Login outside 8 AM - 5 PM
- Administrator account used
- PowerShell execution
- cmd.exe spawned from Office apps
- SSH connections
- New user account created

## Hardening Applied

- RDP disabled
- Remote Registry disabled
- WinRM stopped
- No SSH server installed
- Windows Firewall enabled
