# Network Topology

## Purpose

This document describes the logical topology of the network, including site layout, core components, and traffic flow between systems.

It provides a high-level view of how segmentation, routing, and connectivity are implemented across the environment.

---

## High-Level Architecture

The environment consists of two sites connected through secure VPN connectivity:

- **Site A** – primary infrastructure and service hosting
- **Site B** – endpoint access and user connectivity

High-level relationship:

        Internet
            │
    ┌──────────────┐
    │   Site A     │
    │  Core Router │
    └──────┬───────┘
           │
    Infrastructure
           │
    ┌──────┴──────┐
    │             │
 Servers       Services
    │
    │
 VPN Tunnel
    │
    ▼
    ┌──────────────┐
    │   Site B     │
    │ Access Router│
    └──────┬───────┘
           │
     Client Networks
  (Trusted / Work / IoT /
   Guest / Gaming / Kids)

---

## Site Roles

### Site A – Infrastructure

Site A hosts core services and shared infrastructure.

Typical components:

- application and service hosting
- DNS and core network services
- storage systems
- automation platforms

This site acts as the central service layer for the environment.

---

### Site B – Access / Endpoint

Site B provides connectivity for user devices.

Typical components:

- user and work devices
- gaming and media systems
- IoT devices
- wireless access networks

This site connects to Site A to consume centralized services.

---

## Segmentation Model

Both sites use a consistent VLAN-based segmentation model.

Each network segment represents a trust boundary:

- infrastructure and management networks are isolated
- services are separated from user devices
- low-trust networks (IoT, guest) are restricted

This ensures predictable traffic flow and consistent policy enforcement.

---

## Traffic Flow Overview

Traffic follows controlled paths based on network role.

### User Access to Services


Client Device → Access VLAN → Firewall → Services VLAN


---

### Administrative Access


Admin Device → Admin VLAN → Infrastructure VLAN → Management Systems


---

### IoT Devices


IoT Device → IoT VLAN → Restricted Access → Internet / Approved Services


---

### Cross-Site Traffic


Site B → VPN Tunnel → Site A → Services / Infrastructure


---

## Inter-Site Connectivity

A site-to-site VPN connects both locations.

This enables:

- secure access to centrally hosted services
- consistent segmentation across sites
- no direct exposure of internal services to the internet

Only approved networks and services are routed across the tunnel.

---

## Design Principles

- clear separation of infrastructure, services, and endpoints
- consistent segmentation across sites
- deny-by-default traffic control enforced by firewall policy
- centralized service hosting with controlled access paths
- predictable and auditable traffic flow

---

## Relationship to Other Components

This topology works in conjunction with:

- VLAN design (defines segmentation)
- firewall policy (controls traffic flow)
- IP addressing plan (maps subnets to VLANs)
- VPN architecture (connects sites securely)
- DNS architecture (provides centralized resolution)