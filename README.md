# Zabbix Ansible Automation

Automated deployment of Zabbix agents on multiple Linux hosts using Ansible.

[![Ansible](https://img.shields.io/badge/Ansible-2.9%2B-blue)](https://www.ansible.com/)
[![Zabbix](https://img.shields.io/badge/Zabbix-Monitoring-orange)](https://www.zabbix.com/)
[![Linux](https://img.shields.io/badge/Linux-Ubuntu%2022.04%20LTS-E95420?logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

## What It Does

- Installs Zabbix agent on Ubuntu/Debian hosts
- Configures agent to connect to your Zabbix server
- Enables and starts the service automatically
- Backs up configuration before changes

## Requirements

**Control Machine:**
- Ansible 2.9+
- SSH access to target hosts

**Target Hosts:**
- Ubuntu/Debian 22.04 LTS
- User with sudo privileges
- Network access to Zabbix server

## Quick Start

### 1. Clone and Setup
```bash
git clone https://github.com/MehdiOtmani/zabbix-ansible-automation.git
cd zabbix-ansible-automation
 ```

### 2. Edit Inventory.yml & ansible.cfg
Update `inventory.yml` with your host IPs 
Update `ansible.cfg` with your user on the remote hosts and your private key location 

### 3. Update Zabbix Server IP
Edit `zabbix-agent.yml` and change:
```bash
zabbix_server_ip: "192.168.101.34" # Your Zabbix server
 ```

### 4. Setup SSH Keys
```bash
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@host_ip
 ```
*Note: For multiple hosts, consider using Ansible's `authorized_key` module to automate key distribution.*

### 5. Test Connection
```bash
ansible -i inventory.yml zabbix_agents -m ping
 ```

### 6. Deploy
```bash
ansible-playbook -i inventory.yml zabbix-agent-setup.yml
 ```

## What Gets Configured
The playbook modifies `/etc/zabbix/zabbix_agentd.conf`:
- **Server**: Your Zabbix server IP (passive checks)
- **ServerActive**: Your Zabbix server IP (active checks)
- **Hostname**: Set from inventory hostname

## Project Structure
```bash
zabbix-ansible-automation/
  - group_vars:
      zabbix_agents.yml
  - README.md
  - ansible.cfg # Ansible settings
  - inventory.yml # Your hosts (gitignored)
  - zabbix-agent-setup.yml # Main playbook
```

## Troubleshooting
### Agent not showing in Zabbix UI
- Check service status:
```bash
sudo systemctl status zabbix-agent
 ```
- Verify configuration:
```bash
grep -E '^(Server|ServerActive|Hostname)=' /etc/zabbix/zabbix_agentd.conf
 ```
- Test SSH connection:
```bash
ssh -i ~/.ssh/id_ed25519 user@host_ip
 ```

- Verify inventory:
```bash
ansible-inventory -i inventory.yml --graph
 ```

## Real-World Use :
Deployed across 15+ Ubuntu VMs running security tools (MISP, TheHive, Wazuh) to standardize monitoring infrastructure. Reduced deployment time from 15 minutes per host to under 2 minutes for batch operations.

## Contact
**LinkedIn**: [El Mehdi El Otmani](https://www.linkedin.com/in/elmehdielotmani/)  
**Email**: elmehdielotmani11@gmail.com

---
