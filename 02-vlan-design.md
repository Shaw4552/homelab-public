# Network Segmentation and Topology

This document defines the network segmentation model and logical topology used across the environment.

The segmentation strategy forms the foundation for firewall policy, access control, and traffic flow between infrastructure, services, and endpoint networks.

---

## 🎯 Segmentation Strategy

The network is segmented using VLANs aligned to trust level and operational role.

Design objectives:

* isolate systems based on trust level
* enforce security boundaries between network segments
* minimize lateral (east-west) movement
* provide controlled access to infrastructure services
* maintain consistent segmentation standards across multiple sites

---

## 🧠 VLAN Design

Each VLAN represents a logical network segment associated with an operational role and security posture.

| VLAN | Name                  | Purpose                                      |
| ---- | --------------------- | -------------------------------------------- |
| 10   | Admin                 | privileged management and administration    |
| 15   | Recovery / Provisioning | device recovery and provisioning workflows |
| 20   | Trusted               | trusted user endpoints                       |
| 30   | IoT                   | low-trust smart and embedded devices         |
| 40   | Guest                 | isolated internet-only access                |
| 50   | Servers               | application and service hosting              |
| 60   | Work                  | work-specific endpoints                      |
| 70   | Restricted Users      | restricted user and endpoint access          |
| 75   | Media Services        | isolated media-related application services  |
| 80   | Quarantine            | isolated or untrusted devices                |
| 90   | Entertainment         | entertainment and media-consumption devices  |
| 99   | Infrastructure        | core network and control-plane services      |

---

## 🌐 Logical Topology

The environment consists of two sites connected through secure inter-site connectivity:

* **Site A** serves as the primary infrastructure location and hosts core services
* **Site B** serves as the primary compute and lab environment

A common VLAN numbering and naming convention is used where applicable across sites to improve operational consistency, simplify troubleshooting, and support predictable policy design.

Not every VLAN is necessarily instantiated at every site; network segments are deployed based on operational requirements.

### High-Level Flow

Endpoints → Access VLAN → Site Router → (VPN if required) → Services / Infrastructure

---

## 🔄 Traffic Flow Model

Traffic between VLANs is tightly controlled and enforced through firewall policy.

### Default Behavior

* all inter-VLAN traffic is denied by default
* communication is explicitly allowed based on operational requirements
* lower-trust networks cannot initiate connections to higher-trust networks

---

### Example Flows

**Restricted User Access to Infrastructure Services**

Restricted Users VLAN → Firewall → Approved Infrastructure Service / Port

Access from restricted endpoints to infrastructure services is granted only when operationally required and is limited to explicitly defined destinations and services.

**User Access to Services**

Trusted VLAN → Firewall → Servers VLAN

**Administrative Access**

Admin VLAN → Infrastructure VLAN → Management Interfaces

**IoT Devices**

IoT VLAN → Restricted Access → Internet / Required Services

---

## 🔐 Security Model

The segmentation model follows a zero-trust–inspired approach:

* no implicit trust between network segments
* access is granted only through explicit policy
* infrastructure and management planes are isolated
* sensitive services are protected behind controlled access paths

---

## 🔧 Design Considerations

* infrastructure and services are separated from user networks
* VLAN IDs align with subnet structure for operational clarity
* VLAN numbering and naming conventions are standardized across sites where applicable to simplify troubleshooting and policy management
* server networks exist at each site for architectural consistency

---

## 🔗 Relationship to Other Components

This segmentation model integrates with:

* firewall policy (controls inter-VLAN traffic)
* IP addressing plan (maps VLANs to subnets)
* VPN architecture (connects sites securely)
* DNS architecture (provides centralized resolution services)

---

This segmentation model establishes the security boundaries and traffic control framework for the entire network architecture.
