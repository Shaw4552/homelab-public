# IP Addressing Strategy

## Purpose

This document defines the IP addressing model used across the environment.

The design focuses on predictability, consistency across sites, and alignment with network segmentation to simplify troubleshooting and operational management.

---

## Addressing Model

The environment uses RFC1918 private address space with a structured, per-site allocation model.

- Site A → `10.10.x.0/24`
- Site B → `10.20.x.0/24`

The third octet aligns with the VLAN ID where practical.

This creates a direct relationship between VLAN and subnet, improving visibility and reducing operational complexity.

---

## Design Principles

- consistent addressing across multiple sites
- alignment between VLAN ID and subnet
- easy identification of network segments
- scalable structure for future expansion
- simplified troubleshooting and routing

---

## VLAN-to-Subnet Mapping

Each VLAN is assigned a /24 subnet that maps to its function and trust level.

| VLAN ID | Name           | Example Subnet |
|--------|----------------|----------------|
| 10     | Admin          | 10.x.10.0/24   |
| 20     | Trusted        | 10.x.20.0/24   |
| 30     | IoT            | 10.x.30.0/24   |
| 40     | Guest          | 10.x.40.0/24   |
| 50     | Servers        | 10.x.50.0/24   |
| 60     | Work           | 10.x.60.0/24   |
| 70     | Kids           | 10.x.70.0/24   |
| 80     | Quarantine     | 10.x.80.0/24   |
| 90     | Gaming         | 10.x.90.0/24   |
| 99     | Infrastructure | 10.x.99.0/24   |

---

## Address Allocation Standard

Each subnet follows a consistent internal allocation model:

- `.1` → default gateway
- `.2–.19` → network infrastructure / reserved devices
- `.20–.49` → statically assigned services
- `.50–.199` → DHCP pool
- `.200–.254` → reserved for future use

This approach ensures predictable addressing across all VLANs and reduces ambiguity during troubleshooting.

---

## Design Notes

- infrastructure and administrative networks are intentionally separated
- server networks are defined at both sites for consistency, even if workloads are centralized
- legacy addressing is phased out during migration to maintain a clean final-state design

## Relationship to Other Components

This addressing model supports:

- VLAN segmentation (network boundaries)
- firewall policy enforcement
- VPN routing between sites
- simplified troubleshooting and operations