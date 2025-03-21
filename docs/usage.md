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

### Limiting to Specific Hosts or Groups

To run maintenance only on specific hosts or groups:

```bash
# Run on a specific host
./labops.sh --limit seraph

# Run on a group of hosts
./labops.sh --limit storage

# Run on multiple specific hosts
./labops.sh --limit "seraph,jarvis"
```

### Running Specific Tasks

You can use tags to run only specific types of tasks:

```bash
# Only run system updates
./labops.sh --tags system

# Only run Docker tasks
./labops.sh --tags docker

# Run both system and Docker tasks but skip backup
./labops.sh --tags "system,docker"
```

## Advanced Usage

### Dry Run Mode

To see what changes would be made without actually making them:

```bash
./labops.sh --check
```

### Verbosity Levels

You can increase the verbosity for more detailed output:

```bash
# Normal verbosity
./labops.sh

# Increased verbosity
./labops.sh --verbose

# Maximum verbosity
./labops.sh -vvv
```

### SSH Key Authentication

If you've set up SSH keys, you can skip password prompts:

```bash
./labops.sh --no-password
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
0 2 * * 0 /path/to/labops/labops.sh --no-password >> /path/to/labops/logs/cron.log 2>&1
```

## Configuration File

You can customize default settings in the `labops.conf` file:

```bash
vi labops.conf
```

This lets you set defaults for inventory location, playbook selection, backup retention, and more.

## Common Command Examples

Here are some useful example commands:

```bash
# Update only Ubuntu systems
./labops.sh --limit ubuntu --tags system

# Only update Docker containers on storage servers
./labops.sh --limit storage --tags docker

# Only perform health checks on all systems
./labops.sh --tags healthcheck
```

For more information, see the [Customizing Guide](customizing.md).
