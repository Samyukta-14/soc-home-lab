# Phase 1: Environment Setup

## Wazuh SIEM Deployment

Deployed Wazuh using Docker Compose (single-node configuration).

**Components:**
- Wazuh Manager (port 1514)
- Wazuh Indexer (port 9200)
- Wazuh Dashboard (port 443)

**Setup:**
```bash
git clone https://github.com/wazuh/wazuh-docker.git
cd wazuh-docker/single-node
docker-compose up -d
```

## Linux Bastion Setup

### 1. Create admin user
```bash
sudo adduser admin
sudo usermod -aG sudo admin
```

### 2. Disable root SSH login
```bash
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin no
sudo systemctl restart sshd
```

### 3. Remove unnecessary services
```bash
sudo systemctl stop named
sudo systemctl disable named
```

### 4. Verify only SSH running
```bash
sudo ss -tulnp
# Should show only SSH on port 1452
```

### 5. Install Wazuh agent
```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
apt-get install wazuh-agent
# Configure to point to manager IP
```

## Windows Workstation Setup

### 1. Create/verify user account
```powershell
# Renamed vboxuser to jsmith
Rename-LocalUser -Name "vboxuser" -NewName "jsmith"
```

### 2. Enable Administrator account
```powershell
net user Administrator /active:yes
net user Administrator <password>
```

### 3. Verify hardening
```powershell
# RDP disabled
reg query "HKLM\System\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections
# Should return 0x1

# Firewall on
netsh advfirewall show allprofiles state
# Should show ON

# Remote Registry disabled
Get-Service RemoteRegistry
# Should show Disabled

# No SSH server
Get-Service sshd
# Should error (not found)
```

### 4. Install Sysmon
```powershell
# Using SwiftOnSecurity config
sysmon64.exe -accepteula -i sysmonconfig.xml
```

### 5. Install Wazuh agent
- Download from Wazuh dashboard
- Configure manager IP
- Verify agent appears in dashboard

## Verification

| Check | Command | Expected |
|-------|---------|----------|
| Bastion SSH only | `ss -tulnp` | Port 1452 only |
| Root login disabled | `ssh root@bastion` | Permission denied |
| Wazuh agents connected | Dashboard → Agents | Both showing Active |
| Sysmon logging | Event Viewer → Sysmon | Events present |
