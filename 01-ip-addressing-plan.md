# IP Addressing Strategy

The environment uses RFC1918 addressing with predictable per-site subnet allocation.

## Design Pattern

- Site A uses `10.10.x.0/24`
- Site B uses `10.20.x.0/24`

The third octet maps to the VLAN ID where practical.

## Example VLAN Allocation

| VLAN ID | Name | Example Subnet |
|--------|------|----------------|
| 10 | Admin | 10.x.10.0/24 |
| 20 | Trusted | 10.x.20.0/24 |
| 30 | IoT | 10.x.30.0/24 |
| 40 | Guest | 10.x.40.0/24 |
| 50 | Servers | 10.x.50.0/24 |
| 60 | Work | 10.x.60.0/24 |
| 70 | Kids | 10.x.70.0/24 |
| 80 | Quarantine | 10.x.80.0/24 |
| 90 | Gaming | 10.x.90.0/24 |
| 99 | Infrastructure | 10.x.99.0/24 |