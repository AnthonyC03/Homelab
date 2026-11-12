# Home Network & Cybersecurity Homelab

I'm building this home network and lab to get hands-on experience with Cisco networking, VLANs, firewalls, and network monitoring. This repository is where I keep track of what I configure, how I test it, and what I learn when something doesn't work.

## Current Status

Both Cisco switches have saved configurations from earlier setup sessions. My next step is to verify and back up those settings, connect the switches to my Verizon router, and set up the U7 Lite.

The AP, Ethernet cable, connectors, and cable-making tools are purchased, with delivery expected on November 12, 2026. Internet access through the switches and the new Wi-Fi setup are still pending testing.

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
| Ubiquiti U7 Lite | Purchased; delivery and setup pending |
| Acer Predator Helios 300 PH315-54 | Current lab laptop; 16 GB RAM |
| USB-to-RJ45 FTDI console cable | Switch console access |
| Monoprice Cat6 Ethernet cable | Purchased; delivery pending |
| Cat6 RJ45 pass-through connectors, 50-pack | Purchased; compatibility to be checked before use |
| Klein cable crimper and cable tester | Purchased; delivery pending |
| NIIMBOT D11 and M1 | Cable and equipment labeling |

### Planned Purchases

| Equipment | Intended role |
| --- | --- |
| Protectli Vault V1610 | Main router and firewall |
| Cisco C1111-8P | Lab router |
| Dedicated server — model undecided | Splunk, monitoring, and other virtual machines |

**Internet service:** Verizon Fios, 1 Gbps.

## Switch Configuration Progress

Earlier setup sessions included:

- Setting switch hostnames and management access.
- Creating VLAN 10 (MANAGEMENT), VLAN 40 (CYBER-LAB), and VLAN 999 (NATIVE-UNUSED).
- Configuring the inter-switch trunk on Gi1/0/1.
- Setting SW1 as the spanning-tree root.
- Testing connectivity between switch management interfaces.
- Saving the configurations.

I'll verify the settings during the next session before publishing detailed configurations or connecting the switches to the home network.

## Next Setup Session

1. Check that the cable, connectors, and crimper are compatible.
2. Measure, terminate, test, and label the Ethernet cables.
3. Power on the switches, back up their original configurations, and verify the inter-switch connection.
4. Check the Verizon LAN and DHCP settings, then configure the required switch ports and VLAN connections.
5. Connect Verizon to SW1 and test wired connectivity through both switches.
6. Set up UniFi Network on the laptop, connect and adopt the U7 Lite, and configure Wi-Fi.
7. Test IP addressing, internet access, Wi-Fi coverage, and management access.
8. Save and back up the final settings, then restart devices one at a time to verify recovery.
9. Record port assignments, test results, and troubleshooting notes.

### Planned Cable Labels

| Label | Connection |
| --- | --- |
| C01 | Verizon LAN port to SW1 |
| C02 | SW1 Gi1/0/1 to SW2 Gi1/0/1 |
| C03 | SW1 PoE port to U7 Lite |
| C04 | Switch to laptop, if needed |

Final router, AP, and laptop port assignments will be recorded after setup.

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
