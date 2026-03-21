# Wireless Network Design

## Purpose

This document describes the wireless network design and how it integrates with the overall segmentation and security model.

Wireless networks are mapped directly to VLANs to ensure consistent policy enforcement across both wired and wireless clients.

---

## Design Objectives

- align wireless networks with trust zones
- maintain consistent segmentation across access methods
- simplify device placement and onboarding
- balance performance and compatibility
- enforce security boundaries through network design

---

## SSID Segmentation Model

Each SSID maps directly to a VLAN and corresponding security zone.

| SSID Role | VLAN | Purpose                      |
|----------|------|------------------------------|
| Trusted  | 20   | primary user devices         |
| Work     | 60   | work-specific endpoints      |
| Kids     | 70   | restricted user devices      |
| Gaming   | 90   | entertainment systems        |
| IoT      | 30   | low-trust smart devices      |

This ensures wireless clients follow the same segmentation and firewall policies as wired devices.

---

## Band and Radio Strategy

Wireless bands are assigned based on device capability and operational requirements.

### High-Trust / User Networks
- 2.4 GHz, 5 GHz, 6 GHz (where available)
- optimized for performance and flexibility

### IoT Networks
- 2.4 GHz only
- maximizes compatibility with embedded and legacy devices

---

## RF Design Considerations

- **2.4 GHz** → maximum compatibility, lower throughput
- **5 GHz** → improved performance, reduced interference
- **6 GHz** → highest performance for modern devices

Higher-trust networks are allowed to use higher-frequency bands, while IoT networks prioritize compatibility.

---

## Security Model

- WPA3 is preferred where supported
- WPA2/WPA3 mixed mode used for compatibility
- segmentation and firewall policy provide primary isolation
- wireless encryption is treated as a secondary security layer

---

## Network Isolation

Wireless isolation is enforced through:

- VLAN segmentation
- firewall policy
- controlled inter-network communication

Low-trust wireless networks (IoT, guest) are restricted from accessing internal resources.

---

## Roaming and Coverage

- SSIDs are broadcast consistently across access points
- clients can roam between coverage areas without reconnecting
- network segmentation remains consistent regardless of location

---

## Design Considerations

- SSID naming aligns to function and trust level, not location
- wireless design mirrors wired network segmentation
- consistent VLAN mapping simplifies troubleshooting and policy enforcement