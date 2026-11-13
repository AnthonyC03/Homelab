# Home Network & Cybersecurity Homelab

I'm building this home network and lab to get hands-on experience with Cisco networking, VLANs, firewalls, and network monitoring. This repository is where I keep track of what I configure, how I test it, and what I learn when something doesn't work.

## Current Status

The Verizon router is now connected to SW1 on VLAN 50 (HOME-NETWORK), and wired internet access through SW1 has been tested. VLAN 50 is allowed across the existing inter-switch trunk, with forwarding confirmed on SW2. Wired connectivity through SW2 has also been validated.

The U7 Lite firmware update and standalone setup through the UniFi phone app are complete. Wireless addressing and internet access have been verified, and speed testing is complete. The final 5 GHz settings are **80 MHz on channel 149**.

Both switch configurations and the AP settings are saved. Separate configuration backups and restart recovery testing remain future tasks.

The Verizon CR1000A will remain my main router until I purchase and configure the Protectli Vault V1610. Virtualization, Splunk, and dedicated monitoring are later parts of the project.

## Goals

- Build a reliable wired and wireless home network.
- Learn Cisco configuration and network troubleshooting through hands-on work.
- Separate home, guest, and lab traffic with verified firewall rules.
- Add a dedicated server for virtual machines, Splunk, and monitoring.
- Keep useful configuration notes, diagrams, and test results.

## Equipment

### Owned or Purchased

| Equipment | Role or status |
| --- | --- |
| Verizon CR1000A | Current home router |
| Cisco WS-C2960X-24PD-L | PoE+ switch — SW1-HOMELAB |
| Cisco WS-C2960X-24TS-L | Second switch — LAB-SW2 |
| Ubiquiti U7 Lite | Operational on SW1 port 21; firmware updated and standalone Wi-Fi setup complete |
| Acer Predator Helios 300 PH315-54 | Current lab laptop; 16 GB RAM |
| USB-to-RJ45 FTDI console cable | Switch console access |
| Monoprice Cat6 Ethernet cable | Purchased; cable preparation/testing details still to be documented |
| Cat6 RJ45 pass-through connectors, 50-pack | Purchased; compatibility to be checked before use |
| Klein cable crimper and cable tester | Purchased; cable preparation/testing details still to be documented |
| NIIMBOT D11 and M1 | Cable and equipment labeling |

### Planned Purchases

| Equipment | Intended role |
| --- | --- |
| Protectli Vault V1610 | Main router and firewall |
| Cisco C1111-8P | Lab router |
| Dedicated server — model undecided | Splunk, monitoring, and other virtual machines |

**Internet service:** Verizon Fios, 1 Gbps.

## Switch Configuration Progress

The current inter-switch link is **Gi1/0/24 on both switches**, confirmed using interface status and CDP. It is an 802.1Q trunk with native VLAN 999. VLAN 50 was added to the existing allowed list without replacing VLANs 10, 20, 30, and 40.

| VLAN | Name | Current use |
| --- | --- | --- |
| 10 | MANAGEMENT | Switch management; laptop uses a static address |
| 20 | LAB | Lab device ports |
| 30 | SERVERS | Server ports |
| 40 | SECURITY | Security lab ports |
| 50 | HOME-NETWORK | Verizon LAN, wired test client, and U7 Lite Wi-Fi |
| 999 | NATIVE-UNUSED | Native VLAN on the inter-switch trunk |

VLAN 50 extends the existing Verizon LAN through the switches. It uses **192.168.1.0/24**, with Verizon at **192.168.1.1** providing the gateway and DHCP. Creating this VLAN does not give VLANs 10–40 internet access or configure routing between them.

### Current Port Assignments

| Switch | Port | Role | VLAN / mode |
| --- | --- | --- | --- |
| SW1 | Gi1/0/1 | Management laptop | Access VLAN 10 |
| SW1 | Gi1/0/21 | U7 Lite AP | Access VLAN 50; automatic PoE |
| SW1 | Gi1/0/22 | Temporary wired client test | Access VLAN 50 |
| SW1 | Gi1/0/23 | Verizon LAN uplink | Access VLAN 50 |
| SW1 | Gi1/0/24 | Trunk to SW2 port 24 | VLANs 10, 20, 30, 40, 50; native 999 |
| SW2 | Gi1/0/24 | Trunk to SW1 port 24 | VLANs 10, 20, 30, 40, 50; native 999 |

Ports 21–23 on SW1 were reassigned from VLAN 40 during this session. PortFast was removed from the Verizon uplink. The AP port retains its edge PortFast setting.

Earlier sessions established management access and tested connectivity between the switch management interfaces. The previous README listed port 1 as the trunk; the current outputs confirm port 24 instead. SW1 should not be described as the root for every VLAN: the VLAN 50 output showed an upstream root reached through port 23.

### Tests and Troubleshooting

