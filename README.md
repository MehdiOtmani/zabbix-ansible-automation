# Zabbix Ansible Automation

Automated deployment of Zabbix agents on multiple Linux hosts using Ansible.

[![Ansible](https://img.shields.io/badge/Ansible-2.9%2B-blue)](https://www.ansible.com/)
[![Zabbix](https://img.shields.io/badge/Zabbix-Monitoring-orange)](https://www.zabbix.com/)
[![Linux](https://img.shields.io/badge/Linux-Ubuntu%2FDebian-lightgrey)](https://www.linux.org/)
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
- Ubuntu/Debian
- User with sudo privileges
- Network access to Zabbix server

## Quick Start

### 1. Clone and Setup
git clone https://github.com/YOURUSERNAME/zabbix-ansible-automation.git
cd zabbix-ansible-automation


### 2. Edit Inventory
Update `inventory.yml` with your host IPs and SSH user:


### 3. Update Zabbix Server IP
Edit `zabbix-agent.yml` and change:
zabbix_server_ip: "192.168.101.34" # Your Zabbix server

### 4. Setup SSH Keys
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@host_ip

*Note: For multiple hosts, consider using Ansible's `authorized_key` module to automate key distribution.*

### 5. Test Connection
ansible -i inventory.yml zabbix_agents -m ping


### 6. Deploy
ansible-playbook -i inventory.yml zabbix-agent-setup.yml


## What Gets Configured
The playbook modifies `/etc/zabbix/zabbix_agentd.conf`:
- **Server**: Your Zabbix server IP (passive checks)
- **ServerActive**: Your Zabbix server IP (active checks)
- **Hostname**: Set from inventory hostname

## Project Structure
zabbix-ansible-automation/
  - README.md
  - ansible.cfg # Ansible settings
  - inventory.yml # Your hosts (gitignored)
  - zabbix-agent.yml # Main playbook


## Troubleshooting
### Agent not showing in Zabbix UI

- Check service status:
sudo systemctl status zabbix-agent

- Verify configuration:
grep -E '^(Server|ServerActive|Hostname)=' /etc/zabbix/zabbix_agentd.conf

- Test SSH connection:
ssh -i ~/.ssh/id_ed25519 user@host_ip

- Verify inventory:
ansible-inventory -i inventory.yml --graph


## Real-World Use :
Deployed across 15+ Ubuntu VMs running security tools (MISP, TheHive, Wazuh) to standardize monitoring infrastructure. Reduced deployment time from 15 minutes per host to under 2 minutes for batch operations.

## Contact
**LinkedIn**: [El Mehdi El Otmani](https://www.linkedin.com/in/elmehdielotmani/)  
**Email**: elmehdielotmani11@gmail.com

---
