# Environment Scenario

## Overview

Small business environment with one employee workstation and an SSH bastion for admin access.

| System | Role | 
|--------|------|
| Windows 10/11 | Employee Workstation | 
| Kali Linux | SSH Bastion |
| Wazuh Stack | SIEM | 

## Users

| User | System | Role |
|------|--------|------|
| `jsmith` | Windows | Business Analyst (8 AM - 5 PM) |
| `admin` | Bastion | IT Administrator |
| `kali` | Bastion | Legacy - should NOT be used |

## Normal vs Suspicious Activity

### Windows Workstation

| Normal | Suspicious |
|--------|------------|
| Excel, browser, email, Power BI | PowerShell execution |
| Login 8 AM - 5 PM | Login after hours |
| `jsmith` account | Administrator account |
| No SSH | Any SSH connection |

### Linux Bastion

| Normal | Suspicious |
|--------|------------|
| SSH from host machine | SSH from Windows workstation |
| Login as `admin` | Login as `kali` or `root` |
| Login 8 AM - 5 PM | Login after hours |
| SSH only (port 1452) | Any other service running |