| Check | Observed result |
| --- | --- |
| Verizon uplink | SW1 port 23 connected; VLAN 50 spanning-tree state FWD |
| Inter-switch link | CDP confirmed port 24 to port 24; link negotiated 1 Gbps |
| VLAN 50 trunk allowance | Added on both ends; forwarding confirmed on SW2 port 24 |
| Wired client on SW1 port 22 | Internet access confirmed; approximately 700 Mbps download in a speed test |
| Wired connectivity through SW2 | Validated; detailed client port and speed results not recorded |
| IPv4 DHCP | Laptop received 192.168.1.163/24 with gateway 192.168.1.1 |
| AP power | SW1 port 21 showed PoE on, 15.4 W reported, Class 4 |
| AP link | Port 21 connected in VLAN 50 at 1 Gbps, full duplex; no input/CRC/output errors or output drops in the captured counters |
| AP Wi-Fi service | Firmware update and password-protected Wi-Fi setup complete; internet access verified |
| Wireless IPv4 addressing | Client received an address in 192.168.1.0/24 with gateway 192.168.1.1 |
| Final 5 GHz settings | 80 MHz, channel 149; saved and retested |
| Configuration saves | Both switches and AP settings saved |

The laptop initially retained its management address, **192.168.10.10/24**, after moving to VLAN 50. It showed an IPv6 gateway and could access the internet, but IPv4 was still on the wrong subnet. Changing Ethernet IPv4 and DNS to automatic obtained the expected Verizon DHCP settings.

After the wired test, I returned the laptop to SW1 port 1 with its static management address and reconnected by SSH to **192.168.10.2**. During that stage, Verizon Wi-Fi provided internet while Ethernet was used for management. Verizon Wi-Fi was later disabled for the AP speed comparison.

## Wi-Fi Performance Testing

I tested the U7 Lite with an iPhone 17 Pro Max and compared it with a wired laptop connection. These are approximate internet speed-test download results, not local throughput benchmarks or guaranteed speeds.

| Test condition | Download result |
| --- | --- |
| Initial U7 Lite setup at 40 MHz | About 300 Mbps |
| U7 Lite after saving 80 MHz on channel 149 | About 500 Mbps |
| U7 Lite at 80 MHz with Verizon Wi-Fi disabled | About 576 Mbps |
| Temporary 160 MHz / channel 36 trial | About 630–640 Mbps, with repeated results around 640 Mbps |
| Laptop wired to SW1 port 22 | About 700 Mbps |

After the wider-channel trial, I returned to **80 MHz on channel 149**, repeated Wi-Fi testing, and confirmed I was satisfied with the results. An exact speed for that final retest was not recorded; the 640 Mbps result above belongs to the temporary 160 MHz trial.

Changing channel width and disabling the nearby Verizon Wi-Fi improved the observed results during testing, but these tests do not isolate every cause. Internet speed tests can vary with server load, other traffic, and radio conditions. Long-term stability and coverage will be checked during normal use.

## Next Steps

1. Export private recovery copies of both saved switch configurations; sanitize any examples before publishing.
2. Record cable testing and physical labels; confirm the final role of the temporary test port.
3. Document room-by-room coverage and monitor Wi-Fi stability during normal use.
4. After backing up, verify recovery by restarting devices one at a time.
5. Add routing and firewall policies when the Protectli is installed, then test isolation.

### Planned Cable Labels

| Label | Connection |
| --- | --- |
| C01 | Verizon LAN port to SW1 Gi1/0/23 |
| C02 | SW1 Gi1/0/24 to SW2 Gi1/0/24 |
| C03 | SW1 Gi1/0/21 to U7 Lite |
| C04 | Laptop to SW1 Gi1/0/1 for management; port 22 for VLAN 50 testing |

These are proposed labels; physical labeling has not yet been documented.

## Documentation to Add

- **Network design:** topology, cable labels, switch ports, VLANs, and IP addressing.
- **Configuration notes:** commands, settings, and the reasons for each change.
- **Testing and troubleshooting:** expected results, actual results, problems, and fixes.
- **Configuration backups:** sanitized copies of working configurations, with credentials and sensitive details removed.

Lab isolation has not been verified. I'll confirm traffic separation and the required firewall rules before running security exercises.

## Setup Log

### October 29, 2026 — Started Documentation

- Created this repository.
- Added the project goals.
- Listed owned equipment separately from planned purchases.

### November 12, 2026 — Prepared for Network Setup

- Cleaned up the README and updated the equipment list.
- Documented the Verizon CR1000A as the temporary main router.
- Recorded earlier switch work to verify during the next session.
- Added the setup sequence and planned cable labels.
- Kept planned work separate from completed configuration and testing.

### November 12, 2026 — Verizon Uplink and AP Setup

- Connected Verizon to SW1 port 23 on VLAN 50.
- Verified wired internet access on SW1 port 22 and corrected the laptop's IPv4 settings from static management addressing to DHCP for the test.
- Confirmed the actual inter-switch connection uses port 24 on both switches.
- Added VLAN 50 to both trunk allow lists and confirmed forwarding on SW2.
- Returned the laptop to VLAN 10 for SSH management.
- Connected the U7 Lite to SW1 port 21 and verified PoE and the network link.
- Started AP setup and a firmware update through the UniFi phone app; completed both later in the session.

### November 12, 2026 — End-of-Day Update

- Completed the U7 Lite firmware update and password-protected standalone Wi-Fi setup.
- Verified wireless client addressing and internet access through VLAN 50.
- Confirmed wired connectivity through SW2.
- Checked the AP's wired link: 1 Gbps, full duplex, with no errors or output drops in the captured interface counters.
- Compared wired and wireless download speeds and tested 40, 80, and 160 MHz channel widths.
- Settled on **80 MHz / channel 149** after retesting; 160 MHz / channel 36 was a temporary trial.
- Saved both switch configurations and the AP settings.
- Left separate backup exports, restart recovery checks, and future firewall isolation testing on the remaining-work list.
