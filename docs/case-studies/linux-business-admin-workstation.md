# Linux Business Administration Workstation

## Project Summary

Designed and hardened a Linux Mint laptop as a dedicated IT administration and field-support workstation.

The objective was to build a portable system capable of securely administering infrastructure, troubleshooting networks, protecting business data, and recovering from system or data loss.

Rather than treating the laptop as a general-purpose desktop, I configured it as a managed administrative endpoint with layered security, backup, recovery, and troubleshooting capabilities.

## Project Goals

- Secure remote administration
- Restrict unnecessary inbound network access
- Support field network troubleshooting
- Protect sensitive business information
- Provide system rollback capability
- Maintain encrypted and versioned data backups
- Automate routine security and backup operations
- Test recovery rather than assuming backups are usable
- Maintain documentation of the workstation configuration

## Workstation Platform

The workstation is based on Linux Mint and includes:

- Dedicated Linux root and home filesystems
- Shared cross-platform storage for business files
- Zsh administrative shell
- Git and development tooling
- SSH remote administration
- Network troubleshooting utilities
- Encrypted business-data storage
- Automated system and data recovery mechanisms

## SSH Hardening

The SSH server was hardened for administrative remote access.

Implemented controls include:

- Disabled root SSH login
- Disabled password authentication
- Disabled keyboard-interactive authentication
- Enabled public-key authentication
- Disabled X11 forwarding
- Restricted SSH access to the authorized administrative account
- Tested SSH key authentication before disabling password access
- Validated the SSH configuration before restarting the service

Effective security policy includes:

    PermitRootLogin no
    PubkeyAuthentication yes
    PasswordAuthentication no
    KbdInteractiveAuthentication no
    X11Forwarding no

SSH connectivity was tested from multiple authorized management paths after hardening.

## Host Firewall

Configured UFW using a default-deny inbound security model.

Firewall design:

    Inbound:   DENY by default
    Outbound:  ALLOW by default
    Routed:    DENY by default
    SSH:       Trusted management networks and administrative VPN only

This allows the SSH service to operate normally while the host firewall determines which networks are permitted to reach it.

The firewall was enabled only after authorized SSH access had been tested to reduce the risk of administrative lockout.

## Automated Security Updates

Configured unattended security updates using the native APT and systemd update infrastructure.

The update configuration provides:

- Automatic package-list refresh
- Automatic installation of approved security updates
- Scheduled update execution through systemd timers
- Continued administrative control over larger system changes

This reduces the amount of manual maintenance required while helping keep the workstation patched against known vulnerabilities.

## Business Data Encryption

Sensitive business information is protected using Cryptomator encryption.

The encrypted vault backend resides within the shared business workspace, allowing the protected data to remain portable while still being stored encrypted at rest.

The unlocked plaintext Cryptomator mount is intentionally excluded from automated backups.

This prevents the backup system from unintentionally creating a second unencrypted copy of protected business information.

## System Recovery

Timeshift is configured using RSYNC snapshots for operating-system recovery.

Current retention policy:

- 3 daily snapshots
- 2 weekly snapshots
- 2 monthly snapshots

User home directories are intentionally excluded from Timeshift.

Timeshift is responsible primarily for system rollback, while Restic provides user and business-data recovery.

This separates two different recovery requirements instead of attempting to use a single tool for both purposes.

## Encrypted Data Backup

Restic is used for encrypted, deduplicated, and versioned backups to network-attached storage.

Selected backup sources include:

- SSH configuration
- Shell configuration
- Git configuration
- Documents
- Development workspace
- Homelab documentation
- Business workspace
- Encrypted Cryptomator vault backend

Large caches, temporary data, network-mounted storage, duplicate symlink paths, and the unlocked Cryptomator plaintext mount are excluded.

The backup repository is encrypted by Restic, and its password is maintained separately from the repository.

A recovery copy of the repository credential is also retained independently in a secure password manager.

## Backup Retention

The Restic retention policy is designed to maintain useful recovery history without retaining every snapshot indefinitely.

Current policy:

- 7 daily snapshots
- 4 weekly snapshots
- 6 monthly snapshots
- 1 yearly snapshot

Retention and repository maintenance are handled separately from normal backup creation.

## Backup Failure Protection

One important failure condition was specifically addressed in the backup automation.

A network mount point can continue to exist as an ordinary local directory when the remote NAS is unavailable. Without additional validation, a backup process could potentially write data into that local directory instead of the intended NAS.

The backup script therefore:

1. Verifies that required configuration files exist.
2. Attempts to access the NAS.
3. Verifies that the backup destination is actually mounted using CIFS.
4. Aborts the backup if the expected network filesystem is not present.
5. Only starts Restic after those checks succeed.

