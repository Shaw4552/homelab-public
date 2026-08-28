# Linux Mint Wi-Fi Recovery: Broadcom, DKMS, and Kernel Compatibility

## Executive Summary

This case study documents the diagnosis and recovery of a persistent Wi-Fi failure on an Intel-based MacBook Pro running Linux Mint.

The laptop could detect nearby wireless networks with strong signal strength, but NetworkManager repeatedly failed to establish a connection. The visible error claimed that the wireless network could not be found, even though the SSID appeared consistently during scans.

Investigation showed that the real problem was a compatibility conflict involving:

* A Broadcom BCM4331 wireless adapter
* The open-source `b43` driver
* The proprietary Broadcom STA `wl` driver
* DKMS module compilation
* Linux kernel 7.0
* Dual-band Wi-Fi operation

The system was ultimately stabilized on kernel `6.14.0-37-generic`, where the Broadcom STA driver compiled successfully and restored reliable 5 GHz connectivity.

---

## Environment

| Component               | Configuration                   |
| ----------------------- | ------------------------------- |
| Computer                | Intel-based Apple MacBook Pro   |
| Operating system        | Linux Mint 22.3 Cinnamon        |
| Wireless adapter        | Broadcom BCM4331 802.11a/b/g/n  |
| Initial kernel          | Linux 7.0                       |
| Working kernel          | Linux 6.14.0-37                 |
| Initial driver          | `b43` through `bcma`            |
| Final driver            | Broadcom STA `wl`               |
| Network management      | NetworkManager                  |
| Wireless infrastructure | UniFi                           |
| Target connection       | Sanitized trusted Wi-Fi network |

Private SSIDs, MAC addresses, IP addresses, usernames, and other environment-specific identifiers have been removed or generalized.

---

## Initial Symptoms

The laptop displayed several related symptoms:

* Nearby Wi-Fi networks appeared with strong signal strength.
* The target SSID was visible during scans.
* NetworkManager remained stuck in `connecting (configuring)`.
* The wireless interface did not receive an IPv4 address.
* No Wi-Fi default route was installed.
* Internet tests returned `Network is unreachable`.
* NetworkManager reported `ssid-not-found`.
* Restarting NetworkManager did not resolve the issue.

Ethernet remained operational and was preserved as a safe management connection during troubleshooting.

---

## Troubleshooting Method

The investigation followed a layered approach:

1. Interface state
2. IP addressing
3. Routing
4. Internet reachability
5. DNS resolution
6. Wi-Fi authentication
7. Kernel driver behavior
8. Hardware capabilities
9. DKMS and kernel compatibility
10. Reboot persistence

This prevented unrelated layers, such as DNS or DHCP, from being changed before wireless association was proven to work.

---

## Phase 1: Establish the Failure Boundary

### Check interface state

```bash
nmcli device status
ip -br address
ip route
```

The Ethernet interface was connected, while the wireless interface remained disconnected or stuck in configuration.

### Separate internet connectivity from DNS

```bash
ping -c 3 1.1.1.1
ping -c 3 google.com
```

When Ethernet was available, both tests passed. When Ethernet was removed, the laptop reported:

```text
Network is unreachable
```

This confirmed that the Wi-Fi interface had no usable route.

---

## Phase 2: Inspect NetworkManager

NetworkManager was restarted using the correct service name:

```bash
sudo systemctl restart NetworkManager
```

Recent events were reviewed:

```bash
journalctl -u NetworkManager \
  --since "-3 minutes" \
  --no-pager
```

The interface repeatedly moved through:

```text
scanning -> authenticating -> disconnected
```

NetworkManager eventually reported:

```text
ssid-not-found
```

Because the SSID remained visible during scans, this was treated as a secondary timeout rather than the root cause.

---

## Phase 3: Inspect Kernel-Level Wi-Fi Events

The wireless-related kernel events were reviewed:

```bash
sudo dmesg |
grep -Ei 'b43|bcma|wl|firmware|deauth|auth'
```

The decisive message was:

```text
denied authentication (status 37)
```

This proved that the failure occurred during 802.11 association.

The failure happened before:

* WPA key exchange completion
* DHCP address assignment
* Route installation
* DNS resolution

This eliminated DHCP and DNS as root causes.

---

## Phase 4: Identify the Wireless Hardware and Driver

The adapter and active kernel driver were identified:

```bash
lspci -nnk |
sed -n '/Network controller/,+4p'
```

Result:

```text
Broadcom BCM4331 802.11a/b/g/n
```

Loaded Broadcom-related modules were checked:

```bash
lsmod |
grep -E '^(wl|b43|bcma|brcmsmac|ssb)'
```

The system was initially using:

```text
b43
bcma
ssb
```

The adapter’s exposed wireless capabilities were then inspected:

```bash
iw phy phy0 info
```

Although the BCM4331 is dual-band hardware, the active Linux driver exposed only:

```text
Band 1
```

The 5 GHz band was missing.

This explained why the laptop could see only the 2.4 GHz version of the wireless network.

---

## Phase 5: Identify the Recommended Driver

