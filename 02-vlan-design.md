# VLAN Design

The network is segmented by trust boundary and operational role.

## VLANs

| VLAN | Purpose |
|------|---------|
| Admin | management endpoints |
| Trusted | personal user devices |
| IoT | low-trust smart devices |
| Guest | internet-only guest access |
| Servers | self-hosted services |
| Work | dedicated work devices |
| Kids | family-managed endpoints |
| Quarantine | isolated devices |
| Gaming | entertainment systems |
| Infrastructure | core network services |

## Security Philosophy

- deny inter-VLAN traffic by default
- allow only required paths
- isolate low-trust device classes
- protect infrastructure and management planes