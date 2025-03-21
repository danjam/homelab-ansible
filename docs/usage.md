# LabOps Usage Guide

This guide explains how to effectively use LabOps for maintaining your home lab environment.

## Basic Usage

### Running a Full Maintenance Operation

To perform a complete maintenance operation on all systems:

```bash
./labops.sh
```

This will:
1. Perform health checks on all systems
2. Update operating systems and packages
3. Manage Docker containers and images
4. Verify systems after updates

The maintenance process follows this workflow:
- Initial health check to ensure system viability
- System updates with controlled reboots if required
- Docker container updates and management
- Post-update verification to confirm system health

### Limiting to Specific Hosts or Groups

You can target specific hosts or groups using the `--limit` option:

```bash
# Run on a specific host
./labops.sh --limit seraph

# Run on a group of hosts
./labops.sh --limit storage

# Run on multiple specific hosts
./labops.sh --limit "seraph,jarvis"

# Run on all Ubuntu hosts
./labops.sh --limit ubuntu
```

### Running Specific Tasks

Use tags to run only specific types of tasks:

```bash
# Only run system health checks
./labops.sh --tags healthcheck

# Only run system updates
./labops.sh --tags system

# Only run Docker tasks
./labops.sh --tags docker

# Run both system and Docker tasks
./labops.sh --tags "system,docker"
```

## Advanced Usage

### Dry Run Mode

To see what changes would be made without actually making them:

```bash
./labops.sh --check
```

This is useful for validating what would happen during maintenance before committing to changes.

### Verbosity Levels

You can increase the verbosity for more detailed output:

```bash
# Normal verbosity
./labops.sh

# Increased verbosity
./labops.sh -v

# High verbosity (useful for debugging)
./labops.sh -vv

# Maximum verbosity
./labops.sh -vvv
```

### SSH Key Authentication

If you've set up SSH keys, you can skip password prompts:

```bash
./labops.sh --no-password
```

### Using Different Inventory or Playbook

```bash
# Use a different inventory file
./labops.sh --inventory /path/to/custom/inventory.yml

# Use a different playbook
./labops.sh --playbook /path/to/custom/playbook.yml
```

## Automation with Cron

You can schedule LabOps to run automatically using cron:

```bash
# Edit your crontab
crontab -e
```

Add one of these example entries:

```
# Run weekly maintenance on Sunday at 2 AM
0 2 * * 0 /path/to/labops/labops.sh --no-password > /path/to/labops/logs/cron.log 2>&1

# Run daily health check at 7 AM
0 7 * * * /path/to/labops/labops.sh --no-password --tags healthcheck > /path/to/labops/logs/healthcheck.log 2>&1

# Update Docker containers every Wednesday at 3 AM
0 3 * * 3 /path/to/labops/labops.sh --no-password --tags docker > /path/to/labops/logs/docker.log 2>&1
```

## Task Descriptions

### Health Checks

The health check task (`--tags healthcheck`) evaluates:

- System connectivity
- Disk space on all mounted filesystems (warns at 85% usage)
- Memory usage (warns at 90% usage)
- System load averages (warns when exceeding CPU count)
- Process count
- Available updates count

Health checks fail when multiple critical issues are detected, preventing other tasks from running until resolved.

### System Updates

The system update task (`--tags system`) performs:

- Package cache updates
- Full system upgrades with proper handling of configuration files
- Cleanup of unused packages
- Tracking of held packages
- Kernel update detection and controlled reboots
- Post-update service health verification

### Docker Management

The Docker management task (`--tags docker`) handles:

- Docker service health verification
- Finding and updating Docker Compose projects
- Pulling latest container images
- Recreating containers with updated images
- Health checking containers after updates
- Pruning unused resources (images, containers, networks)

### Verification

The verification task checks that systems are healthy after updates:

- Network connectivity
- Internet connectivity
- Critical service status
- Disk usage after updates
- System load after updates

## Common Command Examples

Here are some useful example commands:

```bash
# Update only Ubuntu systems
./labops.sh --limit ubuntu --tags system

# Only update Docker containers on storage servers
./labops.sh --limit storage --tags docker

# Only perform health checks on all systems
./labops.sh --tags healthcheck

# Perform a full maintenance run with notification but skip password prompts
./labops.sh --no-password

# Check what Docker updates would be performed without making changes
./labops.sh --tags docker --check
```

## Working with Logs

LabOps automatically logs all operations to the `logs/` directory. Each run creates a timestamped log file:

```bash
# View the latest log file
ls -lt logs/ | head -2
cat logs/labops_20250321_120000.log

# Search logs for errors
grep -r "ERROR" logs/
```

For more detailed configuration information, see the [Installation Guide](installation.md) and [Notification Guide](notifications.md).