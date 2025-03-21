# HomelabOps

![HomelabOps](https://via.placeholder.com/800x200?text=HomelabOps)

A comprehensive automation framework for managing and maintaining your home lab infrastructure. HomelabOps uses Ansible to automate system updates, Docker container management, health checks, and notifications.

## Features

- 🔄 **Automated System Updates**: Keep all systems up-to-date while handling reboots safely
- 🐳 **Docker Management**: Update containers and prune unused resources
- 🔍 **Health Monitoring**: Comprehensive checks for disk space, memory, load, and services
- 📧 **Notifications**: Email, webhook, and Telegram alerts for important events
- ✅ **Verification**: Post-update system verification ensures everything is working
- 🔌 **Multi-platform**: Support for Ubuntu, Synology, Windows, and macOS hosts

## Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/homelabops.git
cd homelabops

# Install required Ansible collections
ansible-galaxy collection install -r requirements.yml

# Configure your inventory by editing files in inventory/hosts/ directory
vi inventory/hosts/ubuntu_hosts.yml

# Make the script executable
chmod +x homelabops.sh

# Run the maintenance script
./homelabops.sh
```

## Project Structure

```
homelabops/
├── ansible.cfg                # Ansible configuration
├── homelabops.conf            # Configuration settings
├── homelabops.sh              # Main execution script
├── README.md                  # Project documentation
├── requirements.yml           # Required Ansible collections
│
├── docs/                      # Documentation directory
│   ├── installation.md        # Installation instructions
│   ├── notifications.md       # Notification setup guide
│   └── usage.md               # Usage examples and instructions
│
├── inventory/                 # Inventory structure
│   ├── all.yml                # Global variables
│   ├── groups/                # Group definitions
│   │   ├── function_groups.yml # Function-based groups
│   │   └── os_groups.yml      # OS-based groups
│   ├── hosts/                 # Host definitions
│   │   ├── apple_hosts.yml    # Apple/macOS hosts
│   │   ├── synology_hosts.yml # Synology NAS hosts
│   │   ├── ubuntu_hosts.yml   # Ubuntu Linux hosts
│   │   └── windows_hosts.yml  # Windows hosts
│   ├── inventory.yml          # Main inventory file
│   └── vars/                  # Group variables
│       ├── apple_vars.yml     # Apple/macOS variables
│       ├── synology_vars.yml  # Synology variables
│       ├── ubuntu_vars.yml    # Ubuntu variables
│       └── windows_vars.yml   # Windows variables
│
├── logs/                      # Log directory
│   └── .gitkeep               # Placeholder to maintain directory
│
├── playbooks/                 # Ansible playbooks
│   └── homelab_maintenance.yaml # Main maintenance playbook
│
└── tasks/                     # Task files
    ├── healthcheck.yaml       # System health checks
    ├── send_notification.yaml # Notification system
    ├── update_docker.yaml     # Docker container management
    ├── update_system_ubuntu.yaml # Ubuntu system updates
    └── verify_system.yaml     # Post-update verification
```

## Documentation

- [Installation Guide](docs/installation.md) - How to install and set up HomelabOps
- [Usage Guide](docs/usage.md) - How to use HomelabOps effectively
- [Notification Setup](docs/notifications.md) - Configure various notification methods

## Requirements

- Ansible Core 2.12+
- Python 3.8+
- SSH access to managed systems
- For Docker tasks: Docker, Docker Compose v2
- For notifications: Appropriate credentials (email, Telegram, webhook)

## Command-Line Options

```bash
Usage: ./homelabops.sh [options]

Options:
  -i, --inventory INVENTORY  Specify inventory file
  -p, --playbook PLAYBOOK    Specify playbook file
  -t, --tags TAGS            Specify tags (e.g., system,docker)
  -v, --verbose              Increase verbosity (-v, -vv, or -vvv)
  -S, --no-password          Don't ask for passwords (use SSH keys)
  -l, --limit HOSTS          Limit execution to specified hosts
  -c, --check                Run in check mode (dry run)
  --list-hosts               List all hosts in the inventory
  --version                  Show version information
  -h, --help                 Show this help message
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- The Ansible community for their excellent documentation
- All contributors to the project

## Support

If you find this project useful, please consider giving it a star on GitHub!