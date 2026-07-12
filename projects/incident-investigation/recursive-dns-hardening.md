# Recursive DNS Hardening and DNSSEC Repair

## Summary

A containerized Pi-hole and Unbound deployment resolved normal domains, but deeper testing revealed two hidden problems:

- gateway content filtering was intercepting outbound recursive DNS traffic
- DNSSEC validation was disabled in the active Unbound configuration

The resolver path was corrected, DNSSEC validation was restored, and repeatable health checks were added.

## Architecture

Clients -> Pi-hole filtering -> Unbound recursive resolver -> DNS root and authoritative servers

Pi-hole uses only the internal Unbound resolver as its upstream.

## Symptoms

- UDP queries to a root server returned `SERVFAIL`
- responses incorrectly included the recursion-available flag
- UDP and TCP tests behaved differently
- an intentionally invalid DNSSEC domain resolved instead of failing
- both host-level and containerized Unbound services existed
- Unbound reported a socket-buffer warning

## Investigation

A direct root-server response should return:

- status: `NOERROR`
- flags: `qr aa`

Before remediation, the response returned:

- status: `SERVFAIL`
- flags: `qr ra`

The `ra` flag indicated that another recursive resolver or gateway policy was intercepting the query.

The infrastructure VLAN was found to have gateway content filtering and ad blocking enabled. The active Unbound configuration was also not loading the DNS root trust anchor.

## Root Causes

1. Gateway filtering intercepted recursive DNS traffic.
2. DNSSEC trust-anchor validation was disabled.
3. A duplicate host-level resolver created operational ambiguity.
4. Linux socket-buffer limits were below the resolver's requested size.

## Corrective Actions

- removed gateway DNS filtering from resolver infrastructure
- verified direct root-server access over UDP and TCP
- enabled the DNS root trust anchor
- enabled DNSSEC hardening
- refreshed the trust anchor
- disabled the unused host-level resolver
- raised receive and send socket-buffer limits
- added an automated validation script

## Validation Results

- Root UDP: `NOERROR`, `qr aa`
- Root TCP: `NOERROR`, `qr aa`
- Valid DNSSEC: `NOERROR` with authenticated-data flag
- Invalid DNSSEC: `SERVFAIL`

## Lessons Learned

- normal domain resolution does not prove direct recursion
- recursive resolvers require clean UDP and TCP access
- gateway DNS filtering should not apply to resolver infrastructure
- DNSSEC should be tested with both valid and invalid signed domains
- one production resolver path reduces troubleshooting complexity
- automated checks help prevent regression

## Skills Demonstrated

- Pi-hole and Unbound integration
- recursive DNS validation
- DNSSEC troubleshooting
- Docker bridge and macvlan networking
- gateway policy analysis
- Linux service management
- kernel socket-buffer tuning
- root-cause analysis
- infrastructure validation scripting