Linux Mint’s driver recommendation was checked:

```bash
ubuntu-drivers devices
```

The recommended package was:

```text
broadcom-sta-dkms
```

This package provides Broadcom’s proprietary `wl` driver.

The required kernel headers and driver package were installed:

```bash
sudo apt update

sudo apt install \
  linux-headers-$(uname -r) \
  broadcom-sta-dkms
```

The installation did not complete successfully on kernel 7.0.

---

## Phase 6: Diagnose the DKMS Failure

The package state showed that the driver was only partially configured:

```bash
dpkg -l broadcom-sta-dkms
dkms status
```

The package appeared as:

```text
iF broadcom-sta-dkms
```

The `F` status indicated a failed configuration.

The DKMS build log was reviewed:

```bash
sudo tail -100 \
  /var/lib/dkms/broadcom-sta/6.30.223.271/build/make.log
```

The critical build failure was:

```text
objtool: aes_cbc_encrypt_pad: unannotated intra-function call
```

The installed Broadcom STA source could not compile against the Linux 7.0 kernel.

---

## Phase 7: Establish a Safe Rollback Kernel

Installed kernels were inventoried:

```bash
dpkg -l 'linux-image-[0-9]*-generic' |
awk '$1=="ii" {print $2, $3}'
```

Kernel `6.14.0-37-generic` and its matching headers were already installed.

Their presence was verified:

```bash
dpkg -l |
grep -E 'linux-(image|headers)-6\.14\.0-37'
```

This provided a recoverable path without reinstalling Linux.

---

## Phase 8: Build the Driver for Kernel 6.14

The Broadcom STA module was built specifically for the compatible kernel:

```bash
sudo dkms build \
  broadcom-sta/6.30.223.271 \
  -k 6.14.0-37-generic
```

The resulting module was installed:

```bash
sudo dkms install \
  broadcom-sta/6.30.223.271 \
  -k 6.14.0-37-generic
```

DKMS confirmed:

```text
broadcom-sta/6.30.223.271, 6.14.0-37-generic, x86_64: installed
```

The system was not rebooted until the module had been successfully built and installed.

---

## Phase 9: Test the Compatible Kernel

The exact GRUB entry was identified:

```bash
sudo grep -E \
  "^submenu |^[[:space:]]+menuentry " \
  /boot/grub/grub.cfg |
sed -E "s/^[[:space:]]+//" |
grep -E "Advanced options|6\.14\.0-37"
```

A one-time boot was scheduled:

```bash
sudo grub-reboot \
"Advanced options for Linux Mint>Linux Mint, with Linux 6.14.0-37-generic"
```

After reboot:

```bash
uname -r
```

Result:

```text
6.14.0-37-generic
```

---

## Phase 10: Verify the Correct Driver

The adapter was checked again:

```bash
lspci -nnk |
sed -n '/Network controller/,+4p'
```

The result now showed:

```text
Kernel driver in use: wl
```

The loaded modules were verified:

```bash
lsmod |
grep -E '^(wl|b43|bcma|brcmsmac|ssb)'
```

The proprietary driver was active:

```text
wl
```

The radio capabilities were checked:

```bash
iw phy phy0 info |
grep 'Band'
```

Both bands were now available:

```text
Band 1
Band 2
```

The driver correction restored the adapter’s 5 GHz capability.

---

## Phase 11: Rebuild the NetworkManager Profile

Old and conflicting connection profiles were removed:

```bash
nmcli connection delete "Auto Trusted-WiFi" 2>/dev/null
nmcli connection delete "Trusted-WiFi" 2>/dev/null
```

A clean profile was created:

```bash
nmcli connection add \
  type wifi \
  ifname wlp3s0 \
  con-name "Trusted-WiFi" \
  ssid "Trusted-WiFi"
```

The profile was configured for 5 GHz and WPA2:

```bash
nmcli connection modify "Trusted-WiFi" \
  802-11-wireless.band a \
  802-11-wireless-security.key-mgmt wpa-psk \
  802-11-wireless-security.pmf disable \
  connection.autoconnect yes
```

The connection was activated using an interactive password prompt:

```bash
nmcli --ask connection up "Trusted-WiFi"
```

Using `--ask` prevented the wireless password from being exposed in shell history.

---

## Phase 12: Validate Wi-Fi Independently

The interface, address, route, and wireless link were checked:

```bash
nmcli device status
ip -4 address show wlp3s0
ip route
iw dev wlp3s0 link
```

Ethernet was then disconnected so that Wi-Fi could be tested independently:

```bash
ping -c 3 1.1.1.1
ping -c 3 google.com
```

Both internet reachability and DNS resolution succeeded.

---

## Phase 13: Make the Working Kernel Persistent

After the one-time boot and Wi-Fi tests succeeded, GRUB was configured to retain the known-good kernel:

```bash
sudo sed -i \
  's/^GRUB_DEFAULT=.*/GRUB_DEFAULT=saved/' \
  /etc/default/grub
```

The working entry was saved:

```bash
sudo grub-set-default \
"Advanced options for Linux Mint>Linux Mint, with Linux 6.14.0-37-generic"
```

