# LabOps Installation Guide

This guide will walk you through the process of setting up LabOps for your home lab environment.

## Prerequisites

Before installing LabOps, ensure you have the following prerequisites:

- A control node with:
  - Python 3.8 or newer
  - Ansible 2.12 or newer
  - Git
- SSH access to your managed hosts
- Sudo/root privileges on all systems

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/danjam/labops.git
cd labops
```

### 2. Install Required Ansible Collections

The project requires several Ansible collections. These may include:

```bash
# Install community.general (for mail module)
ansible-galaxy collection install community.general

# Install community.docker (for Docker management)
ansible-galaxy collection install community.docker
```

### 3. Configure Your Inventory

The inventory is defined in `inventory/inventory.yml`. This file contains all your hosts organized into groups:

```bash
# Edit the inventory file
vi inventory/inventory.yml
```

The inventory is organized into:
- **OS-based groups**: ubuntu, synology, windows, apple
- **Functional groups**: storage, workstations, homelab
- **OS-family groups**: linux, unix_like, all_systems

For each host, you'll need to specify:
- `ansible_host`: The IP address or hostname
- `ansible_user`: SSH username for connecting to the host
- `ansible_become`: Whether to use privilege escalation (sudo)

Example host entry:
```yaml
seraph:
  ansible_host: 192.168.1.100
  ansible_user: admin
```

### 4. Configure Global Settings

Edit the `labops.conf` file to customize default behavior:

```bash
vi labops.conf
```

Key settings include:
- Default inventory and playbook paths
- SSH authentication method
- Log directory
- Notification settings
- Docker paths and settings
- System update settings
- Critical services to monitor

### 5. Setup Notifications (Optional)

To enable notifications, edit `labops.conf` and uncomment the relevant section:

#### Email Notifications
```
SMTP_SERVER="smtp.gmail.com"
SMTP_PORT="587"
SMTP_USER="your-email@gmail.com"
SMTP_PASSWORD="your-app-password"
NOTIFICATION_EMAIL="admin@example.com"
```

#### Telegram Notifications
```
TELEGRAM_BOT_TOKEN="your-bot-token"  # From @BotFather
TELEGRAM_CHAT_ID="your-chat-id"      # User or group chat ID
TELEGRAM_SILENT_NOTIFICATION="false" # Set to true for silent notifications
```

See [Notification Guide](notifications.md) for detailed setup instructions.

### 6. Make the Script Executable

```bash
chmod +x labops.sh
```

### 7. Setup SSH Keys (Optional but Recommended)

For passwordless automation:

```bash
# Generate an SSH key if you don't have one
ssh-keygen -t ed25519 -C "labops"

# Copy your key to each host
ssh-copy-id user@host
```

If you set up SSH keys, you can use the `--no-password` option when running the script.

### 8. Verify Your Installation

Run a simple check to verify everything is set up correctly:

```bash
./labops.sh --list-hosts
```

This should list all the hosts in your inventory without making any changes.

## Next Steps

Once installation is complete, you can:

1. Run your first health check:
   ```bash
   ./labops.sh --tags healthcheck
   ```

2. Run a full maintenance operation:
   ```bash
   ./labops.sh
   ```

3. Schedule regular maintenance with cron:
   ```bash
   # Edit your crontab
   crontab -e
   
   # Add a weekly maintenance job (Sunday at 2 AM)
   0 2 * * 0 /path/to/labops/labops.sh --no-password > /path/to/labops/logs/cron.log 2>&1
   ```

## Troubleshooting

If you encounter issues during installation:

1. Verify Ansible is installed correctly:
   ```bash
   ansible --version
   ```

2. Check connectivity to your hosts:
   ```bash
   ansible all -i inventory/inventory.yml -m ping
   ```

3. Verify Python is available on all hosts:
   ```bash
   ansible all -i inventory/inventory.yml -m raw -a "python3 --version || python --version"
   ```

4. Check for syntax errors in playbooks:
   ```bash
   ansible-playbook --syntax-check playbooks/homelab_maintenance.yaml -i inventory/inventory.yml
   ```

5. Run with increased verbosity for more detailed error messages:
   ```bash
   ./labops.sh -vvv
   ```