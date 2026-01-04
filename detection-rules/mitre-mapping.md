# MITRE ATT&CK Mapping

## Coverage by Tactic

### Initial Access

| Technique | ID | Rules |
|-----------|----|-------|
| Valid Accounts: Local Accounts | T1078.003 | 100002, 100003, 100004, 100011, 100012 |

### Execution

| Technique | ID | Rules |
|-----------|----|-------|
| PowerShell | T1059.001 | 100013 |
| Windows Command Shell | T1059.003 | 100014 |

### Persistence

| Technique | ID | Rules |
|-----------|----|-------|
| Create Account: Local Account | T1136.001 | 100006, 100015 |
| Systemd Service | T1543.002 | 100007 |

### Lateral Movement

| Technique | ID | Rules |
|-----------|----|-------|
| Remote Services: SSH | T1021.004 | 100002, 100004, 100005 |

## Coverage by Rule

| Rule ID | Technique(s) | Tactic(s) |
|---------|--------------|-----------|
| 100002 | T1078.003, T1021.004 | Initial Access, Lateral Movement |
| 100003 | T1078.003 | Initial Access |
| 100004 | T1021.004, T1078.003 | Lateral Movement, Initial Access |
| 100005 | T1021.004 | Lateral Movement |
| 100006 | T1136.001 | Persistence |
| 100007 | T1543.002 | Persistence |
| 100011 | T1078.003 | Initial Access |
| 100012 | T1078.003 | Initial Access |
| 100013 | T1059.001 | Execution |
| 100014 | T1059.003 | Execution |
| 100015 | T1136.001 | Persistence |

## References

- [MITRE ATT&CK Enterprise Matrix](https://attack.mitre.org/matrices/enterprise/)
