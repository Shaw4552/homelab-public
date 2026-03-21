# Wireless Network Design

This document defines the wireless network design and its integration with the overall segmentation and security model.

Wireless networks extend the same VLAN-based segmentation used in the wired environment to ensure consistent policy enforcement across all client access methods.

---

## 🎯 Design Objectives

* align wireless networks with trust zones
* maintain consistent segmentation across access methods
* simplify device placement and onboarding
* balance performance and compatibility
* enforce security boundaries through network design

---

## 🧠 SSID Segmentation Model

Each SSID maps directly to a VLAN and corresponding security zone.

| SSID Role | VLAN | Purpose                 |
| --------- | ---- | ----------------------- |
| Trusted   | 20   | primary user devices    |
| Work      | 60   | work-specific endpoints |
| Kids      | 70   | restricted user devices |
| Gaming    | 90   | entertainment systems   |
| IoT       | 30   | low-trust smart devices |

This mapping ensures wireless clients follow the same segmentation and firewall policies as wired devices.

---

## 📡 Band and Radio Strategy

Wireless bands are assigned based on device capability and operational requirements.

### High-Trust / User Networks

* 2.4 GHz, 5 GHz, and 6 GHz (where available)
* optimized for performance, roaming, and flexibility

### IoT Networks

* 2.4 GHz only
* maximizes compatibility with embedded and legacy devices

---

## 📶 RF Design Considerations

* **2.4 GHz** → maximum compatibility, lower throughput
* **5 GHz** → improved performance, reduced interference
* **6 GHz** → highest performance for modern devices

Higher-trust networks leverage higher-frequency bands for performance, while IoT networks prioritize compatibility and stability.

---

## 🔐 Security Model

* WPA3 is preferred where supported
* WPA2/WPA3 mixed mode is used for compatibility
* segmentation and firewall policy provide primary isolation
* wireless encryption is treated as a secondary security layer

---

## 🔒 Network Isolation

Wireless isolation is enforced through:

* VLAN segmentation
* firewall policy
* controlled inter-network communication

Low-trust wireless networks (IoT, guest) are restricted from accessing internal resources.

---

## 🔄 Roaming and Coverage

* SSIDs are broadcast consistently across access points
* clients roam between coverage areas without reconfiguration
* segmentation and policy enforcement remain consistent regardless of location

---

## 🔧 Design Considerations

* SSID naming aligns to function and trust level, not physical location
* wireless design mirrors wired network segmentation
* consistent VLAN mapping simplifies troubleshooting and policy enforcement

---

This wireless design ensures consistent security, performance, and policy enforcement across all client access methods within the environment.
