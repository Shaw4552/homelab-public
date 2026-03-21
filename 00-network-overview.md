# Enterprise-Style Homelab Network Overview

This document provides a high-level overview of a production-style, segmented, multi-site network designed using enterprise-grade networking and security principles.

It outlines the architectural approach, design goals, and core components that define the environment.

---

## 🎯 Design Goals

* segment networks based on trust level and operational function
* enforce security boundaries using a deny-by-default firewall model
* separate infrastructure, services, and endpoint networks
* support secure remote access and inter-site connectivity
* model real-world network architecture patterns
* maintain clear, production-style documentation

---

## 🌐 Environment Summary

The environment consists of two logical sites:

### Site A – Infrastructure

* centralized service hosting
* containerized application workloads
* DNS and core network services
* storage and internal platforms

### Site B – Access / Endpoint

* user and work devices
* gaming and media systems
* IoT devices
* wireless access networks

Site B connects securely to Site A through VPN and controlled routing policies.

---

## 🧠 Architecture Highlights

* VLAN-based segmentation aligned to trust zones
* consistent IP addressing across multiple sites
* deny-by-default inter-network communication
* controlled inter-VLAN routing via firewall policy
* separation of infrastructure, services, and endpoint networks
* wireless networks mapped directly to security boundaries
* centralized DNS filtering with recursive resolution
* VPN architecture supporting both remote access and site-to-site connectivity

---

## 🔐 Security Model

The network follows a segmented, zero-trust–inspired design:

* all inter-network traffic is denied by default
* communication is explicitly allowed only where required
* infrastructure and administrative access are isolated
* low-trust networks (IoT, guest) are heavily restricted
* lateral movement between segments is minimized

---

## 🛠️ Core Skills Demonstrated

* VLAN architecture and segmentation design
* IP addressing and subnet planning
* firewall policy design and traffic control
* multi-site network architecture
* wireless network segmentation
* VPN design (remote access and site-to-site)
* DNS architecture (filtering and recursive resolution)
* infrastructure documentation aligned with enterprise practices

---

This overview serves as the foundation for the detailed design documents that follow in this repository.