# Wireless Design

Wireless networks are mapped directly to VLANs to keep policy consistent across client access methods.

## Example SSID Roles

- Trusted
- Work
- Kids
- Gaming
- IoT

## Design Notes

- IoT is typically restricted to 2.4 GHz for compatibility
- higher-trust user networks can use 5 GHz / 6 GHz for performance
- WPA3 is preferred where practical
- segmentation is enforced primarily through VLAN and firewall policy