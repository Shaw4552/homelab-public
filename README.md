# Enterprise-Style Homelab Network

This project demonstrates the design and implementation of an enterprise-style network architecture within a homelab environment.

It focuses on segmentation, security boundaries, service placement, and multi-site connectivity using real-world infrastructure design principles.

---

## Overview

The environment simulates a small-scale enterprise network with:

- multi-site architecture (primary infrastructure + remote access site)
- VLAN-based segmentation aligned to trust zones
- deny-by-default firewall enforcement
- centralized infrastructure and service hosting
- secure remote access and site-to-site connectivity
- structured, production-style documentation

---

## Architecture Highlights

- VLAN segmentation based on trust boundaries
- Consistent IP addressing across multiple sites
- Controlled inter-VLAN routing via explicit firewall rules
- Separation of infrastructure, services, and endpoint networks
- Wireless networks mapped to security zones
- Centralized DNS filtering with recursive resolution
- VPN architecture supporting both remote access and site connectivity

---

## Network Segmentation Model

The network is designed using a hierarchical trust model:
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

Each segment is isolated by default, with communication explicitly allowed based on operational requirements.

---

## Skills Demonstrated

This project reflects hands-on experience in:

- network segmentation and VLAN design
- firewall policy and traffic control
- multi-site network architecture
- secure remote access (VPN design)
- DNS architecture (filtering + recursive resolution)
- infrastructure planning and service placement
- technical documentation and system design

---

## Purpose

This repository is part of a broader effort to develop practical skills in network engineering and infrastructure design.

It demonstrates the ability to:

- design secure, segmented networks
- think in terms of systems and architecture
- implement real-world networking concepts in a working environment
- document infrastructure in a clear, structured, and professional manner

## Future Enhancements

- infrastructure automation (IaC)
- monitoring and observability stack
- high-availability DNS design
- advanced routing and policy-based traffic control