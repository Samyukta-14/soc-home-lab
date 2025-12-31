# Linux SSH Bastion Baseline

## System Information

| Attribute | Value |
|-----------|-------|
| Hostname | kali |
| OS | Kali Linux |
| Role | SSH Bastion / Jump Server |

## Accounts

| Account | Type | Status |
|---------|------|--------|
| admin | Sudo User | Active |
| kali | Legacy | Do not use |
| root | Root | Direct login disabled |

## Expected Activity

| Attribute | Baseline |
|-----------|----------|
| SSH Hours | 8 AM - 5 PM |
| SSH Source | Host machine only (not Windows workstation) |
| Services | SSH (port 1452) only |
| Outbound | Package updates only |

## What's Suspicious

- SSH from unauthorized IP
- SSH login outside 8 AM - 5 PM
- Login as `kali` or `root`
- Multiple failed login attempts
- New user created
- New service installed

## Hardening Applied

- Root SSH login disabled (`PermitRootLogin no`)
- SSH on non-standard port (1452)
- Unnecessary services removed (DNS/named)
- Only SSH running