This provides protection against accidentally storing backups on the workstation because of a failed network mount.

## Restore Validation

The backup implementation was not considered complete simply because Restic successfully created snapshots.

A real restore test was performed.

Validation included:

- Restoring a complete snapshot into temporary storage
- Comparing important configuration files byte-for-byte
- Comparing the live and restored business directories
- Verifying file counts
- Verifying directory counts
- Performing checksum-based comparison
- Running Restic repository integrity checks

The restored files matched the source data.

This verified that the backup system could actually recover the protected information.

## Backup Automation

Systemd services and timers are used to automate the backup lifecycle.

### Daily Backup

A persistent systemd timer performs the regular Restic backup each evening.

The backup workflow:

1. Verifies required configuration.
2. Confirms NAS availability.
3. Confirms the destination is mounted using CIFS.
4. Executes the encrypted Restic backup.
5. Records execution information in the system journal.

Using a persistent timer allows a missed scheduled execution to run later when appropriate instead of simply being lost.

### Repository Maintenance

A separate systemd workflow performs periodic Restic repository maintenance.

The maintenance process performs:

1. Repository integrity check
2. Repository pruning
3. Final repository integrity check

Separating heavier repository maintenance from normal backup creation keeps routine backups lightweight while still providing periodic repository validation.

## Recovery Architecture

The workstation uses multiple recovery layers rather than relying on a single backup mechanism.

    Linux system failure
            |
            +--> Timeshift
                 System rollback

    User / business data loss
            |
            +--> Restic
                 Encrypted versioned recovery

    Sensitive business information
            |
            +--> Cryptomator
                 Encryption at rest

    Restic repository
            |
            +--> Network-attached storage

This provides separate controls for system recovery, data recovery, and data confidentiality.

## Field Troubleshooting Toolkit

The workstation includes a collection of tools for network and infrastructure troubleshooting.

### Network Discovery

- `nmap`
- `arping`
- `arp`
- `ip`

### DNS Troubleshooting

- `dig`
- `nslookup`
- `host`

### Routing and Latency

- `traceroute`
- `tracepath`
- `mtr`

### Network Performance

- `iperf3`

### Packet Analysis

- `tcpdump`
- Wireshark

### Wireless Networking

- `iw`
- `nmcli`

### Ethernet Diagnostics

- `ethtool`

### Connectivity and Protocol Testing

- `nc`
- `socat`
- `curl`
- `wget`
- `openssl`

### Remote Administration and File Transfer

- SSH
- `rsync`
- `smbclient`
- Remmina

These tools allow the workstation to be used for both infrastructure administration and on-site network troubleshooting.

## Security and Operational Lessons

Several important administration principles were reinforced during the build:

- Test SSH key access before disabling password authentication.
- Validate SSH configuration before restarting the SSH service.
- Use a default-deny firewall policy for inbound access.
- Restrict administrative access to trusted networks.
- Treat system rollback and data backup as separate requirements.
- Do not assume that a successful backup means the data is recoverable.
- Perform real restoration tests.
- Avoid backing up unlocked plaintext encryption mounts.
- Validate network filesystems before automated backup writes.
- Separate routine backups from heavier repository maintenance.
- Protect backup credentials independently from backup storage.
- Use systemd timers for predictable and logged automation.
- Keep credentials, internal addressing, and sensitive infrastructure information out of public documentation.

## Skills Demonstrated

This project demonstrates practical experience with:

- Linux system administration
- Linux Mint
- SSH administration and hardening
- Public-key authentication
- UFW firewall configuration
- Network segmentation concepts
- Secure remote administration
- Linux filesystem management
- SMB/CIFS network storage
- Encryption at rest
- Cryptomator
- Restic
- Timeshift
- Backup architecture
- Backup retention
- Disaster recovery
- Restore testing
- systemd services
- systemd timers
- Bash scripting
- Network troubleshooting
- DNS troubleshooting
- Packet analysis
- Git
- Technical documentation

## Result

The completed workstation provides a portable administrative platform with:

- Hardened SSH access
- Restricted inbound network connectivity
- Automated security updates
- Encrypted business-data storage
- Local system rollback
- Encrypted versioned NAS backups
- Tested data recovery
- Automated backup execution
- Automated repository maintenance
- A comprehensive network troubleshooting toolkit

The project demonstrates more than installation of individual tools. It represents the complete operational lifecycle of an administrative endpoint:

**Design → Implementation → Hardening → Automation → Testing → Recovery Validation → Documentation**