# VPN Architecture

This document defines the VPN architecture used to provide secure remote access, site-to-site connectivity, and controlled outbound routing.

VPN access is treated as an extension of internal network segmentation, ensuring consistent security boundaries across all access methods.

---

## 🎯 Design Objectives

* enable secure remote access to internal resources
* connect multiple sites without exposing services publicly
* maintain separation between administrative and user access
* support controlled outbound routing for selected traffic
* ensure VPN access follows existing network segmentation

---

## 🧠 VPN Models

The environment uses multiple VPN types for different operational purposes:

| VPN Type             | Purpose                                      |
| -------------------- | -------------------------------------------- |
| Remote Access VPN    | secure access for administrators and users   |
| Site-to-Site VPN     | connect remote site to core infrastructure   |
| Outbound VPN Clients | route selected traffic through external VPNs |
| Policy-Based Routing | direct traffic based on source network       |

---

## 🔐 Remote Access Design

Remote access is segmented by role to maintain security boundaries.

### Administrative Access

* access to infrastructure and management systems
* ability to manage network devices and services
* restricted to trusted administrative endpoints

### User Access

* access to approved internal services
* general network usage through the environment
* restricted from infrastructure and management networks

This separation enforces least-privilege access for remote clients.

---

## 🌐 Site-to-Site Connectivity

A site-to-site VPN connects remote networks to the primary infrastructure site.

This enables:

* access to centrally hosted services
* consistent network experience across sites
* secure communication without exposing internal systems

Only approved networks and services are routed between sites.

---

## 🔄 Outbound VPN Routing

Selected traffic is routed through external VPN providers when required.

Typical use cases include:

* privacy-focused routing
* isolation of specific network segments
* testing and validation scenarios

Outbound VPN usage does not bypass internal firewall or segmentation policy.

---

## ⚙️ Policy-Based Routing

Traffic is directed through different egress paths based on source network.

Examples:

* work-related traffic routed through a designated VPN path
* isolated networks using separate outbound routes
* selected user traffic routed through specific providers

This enables flexible routing without impacting the broader environment.

---

## 🔐 Security Model

* VPN access is restricted based on role and network segment
* administrative access is tightly controlled
* user access is limited to required services
* all VPN traffic is subject to firewall policy
* segmentation is preserved across VPN connections

---

## 🔧 Design Considerations

* VPN architecture complements, not replaces, network segmentation
* remote access follows the same trust model as internal networks
* site-to-site connectivity is intentionally scoped
* outbound routing is controlled and limited to specific use cases

---

This VPN architecture extends secure access to the environment while maintaining consistent segmentation, policy enforcement, and security boundaries.
