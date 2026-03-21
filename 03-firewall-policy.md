# Firewall Policy and Traffic Control

This document defines the firewall policy model used to control traffic between network segments.

The firewall serves as the primary enforcement layer for segmentation, access control, and trust boundaries across the environment.

The design follows least-privilege and zero-trust principles to minimize lateral movement and protect critical infrastructure.

---

## 🔐 Security Model

The firewall operates with a **deny-by-default posture**:

* all inter-VLAN traffic is denied unless explicitly allowed
* access is restricted by source, destination, and service
* lower-trust networks are isolated from higher-trust systems
* infrastructure and management access are tightly controlled

---

## 🔄 Traffic Direction Model

The policy distinguishes between two primary traffic types:

### North-South Traffic

* traffic between internal networks and external networks (internet)
* generally allowed based on network role
* may be restricted for specific segments (e.g., IoT, Guest)

### East-West Traffic

* traffic between internal VLANs
* denied by default
* allowed only for required services or administrative access

---

## 🎯 Policy Objectives

* protect infrastructure and management systems
* reduce risk of lateral movement
* isolate low-trust and unmanaged devices
* enable required services without overexposing internal systems
* maintain clear and auditable rule intent

---

## ✅ Typical Allowed Flows

| Source      | Destination    | Purpose                         |
| ----------- | -------------- | ------------------------------- |
| Admin       | Infrastructure | device and platform management  |
| Admin       | Servers        | service administration          |
| Trusted     | Servers        | access to internal applications |
| Trusted     | Infrastructure | limited services (e.g., DNS)    |
| Work        | Servers        | access to approved services     |
| Automation  | IoT            | device control and monitoring   |
| Remote Site | Core Services  | cross-site service access       |

---

## 🚫 Typical Blocked Flows

| Source     | Destination    | Purpose                      |
| ---------- | -------------- | ---------------------------- |
| IoT        | Trusted        | prevent lateral movement     |
| IoT        | Admin          | protect management systems   |
| Guest      | Internal       | enforce internet-only access |
| Gaming     | Infrastructure | protect core services        |
| Kids       | Admin/Infra    | restrict privileged access   |
| Quarantine | Internal       | full isolation               |

---

## 🛡️ Management Plane Protection

Access to infrastructure and management interfaces is restricted to:

* administrative VLANs
* explicitly approved access paths
* secure remote-access connections (VPN)

Management interfaces are never exposed to low-trust networks.

---

## 🔧 Service Exposure Principles

Internal services are exposed using **minimal access rules**:

* specific source networks
* specific destination systems
* restricted destination ports

Broad or unrestricted access (e.g., any-to-any rules) is avoided.

---

## 🧠 Rule Design Principles

Firewall rules are designed with:

* narrow scope (least privilege)
* explicit source and destination definitions
* restricted port usage
* clear and documented intent

This improves both security and operational clarity.

---

## ⚙️ Rule Evaluation Model

Firewall rules are processed in the following order:

1. established and related traffic
2. explicit allow rules
3. explicit deny rules
4. default deny

This ensures only intended traffic is permitted.

---

## 🔍 Operational Considerations

Firewall policies are regularly reviewed to:

* remove unused or stale rules
* tighten overly broad access
* align with current network design
* support ongoing segmentation and infrastructure changes

---

This firewall model enforces the network’s security boundaries and ensures controlled, auditable communication between all segments.
