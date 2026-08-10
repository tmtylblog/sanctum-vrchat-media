# Omada Home Network — Hardware Inventory & Discovered Topology

Reference notes so Claude sessions can pick up context about this network.
Originally sourced from Amazon order history screenshots (2026-08-09); this
revision adds live discovery pulled from the Omada controller web API by a
LAN-side session on 2026-08-09.

> **Redaction policy (this repo is PUBLIC):** no passwords, API secrets, or
> public IPs. Additionally withheld because the repo is public: device serial
> numbers, full MAC addresses (last 3 octets only — AP MACs are geolocatable
> BSSIDs), and camera location names (generic labels used). Full detail is
> visible in the controller UI.

## Gear (purchase inventory)

| Device | Role | Qty | Ordered |
|---|---|---|---|
| TP-Link Omada Hardware Controller (OC200 2.0) | Omada SDN controller | 1 | Jun 15, 2025 |
| TP-Link ER8411 | 10G VPN router (WAN/gateway) | 1 | Jun 8, 2025 |
| TP-Link TL-SX3008F | 8-port 10G SFP+ aggregation switch | 1 | Jun 21, 2025 |
| TP-Link TL-SG3428XMP | Jetstream 24-port Gigabit PoE+ switch (4× SFP+) | 2 | Nov 19, 2024 |
| Omada EAP783 | BE22000 tri-band Wi-Fi 7 ceiling AP | 3 | May 17, 2024 |
| TP-Link Omada EAP625-Outdoor HD | AX1800 Wi-Fi 6 outdoor AP | 1 | Jun 8, 2025 |
| TP-Link RE515X | AX1500 Wi-Fi 6 range extender w/ Ethernet port | 1 | Aug 2026 — **being returned** (EasyMesh, can't join Omada) |
| Omada EAP772-Outdoor | BE11000 Wi-Fi 7 outdoor AP — for back patio coverage | 1 | Ordered Aug 9, 2026 |

## Controller

- **OC200 2.0** hardware controller, name `Omada Controller_101`
- Controller version **6.2.0.17** (API v3), device firmware 2.24.9 (Apr 2026)
- Management IP **192.168.0.8** (HTTPS UI on 443/8043), timezone America/New_York
- Cloud-registered to omada.tplinkcloud.com; MSP mode off; single site
- Capacity in use: 4/100 APs, 3/20 switches, 1/10 gateways (8 devices total)
- Storage: 1.45 / 3.0 GB used

## Adopted devices (all CONNECTED — nothing pending adoption or offline)

| Type | Model | Name | IP | MAC (last 3) | Firmware | Clients | Uptime @ discovery | Update avail. |
|---|---|---|---|---|---|---|---|---|
| Gateway | ER8411 v1.0 | Office Router | 192.168.0.1 | …B7-2F-F0 | 1.3.6 (Oct 2025) | — | ~10 h | **YES** |
| Switch | SG3428XMP v3.20 | Office Main PoE Switch | 192.168.0.14 | …7B-A4-99 | 3.20.28 (May 2026) | 8 | ~12 h | no |
| Switch | SG3428XMP v3.20 | Third Floor PoE Switch | 192.168.0.124 | …7B-A5-93 | 3.20.28 (May 2026) | 11 | 68 d | no |
| Switch | SX3008F v1.20 | Office 10G Switch | 192.168.0.2 | …4C-98-59 | 1.20.23 (May 2026) | 1 | ~12 h | no |
| AP | EAP783 v1.0 | Main Floor EAP | 192.168.0.13 | …24-E0-80 | 1.1.5 (Dec 2025) | 9 | 129 d | no |
| AP | EAP783 v1.0 | Office EAP | 192.168.0.18 | …24-E2-C0 | 1.1.5 (Dec 2025) | 13 | 129 d | no |
| AP | EAP783 v1.0 | Third Floor EAP | 192.168.0.125 | …24-E2-E0 | 1.1.5 (Dec 2025) | 6 | 120 d | no |
| AP | EAP625-Outdoor HD v1.0 | Fairy Falls EAP | 192.168.0.6 | …22-5F-42 | 1.4.4 (Jul 2025) | 1 | ~12 h | **YES** |

## LAN networks / VLANs

| Network | VLAN | Gateway/Subnet | Purpose |
|---|---|---|---|
| Default | 1 | 192.168.0.1/24 | Management + trusted LAN + all Sonos (consolidated 2026-08-09) |
| Camera | 160 | 192.168.160.1/24 | PoE cameras + NVR |
| VR | 170 | 192.168.170.1/24 | VR / gaming PCs (incl. the RTX workstations) |

(SonosZP / VLAN 150 deleted 2026-08-09: all Sonos — 5 wired units on Office
Main ports 8-12 + ~12 wireless speakers — consolidated onto Default so the
Sonos app works from the main SSID. The dedicated `new-horizons-sonos` SSID,
both Sonos port profiles, and the VLAN were removed.)

## SSIDs (WLAN group "Default")

All WPA-Personal, all broadcast. Band code: 2.4/5/6 GHz.

| SSID | Bands | VLAN → network |
|---|---|---|
| new-horizons | 2.4 + 5 | untagged → Default (VLAN 1) — now also carries all wireless Sonos |
| new-horizons-gaming | 5 + 6 only (2.4 dropped 2026-08-10) | 170 → VR |
| new-horizons-security | 2.4 + 5 | 160 → Camera |

2.4 GHz channels are 1/6/11 across the three EAP783s (19 dBm); 5 GHz and
6 GHz enabled on all three; the outdoor EAP625 runs 2.4 (ch 6, 25 dBm) + 5 GHz.

## WAN (ER8411 "Office Router")

- **Dual WAN, both up and online, both DHCP** (no PPPoE):
  - SFP+ WAN1 — link up at 1 G, primary internet
  - WAN/LAN11 (RJ45) — link up at 1 G, second internet uplink
  - Public IPs redacted.
- SFP+ WAN/LAN2 in LAN mode, 10 G link up (feeds the 10G core).
- Router health at discovery: CPU 5 %, RAM 62 %, temp 60 °C, fan OK.
- **Firmware update available** (running 1.3.6 Build 20251028).

## Switching / PoE

**Office Main PoE Switch** (SG3428XMP, PoE budget 384 W, ~20.6 W in use):
- 2 PoE cameras (Camera profile), 5 Sonos Ports (SonosZP profile, ports 8–12)
- Ports 18/23/24 up on `All` profile (23/24 drawing PoE — likely APs)
- Port 28 = 10 G uplink (active). Port 25 is labeled "Third Floor Uplink" but
  is **link-down** — label appears stale; inter-switch traffic rides the 10G core.

**Third Floor PoE Switch** (SG3428XMP, PoE budget 384 W, ~40.4 W in use):
- 8 PoE cameras (Camera profile) + Reolink NVR (port 21, Camera profile)
- Dream PC "SUSPERIA-OG" on port 19 (VR profile, 1 G), LG TV on port 17
- Port 27 "Office Uplink" 10 G up (= designated uplink) and port 28 also 10 G up
- Port 25 labeled "Third Floor EAP" is link-down (stale label — the EAP is
  online via another path)

**Office 10G Switch** (SX3008F, no PoE):
- Port 1: "CHAMELEON-OG2 (VR PC)" — 10 G, VR profile
- Ports 2/3/4/7/8 up at 10 G on `All` profile; port 8 = designated uplink

## Topology summary

- 10G core confirmed: ER8411 (SFP+ LAN2, 10 G) → SX3008F aggregation → both
  SG3428XMP PoE switches uplinked at 10 G; SG3428XMPs cross-connected at 10 G.
- All Omada devices manage on VLAN 1 (192.168.0.x); user traffic is segmented
  onto VLANs 150/160/170 by SSID mapping and switch-port profiles
  (`Camera`, `SonosZP`, `VR`, `All`).
- Client load at discovery: 29 wireless clients across 4 APs; ~20 wired.

## Coverage plan (back patio) — in progress 2026-08-09

- Problem: weak Wi-Fi on the back patio. Fairy Falls EAP is deep in the
  forest (covers the waterfall area) and can't be relocated.
- **EAP772-Outdoor ordered** (Aug 9) — will be wired to a PoE switch port,
  adopted as "Back Patio EAP"; existing SSIDs propagate automatically.
- Stopgap applied: Main Floor EAP 2.4 GHz tx power raised 19 → 26 dBm
  (custom level). Revisit after the patio AP is adopted — may drop back to
  High/auto to keep indoor roaming crisp.
- RE515X extender being returned (consumer EasyMesh — incompatible with Omada).
- **DONE 2026-08-09: Sonos consolidation.** SSID retagged to Default (the
  controller's `vlanSetting` object is authoritative, not the legacy
  `vlanEnable` field), wired ports 8-12 + spares 21-22 re-profiled to
  Default, speakers re-homed to `new-horizons` via the Sonos app, then the
  `new-horizons-sonos` SSID, both Sonos profiles, and VLAN 150 deleted.
  Verified after settling: all 17 Sonos units on the main network.
  New wired additions same evening: Sonos Amp → Office Main port 3
  (re-profiled VR→Default, labeled "Sonos Amp (Living Room)"); a Beam in
  the master bedroom is wired via a shared jack/port (no distinct switch
  port lit up — port unknown, works fine on Default). Trueplay tuning
  failures during the migration were resolved by a Sonos software update.

## Printer & IoT on the gaming VLAN — 2026-08-10

- **HP Color LaserJet Pro M478f-9f**: now Ethernet-only on **Office Main
  switch port 4** (labeled), gigabit, **fixed IP 192.168.170.13** (gateway
  DHCP reservation), Wi-Fi radio off. Verified end-to-end with raw-9100 test
  prints. NOTE: the wall-jack/path it was first plugged into passes link but
  drops traffic — flaky, do not reuse without testing. Its IP changed
  .11 → .13 after a clean boot; reservation prevents future drift.
  Model per EWS: M479fdw (panel shows M478f-9f). Firmware updated
  2026-08-10, now CLRWTRXXXN002.2539E.00 — no update pending.
- **2026-08-10: 2.4 GHz removed from `new-horizons-gaming`** — now 5+6 GHz
  only. The ESP32s that were its last 2.4 tenants are dead/being redone
  anyway (Freddie's call); when rebuilt they should join `new-horizons`
  (Default), not the gaming SSID.
- MLO (Wi-Fi 7 multi-link) remains OFF on the gaming SSID — offered as an
  optional performance upgrade for MLO-capable clients (Pixel 9, new laptops).

## WAN diagnosis (Sonos/Spotify stream drops) — 2026-08-09

Symptom: streaming to Sonos cuts out intermittently (music stops on all
speakers). Firewall ruled out — ACL engine disabled, zero ACL rules on
gateway/switches/APs (note: this also means VLANs are not actually isolated).

Findings:
- **Primary WAN (SFP+ WAN1) is double-NAT'd**: ER8411 WAN has a private IP
  (192.168.1.234) behind an AT&T Fiber gateway (ARRIS BGW-class) at
  192.168.1.254 doing its own NAT. Every LAN session consumes the BGW's
  limited NAT session table — with this house's device/agent load, session
  exhaustion there is the prime suspect for random mid-stream drops.
- **Backup WAN (port 11) is Starlink** (CGNAT). Its online-detection is
  disabled/failing (onlineDetection=0), so failover can fire onto a link
  that isn't actually usable, and any failover/failback resets all sessions.
- WAN mode: failover (primary/backup, weight 1,0), app-optimized routing on.

Fix status (2026-08-09):
1. **DONE — IP Passthrough enabled.** AT&T BGW320-500 → Firewall → IP
   Passthrough → Passthrough / DHCPS-fixed, targeting the ER8411's
   AT&T-facing WAN interface **MAC ...2f:f1** (SFP+ WAN1, the one holding
   the 192.168.1.x lease). NOTE: the gateway had prefilled the WRONG MAC
   (...2f:fb = the Starlink-facing WAN); using it would have broken WAN.
   Correct MAC confirmed against controller port-stats before saving.
   Gateway restarted to apply. **Verified: ER8411 SFP+ WAN1 now holds a
   PUBLIC IP directly — double NAT eliminated.** This is the likely root
   cause of the Sonos/Spotify stream drops (BGW NAT-session-table
   exhaustion under this house's device load).
2. **Starlink backup online-detection: NO CHANGE — working as designed.**
   Reconsidered: `onlineDetection=0` on the backup WAN is normal for a
   standby link in Link-Backup mode; detection that matters is on the
   PRIMARY (AT&T, =1). And we live-tested failover during the AT&T reboot:
   WAN1 went down, the house rode Starlink with no outage, then failed back
   to AT&T on the public IP. Failover verified good — left untouched.
3. **TODO: confirm the Sonos/Spotify drops are actually gone** now that
   double-NAT is fixed (requires playing music over a day or two).

## Open items

- **Firmware**: ER8411 upgraded 2026-08-10 to 1.4.3 Build 20260721 (by
  Freddie via controller UI; IP Passthrough and public IP survived the
  reboot, verified). EAP625-Outdoor update still pending — last device
  with an available update. Everything else current.
- **MLO on the gaming SSID**: attempted via API 2026-08-10 — controller
  v6.2 rejects all direct SSID patches (wants its UI wizard to manage the
  WPA3/PMF transition). Do via UI if desired: Settings → Wireless Networks
  → new-horizons-gaming → MLO. Benefit limited to Pixel/newest laptops;
  Quests gain nothing. SSID left unchanged (5+6 GHz, WPA2/3, MLO off).
- **Reboot event ~12 h before discovery** hit the router, both office
  switches, and the outdoor AP (uptimes all ~10–12 h) while the Third Floor
  switch (68 d) and the three EAP783s (120–129 d, PoE-powered) stayed up —
  looks like a power blip on the office circuit. Worth a small UPS on the
  office rack if not already planned.
- **Stale port labels**: port 25 on both SG3428XMPs is labeled as an
  uplink/EAP port but link-down. Relabel to match current cabling.
- **RE515X placement**: decide where the new extender goes; it's EasyMesh
  (not Omada), so it extends an SSID but won't be controller-managed.
- **Repo visibility**: this repo is public — serials, full MACs, and camera
  location names are intentionally withheld. Consider a private repo for
  fuller documentation.
