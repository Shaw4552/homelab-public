# Network Segmentation and Topology

## Purpose

This document describes the network segmentation model and logical topology used across the environment.

It defines how VLANs are used to enforce trust boundaries, isolate systems, and control traffic flow between infrastructure, services, and endpoint networks.

---

## Segmentation Strategy

The network is segmented using VLANs aligned to trust level and operational role.

Design goals:

- isolate device classes based on trust level
- enforce security boundaries between network segments
- minimize lateral (east-west) movement
- provide controlled access to infrastructure services
- maintain consistent segmentation across multiple sites

---

## VLAN Design

Each VLAN represents a logical security zone.

| VLAN | Name           | Purpose                          |
|------|----------------|----------------------------------|
| 10   | Admin          | management and administrative access |
| 20   | Trusted        | primary user devices             |
| 30   | IoT            | low-trust smart devices          |
| 40   | Guest          | internet-only access             |
| 50   | Servers        | application and service hosting  |
| 60   | Work           | work-specific devices            |
| 70   | Kids           | restricted user devices          |
| 80   | Quarantine     | isolated or untrusted devices    |
| 90   | Gaming         | entertainment systems            |
| 99   | Infrastructure | core network and control plane   |

---

## Logical Topology

The environment consists of two sites with consistent segmentation:

- **Site A** hosts infrastructure and services
- **Site B** provides endpoint access

Both sites implement the same VLAN structure to maintain consistency and simplify routing and policy enforcement.

High-level flow:

Endpoints → Access VLAN → Site Router → (VPN if required) → Services / Infrastructure

---

## Traffic Flow Model

Traffic between VLANs is tightly controlled.

### Default Behavior

- all inter-VLAN traffic is denied by default
- communication is explicitly allowed based on need
- lower-trust networks cannot initiate connections to higher-trust networks

---

### Example Flows

**User Access to Services**

Trusted VLAN → Firewall → Servers VLAN


**Administrative Access**

Admin VLAN → Infrastructure VLAN → Management Interfaces


**IoT Devices**


IoT VLAN → Restricted Access → Internet / Required Services


---

## Security Model

The segmentation model follows a zero-trust–inspired approach:

- no implicit trust between network segments
- access is granted only through explicit policy
- infrastructure and management planes are isolated
- sensitive services are protected behind controlled access paths

---

## Design Considerations

- infrastructure and services are separated from user networks
- VLAN IDs align with subnet structure for operational clarity
- segmentation is consistent across sites to simplify troubleshooting
- server networks are defined at each site for architectural consistency

---

## Relationship to Other Components

This segmentation model works in conjunction with:

- firewall policy (controls traffic between VLANs)
- IP addressing plan (maps VLANs to subnets)
- VPN architecture (connects sites securely)
- DNS architecture (provides centralized resolution services)
