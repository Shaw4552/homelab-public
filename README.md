# Enterprise-Style Homelab Network

Designed and implemented a multi-site, segmented network using enterprise-grade security and infrastructure design principles.

This project demonstrates the design and implementation of a production-style, security-focused network architecture within a homelab environment.

It focuses on segmentation, security boundaries, service placement, and multi-site connectivity using real-world infrastructure design principles.

---

## 🚀 Overview

The environment simulates a small-scale enterprise network with:

* multi-site architecture (primary infrastructure + remote access site)
* VLAN-based segmentation aligned to trust zones
* deny-by-default firewall enforcement
* centralized infrastructure and service hosting
* secure remote access and site-to-site connectivity
* production-style documentation and design standards

---

## 🧠 Architecture Highlights

* VLAN segmentation based on trust boundaries (not device type)
* Consistent IP addressing scheme across multiple sites
* Explicit firewall-controlled inter-VLAN communication
* Separation of infrastructure, services, and endpoint networks
* Wireless networks mapped directly to security zones
* Centralized DNS filtering with recursive resolution (Pi-hole + Unbound)
* VPN architecture supporting both remote access and site-to-site connectivity

---

## 🧩 Technologies Used

* Ubiquiti UniFi (UDM Pro, APs)
* Pi-hole + Unbound (DNS filtering and recursion)
* WireGuard (VPN)
* DD-WRT / OpenWRT (edge networking)
* Docker (self-hosted services)

---

## 🔐 Network Segmentation Model

The network follows a hierarchical trust model:

```
Admin
↓
Infrastructure
↓
Servers
↓
Trusted / Work
↓
Gaming
↓
Kids
↓
IoT
↓
Guest
↓
Quarantine
```

All segments are isolated by default.
Access between zones is explicitly permitted through firewall policy.

---

## 🛠️ Skills Demonstrated

This project reflects hands-on experience in:

* network segmentation and VLAN design
* firewall policy design (deny-by-default model)
* multi-site network architecture
* VPN design (remote access and site-to-site)
* DNS architecture (filtering + recursive resolution)
* infrastructure planning and service placement
* technical documentation aligned with enterprise practices

---

## 🎯 Purpose

This repository is part of a broader effort to develop practical, real-world skills in network engineering and infrastructure design.

It demonstrates the ability to:

* design secure, segmented network environments
* apply zero-trust-inspired principles
* implement production-style infrastructure patterns
* document systems clearly and professionally

---

## 📈 Future Enhancements

* infrastructure automation (IaC: Terraform / Ansible)
* monitoring and observability (Prometheus / Grafana)
* high-availability DNS design
* advanced routing and policy-based traffic control
* containerized service expansion

---

## 📚 Documentation

Detailed design documentation is organized by component:

* network overview
* IP addressing plan
* VLAN segmentation
* firewall policy
* wireless design
* VPN architecture
* DNS architecture
* network topology

---

## 📌 Notes

This is a **sanitized environment**.
IP ranges, hostnames, and identifiers have been modified for public sharing.
