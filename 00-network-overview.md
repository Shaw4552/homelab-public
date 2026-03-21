# Enterprise-Style Homelab Network Overview

This project demonstrates the design and implementation of a segmented, multi-site network architecture using enterprise networking principles.

It focuses on network segmentation, security boundaries, service placement, and controlled connectivity between infrastructure and endpoint environments.

---

## Design Goals

- segment networks based on trust level and operational function
- enforce security boundaries using a deny-by-default firewall model
- separate infrastructure, services, and user networks
- support secure remote access and inter-site connectivity
- model real-world network architecture patterns
- maintain clear, production-style documentation

---

## Environment Summary

The environment consists of two logical sites:

### Site A – Infrastructure

- centralized service hosting
- application and container workloads
- DNS and core network services
- storage and internal platforms

### Site B – Access / Endpoint

- user and work devices
- gaming and media systems
- IoT devices
- wireless access networks

Site B connects securely to Site A through VPN and controlled routing policies.

---

## Architecture Highlights

- VLAN-based segmentation aligned to trust zones
- consistent IP addressing across multiple sites
- deny-by-default inter-network communication
- controlled inter-VLAN routing via firewall policy
- separation of infrastructure, services, and endpoints
- wireless networks mapped to security boundaries
- centralized DNS filtering with recursive resolution
- VPN architecture supporting remote access and site connectivity

---

## Security Model

The network follows a segmented, zero-trust–inspired design:

- all inter-network traffic is denied by default
- communication is explicitly allowed only where required
- infrastructure and administrative access are isolated
- low-trust networks (IoT, guest) are restricted
- lateral movement between segments is minimized

---

## Core Skills Demonstrated

- VLAN architecture and segmentation design
- IP addressing and subnet planning
- firewall policy design and traffic control
- multi-site network architecture
- wireless network segmentation
- VPN design (remote access and site-to-site)
- DNS architecture (filtering and recursive resolution)
- infrastructure documentation and system design