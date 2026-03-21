# DNS Architecture

This document defines the DNS architecture used across the environment.

DNS services are centralized to provide consistent name resolution, policy enforcement, and visibility across all network segments.

The design leverages a layered approach combining DNS filtering and recursive resolution to improve security, privacy, and operational control.

---

## 🎯 Design Objectives

* provide reliable and consistent DNS resolution across all networks
* enforce DNS-based filtering and policy control
* improve privacy through local recursive resolution
* reduce dependency on external public resolvers
* centralize visibility and troubleshooting

---

## 🧠 Architecture Overview

The DNS architecture follows a two-layer model:

1. **DNS Filtering Layer** (Pi-hole)
2. **Recursive Resolver Layer** (Unbound)

### Logical Flow

Client → DNS Filter (Pi-hole) → Recursive Resolver (Unbound) → Internet

This design enables filtering at the DNS layer while maintaining full control over query resolution.

---

## 🔧 Core Components

### DNS Filtering Layer (Pi-hole)

Provides:

* DNS-based ad and tracker blocking
* policy enforcement through blocklists
* visibility into client DNS activity
* local DNS overrides for internal services

---

### Recursive Resolver Layer (Unbound)

Provides:

* direct recursive resolution (no upstream public DNS)
* improved privacy by eliminating third-party resolvers
* local caching for performance optimization
* validation of DNS responses

---

## 🔐 Security Model

* all client DNS traffic is directed to the centralized DNS service
* external DNS usage can be restricted or blocked
* filtering is enforced at the DNS layer before traffic leaves the network
* DNS logs provide visibility into client behavior

---

## 🌐 Network Integration

* DNS services reside within the infrastructure network
* all VLANs are configured to use centralized DNS
* firewall rules enforce DNS usage through approved resolvers
* VPN clients inherit the same DNS configuration

---

## 📈 Benefits

* centralized policy enforcement
* improved visibility into network activity
* enhanced privacy through recursive resolution
* reduced external dependency on public DNS providers
* improved performance through local caching
* simplified troubleshooting and operational consistency

---

## 🔧 Design Considerations

* DNS is treated as a core infrastructure service
* filtering and resolution are separated for flexibility and control
* centralized DNS simplifies policy enforcement across all network segments
* DNS architecture aligns with overall segmentation and security model

---

This DNS architecture provides a secure, private, and centrally managed foundation for name resolution across the entire network environment.
