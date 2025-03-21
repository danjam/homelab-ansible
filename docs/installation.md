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
git clone https://github.com/yourusername/labops.git
cd labops
```

### 2. Install Required Ansible Collections

```bash
ansible-galaxy collection install -r requirements.yml
```

### 3. Configure Your Inventory

The inventory is structured in multiple files for better organization:

1. Edit host definitions in the `inventory/hosts/` directory:

```bash
# Example: Edit Ubuntu hosts
vi inventory/hosts/ubuntu_hosts.yml
```

2. Customize group variables in the `inventory/vars/` directory:

```bash
# Example: Edit Ubuntu variables
vi inventory/vars/ubuntu_vars.yml
```

3. If needed, modify group definitions in `inventory/groups/`:

```bash
# Example: Edit function-based groups
vi inventory/groups/function_groups.yml
```

### 4. Configure Notifications (Optional)

To enable notifications:

1. Edit the `labops.conf` file:

```bash
vi labops.conf
```

2. Uncomment and set the notification settings:

```bash
SMTP_SERVER="smtp.gmail.com"
SMTP_PORT="587"
SMTP_USER="your-email@gmail.com"
SMTP_PASSWORD="your-app-password"
NOTIFICATION_EMAIL="admin@example.com"
```

### 5. Make the Script Executable

```bash
chmod +x labops.sh
```

### 6. Setup SSH Keys (Optional but Recommended)

For passwordless automation:

```bash
# Generate an SSH key if you don't have one
ssh-keygen -t ed25519 -C "labops"

# Copy your key to each host
ssh-copy-id user@host
```

If you set up SSH keys, you can use the `--no-password` option when running the script.

### 7. Verify Your Installation

Run a simple check to verify everything is set up correctly:

```bash
./labops.sh --list-hosts
```

This should list all the hosts in your inventory without making any changes.

## Next Steps

Once installation is complete, you can:

1. Run your first maintenance operation:
   ```bash
   ./labops.sh
   ```

2. Run health checks on your systems:
   ```bash
   ./labops.sh --tags healthcheck
   ```

3. Schedule regular maintenance with cron (see [Usage Examples](usage.md)).

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

For more detailed troubleshooting, see the [Troubleshooting Guide](troubleshooting.md).