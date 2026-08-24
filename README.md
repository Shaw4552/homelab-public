# Enterprise-Style Homelab Infrastructure Portfolio

## Overview

This repository documents a multi-site homelab environment designed around enterprise infrastructure, networking, security, monitoring, automation, and operational reliability.

The environment is used as a hands-on platform for building and maintaining practical skills across:

* network engineering
* systems administration
* Linux infrastructure
* virtualization
* containerized services
* monitoring and observability
* DNS architecture
* security segmentation
* troubleshooting
* infrastructure documentation

The lab is intentionally designed to resemble a small enterprise environment rather than a collection of isolated home services.

---

## Architecture

### Current Multi-Site Topology

The current environment spans two sites connected through a site-to-site VPN and includes segmented VLANs, centralized DNS, virtualization, monitoring, storage, and application services.

[View the full multi-site topology](./diagrams/full-multisite-topology.md)

### Historical Diagrams

Earlier architecture diagrams are retained under [`diagrams/archive/`](./diagrams/archive/) to document the evolution of the environment.

---

## Core Infrastructure

The environment includes:

* Proxmox VE virtualization
* Linux virtual machines and LXC containers
* Docker-based application services
* UniFi routing, switching, and wireless infrastructure
* multi-site connectivity
* segmented VLAN architecture
* site-to-site VPN connectivity
* centralized DNS
* internal HTTPS and PKI
* infrastructure monitoring
* operational backup and recovery procedures

---

## Networking and Security

The network follows a trust-based segmentation model.

```text
Admin
↓
Infrastructure
↓
Servers
↓
Trusted / Work
↓
Media Services
↓
Entertainment
↓
Restricted Users
↓
IoT
↓
Guest
↓
Quarantine
```

Inter-VLAN communication is controlled through explicit firewall policy.

The design follows a deny-by-default approach where access between trust zones is permitted only when required.

Key networking concepts demonstrated include:

* VLAN segmentation
* firewall policy design
* inter-VLAN routing
* multi-site architecture
* wireless network segmentation
* VPN architecture
* infrastructure service isolation
* controlled DNS access
* least-privilege access patterns

---

## DNS Infrastructure

DNS services are built around Pi-hole and Unbound.

The architecture demonstrates:

* recursive DNS resolution
* DNS filtering
* centralized policy management
* local DNS records
* multi-site DNS services
* DNSSEC validation
* service redundancy
* infrastructure naming
* troubleshooting and validation workflows

Detailed DNS documentation is available in:

* [`06-dns-architecture.md`](./06-dns-architecture.md)

---

## Virtualization and Systems Administration

The lab uses Proxmox VE as the primary virtualization platform.

Workloads include:

* Linux LXC containers
* Linux virtual machines
* containerized infrastructure services
* monitoring platforms
* application services
* administrative workstations

Operational tasks include:

* service deployment
* storage configuration
* container lifecycle management
* Linux troubleshooting
* network troubleshooting
* backup and recovery
* infrastructure validation

---

## Monitoring and Observability

The environment includes centralized infrastructure monitoring using LibreNMS and SNMPv3.

Monitoring covers infrastructure such as:

* Proxmox hosts
* Linux containers
* UniFi networking devices
* application infrastructure
* DNS systems
* network availability
* service health

The monitoring environment is documented separately in the public project:

