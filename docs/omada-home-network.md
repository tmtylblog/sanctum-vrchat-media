# Omada Home Network — Hardware Inventory

Reference notes so Claude sessions can pick up context about this network.
Sourced from Amazon order history screenshots (shared 2026-08-09).

## Gear

| Device | Role | Qty | Ordered |
|---|---|---|---|
| TP-Link Omada Hardware Controller (OC200-class) | Omada SDN controller | 1 | Jun 15, 2025 |
| TP-Link ER8411 | 10G VPN router (WAN/gateway) | 1 | Jun 8, 2025 |
| TP-Link TL-SX3008F | 8-port 10G SFP+ aggregation switch | 1 | Jun 21, 2025 |
| TP-Link TL-SG3428XMP | Jetstream 24-port Gigabit PoE+ switch | 2 | Nov 19, 2024 |
| Omada EAP783 | BE22000 tri-band Wi-Fi 7 ceiling AP | 3 | May 17, 2024 |
| TP-Link Omada EAP625-Outdoor HD | AX1800 Wi-Fi 6 outdoor AP | 1 | Jun 8, 2025 |

## Topology notes

- Fully Omada-managed stack: router, switches, and APs all adoptable by the
  hardware controller.
- 10G core: ER8411 (10G SFP+ WAN/LAN) → TL-SX3008F aggregation → SG3428XMP
  PoE+ access switches (4× SFP+ uplinks each).
- PoE budget: each SG3428XMP provides 384W PoE+ — plenty for the EAP783s
  (PoE++/802.3bt preferred for full Wi-Fi 7 performance; check per-port
  wattage) and the EAP625-Outdoor.
- Known open items / details to fill in as they come up:
  - VLAN layout
  - SSID plan
  - Firmware/controller version
  - WAN details
