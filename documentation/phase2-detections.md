# Phase 2: Detection Rules

## Approach

1. Document baselines
2. Identify baseline violations
3. Find parent rules in Wazuh
4. Create custom rules with MITRE tags
5. Test each rule

## Parent Rules Used

| Parent Rule | Description | Used For |
|-------------|-------------|----------|
| 5715 | SSH authentication success | Bastion login rules |
| 5902 | New user added (Linux) | Bastion persistence |
| 5904 | New service installed (Linux) | Bastion persistence |
| 60106 | Windows logon success | Windows login rules |
| 60109 | Windows user created | Windows persistence |
| 61603 | Sysmon Event ID 1 (Process Create) | Windows process rules |

## Custom Rules Location

```
/var/ossec/etc/rules/local_rules.xml
```

Or via Docker volume:
```
/var/lib/docker/volumes/single-node_wazuh_etc/_data/rules/local_rules.xml
```

## Rules Created

### Bastion Rules

| Rule | Detection | Level |
|------|-----------|-------|
| 100002 | SSH outside work hours | 10 |
| 100003 | Login as `kali` | 12 |
| 100004 | SSH from Windows workstation | 13 |
| 100006 | New user created | 14 |
| 100007 | New service installed | 14 |

### Windows Rules

| Rule | Detection | Level |
|------|-----------|-------|
| 100005 | SSH client executed | 12 |
| 100011 | Login outside work hours | 10 |
| 100012 | Administrator account used | 12 |
| 100013 | PowerShell executed | 12 |
| 100014 | cmd.exe from Office | 14 |
| 100015 | New user created | 14 |

## Tuning Applied

### False Positive: Wazuh Agent PowerShell

**Alert:** Rule 100013 fired on Wazuh SCA scans

**Solution:** Exclude Wazuh agent as parent process
```xml
<field name="win.eventdata.parentImage" negate="yes" type="pcre2">(?i)\\wazuh-agent\.exe$</field>
```

### Duplicate Alert: User Creation

**Alert:** Rule 100015 fired twice (Event 4720 + 4722)

**Solution:** Filter to Event ID 4720 only
```xml
<field name="win.system.eventID">^4720$</field>
```