[**enterprise-monitoring-observability**](https://github.com/Shaw4552/enterprise-monitoring-observability)

This project demonstrates:

* device onboarding
* SNMPv3 configuration
* infrastructure health monitoring
* alerting design
* service visibility
* operational troubleshooting
* monitoring documentation

---

## Internal HTTPS and PKI

Internal services use HTTPS through a centralized reverse proxy and private certificate authority architecture.

The environment includes experience with:

* Caddy
* reverse proxy configuration
* internal DNS names
* TLS certificates
* private certificate authorities
* certificate trust distribution
* service publishing
* HTTPS troubleshooting

---

## Automation and Change Management

Infrastructure changes are increasingly managed through Git-based workflows.

Practices include:

* Git repositories as infrastructure documentation
* branch-based changes
* pull request workflows
* deployment scripts
* validation scripts
* rollback procedures
* change documentation
* GitHub Actions and CI/CD concepts

This provides a repeatable and auditable approach to infrastructure changes.

---

## Incident Investigation and Troubleshooting

The lab also serves as a platform for documenting real infrastructure incidents.

Case studies include:

### Proxmox Intel I217-LM Network Instability

[`projects/incident-investigation/proxmox-i217lm-network-instability.md`](./projects/incident-investigation/proxmox-i217lm-network-instability.md)

Demonstrates:

* network interface troubleshooting
* Linux diagnostics
* Proxmox host investigation
* root-cause analysis
* remediation documentation

### Recursive DNS Hardening

[`projects/incident-investigation/recursive-dns-hardening.md`](./projects/incident-investigation/recursive-dns-hardening.md)

Demonstrates:

* DNS troubleshooting
* recursive resolver hardening
* DNSSEC validation
* infrastructure validation
* production-style change documentation

---

## Technologies

### Networking

* Ubiquiti UniFi
* VLANs
* firewall policy
* site-to-site VPN
* WireGuard
* OpenWrt

### Systems

* Proxmox VE
* Linux
* LXC
* Docker
* Bash

### Infrastructure Services

* Pi-hole
* Unbound
* Caddy
* internal PKI

### Monitoring

* LibreNMS
* SNMPv3
* service monitoring
* infrastructure alerting

### DevOps and Operations

* Git
* GitHub
* GitHub Actions
* CI/CD workflows
* shell scripting
* change management
* runbooks
* technical documentation

---

## Skills Demonstrated

This environment demonstrates practical experience with:

* infrastructure architecture
* network engineering
* Linux systems administration
* virtualization
* container administration
* network segmentation
* firewall policy
* DNS infrastructure
* VPN architecture
* monitoring and observability
* SNMPv3
* reverse proxy architecture
* PKI and TLS
* troubleshooting
* incident response
* automation
* Git workflows
* technical documentation

---

## Repository Documentation

Detailed infrastructure documentation includes:

* [`00-network-overview.md`](./00-network-overview.md)
* [`01-ip-addressing-plan.md`](./01-ip-addressing-plan.md)
* [`02-vlan-design.md`](./02-vlan-design.md)
* [`03-firewall-policy.md`](./03-firewall-policy.md)
* [`04-wireless-design.md`](./04-wireless-design.md)
* [`05-vpn-architecture.md`](./05-vpn-architecture.md)
* [`06-dns-architecture.md`](./06-dns-architecture.md)
* [`07-network-topology.md`](./07-network-topology.md)
* [`architecture.md`](./architecture.md)

---

## Current Focus

The environment continues to evolve in the following areas:

* expanded monitoring and observability
* infrastructure automation
* centralized logging
* backup validation
* service resiliency
* configuration management
* improved operational documentation

---

## Purpose

The purpose of this project is to demonstrate hands-on infrastructure engineering experience through a real, actively maintained environment.

Rather than documenting theoretical designs alone, the repository focuses on systems that are deployed, operated, monitored, troubleshot, improved, and maintained over time.

This provides practical experience relevant to roles including:

* Systems Administrator
* Infrastructure Engineer
* Network Engineer
* NOC Analyst
* Technical Support Engineer
* IT Operations Engineer

---

## Public Repository Notice

This repository contains sanitized infrastructure documentation.

Public IP addresses, credentials, secrets, tokens, private keys, internal identifiers, and sensitive operational configuration are intentionally excluded or modified before publication.

Network names, identifiers, and service descriptions may be generalized for public documentation while preserving the implemented security boundaries, architecture, and policy relationships.