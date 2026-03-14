# Firewall Policy

The firewall model follows least privilege and zero-trust segmentation principles.

## Default Posture

- deny inter-VLAN traffic
- allow only approved flows
- restrict access by source, destination, and service

## Example Allowed Flows

- Trusted -> Servers
- Admin -> Infrastructure
- Automation -> IoT
- Work -> approved internal services

## Example Blocked Flows

- IoT -> Trusted
- Guest -> Internal
- Gaming -> Infrastructure
- Quarantine -> Internal