The kernel and headers were marked as manually installed:

```bash
sudo apt-mark manual \
  linux-image-6.14.0-37-generic \
  linux-headers-6.14.0-37-generic
```

GRUB was regenerated:

```bash
sudo update-grub
```

The configuration was verified:

```bash
sudo grub-editenv list
```

---

## Phase 14: Repair Package Management

The Broadcom package still attempted to build against every installed newer kernel. The incompatible HWE kernel packages were removed only after the following were confirmed:

* Kernel 6.14 booted successfully
* The `wl` module was installed
* Both wireless bands were visible
* Wi-Fi authenticated successfully
* DHCP and routing worked
* DNS worked
* Automatic Wi-Fi reconnection worked
* GRUB had a persistent working entry

After removing the incompatible kernels, package configuration completed successfully:

```bash
sudo dpkg --configure -a
```

Verification:

```bash
dpkg -l broadcom-sta-dkms
dkms status
```

Final result:

```text
ii broadcom-sta-dkms
broadcom-sta/6.30.223.271, 6.14.0-37-generic, x86_64: installed
```

Unused kernel packages were then removed:

```bash
sudo apt autoremove --purge
```

---

## Phase 15: Repair an Unrelated Repository Error

During validation, `apt update` revealed an obsolete third-party Ookla repository that did not provide packages for the current Ubuntu base release.

The source was located:

```bash
grep -Ril \
  'packagecloud.io/ookla/speedtest-cli' \
  /etc/apt/sources.list \
  /etc/apt/sources.list.d 2>/dev/null
```

The repository was disabled without permanently deleting its configuration:

```bash
sudo mv \
  /etc/apt/sources.list.d/ookla_speedtest-cli.list \
  /etc/apt/sources.list.d/ookla_speedtest-cli.list.disabled
```

Package management was validated again:

```bash
sudo apt update
sudo apt upgrade
sudo dpkg --audit
```

No remaining package errors were reported.

---

## Final Validation

The final system state was verified after reboot:

```bash
uname -r
nmcli device status
ip route
lspci -nnk |
sed -n '/Network controller/,+4p'
dkms status
ping -c 3 google.com
```

Final results:

| Test                   | Result              |
| ---------------------- | ------------------- |
| Persistent kernel      | `6.14.0-37-generic` |
| Wireless driver        | Broadcom STA `wl`   |
| DKMS status            | Installed           |
| 2.4 GHz support        | Available           |
| 5 GHz support          | Available           |
| Wi-Fi authentication   | Successful          |
| DHCP                   | Successful          |
| Routing                | Successful          |
| DNS                    | Successful          |
| Automatic reconnection | Successful          |
| Package state          | Clean               |
| Reboot persistence     | Verified            |

---

## Root Cause

The Wi-Fi failure was caused by a driver and kernel compatibility chain:

```text
Linux kernel 7.0
        ↓
Broadcom STA DKMS build failure
        ↓
System falls back to b43/bcma
        ↓
Only 2.4 GHz exposed for BCM4331
        ↓
Wireless association fails
        ↓
NetworkManager reports misleading ssid-not-found error
```

The permanent working configuration used Linux kernel 6.14 with the successfully compiled Broadcom STA `wl` driver.

---

## Operational Lessons

### Preserve a management connection

Ethernet remained connected during driver, kernel, and NetworkManager changes. This prevented loss of administrative access.

### Diagnose in layers

Wireless association, DHCP, routing, internet access, and DNS were tested separately.

### Treat visible errors as evidence, not conclusions

NetworkManager reported `ssid-not-found`, but kernel logs revealed the actual association rejection.

### Inspect actual driver capabilities

Detecting a wireless adapter does not prove that every hardware feature is supported by the active driver.

### Validate DKMS before rebooting

The replacement module was built and installed for the target kernel before the system was restarted.

### Use a one-time boot before changing defaults

The compatible kernel was tested through `grub-reboot` before it became the persistent default.

### Maintain a rollback path

The working kernel was protected from automatic removal until the complete repair survived reboot.

### Avoid weakening infrastructure prematurely

The UniFi SSID security configuration was not changed until the client driver and kernel were fully investigated.

### Verify changes after reboot

A repair is not complete until drivers, services, routes, and connections survive a normal restart.

---

## Skills Demonstrated

* Linux system administration
* NetworkManager troubleshooting
* Wi-Fi authentication analysis
* Kernel log interpretation
* Broadcom wireless diagnostics
* Linux kernel management
* DKMS module management
* GRUB boot configuration
* Debian and Ubuntu package recovery
* UniFi client correlation
* DHCP, routing, and DNS validation
* Change control and rollback planning
* Technical documentation
* Root-cause analysis

---

## Security Notes

This public case study intentionally excludes:

* Production SSIDs
* Wireless passwords
* Public and private IP addresses
* Device MAC addresses
* Access-point BSSIDs
* Personal usernames
* Internal hostnames
* Network-controller addresses
* Private VLAN assignments

The private operational record retains the exact values required for future recovery.
