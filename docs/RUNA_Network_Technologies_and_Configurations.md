# Network Technologies & Configurations

**Project:** RUNA S.R.L. Enterprise Network Design

---

## Lab Access

> These credentials are for the Packet Tracer lab only. They are exclusive to this project and are not used anywhere else.

### Credentials

| Device / Service | Username | Password | Notes |
|---|---|---|---|
| Network devices (console) | n/a | `cisco` | Local |
| Network devices (SSH) | `admin` | `Runa-Lab-2026!` | Local fallback if the TACACS+ server is unreachable |
| Privileged mode (`enable`) | n/a | `cisco` | Same on all switches |
| TACACS+ server (AAA) | `silvio.rojas` | `Runa-Lab-2026!` | Server at 10.10.200.6 |
| TACACS+ shared key | n/a | `{View on Packet Tracer}` | Configured on the switches |
| WLC web interface | `admin` | `Runa-Lab-2026!` | `https://10.10.242.10 or https://wlc.runa.com` |
| CORP-WIFI | n/a | `Runa-Lab-2026!` | WPA2-PSK |
| GUEST-WIFI | n/a | n/a | Open network, isolated on VLAN 216 |
| Voice Gateway | `silvio.rojas` | `Runa-Lab-2026!` | Configured on the Voice Gateway |

### How to Access the Devices

- **Console:** available on every device from the Packet Tracer CLI tab. This is the quickest way to inspect a configuration.
- **SSH to switches:** VTY lines accept SSH v2 only and are restricted by ACL to the IT-Infra (`10.10.50.0/25`), NOC (`10.10.60.0/25`) and SOC (`10.10.70.0/25`) subnets.
- **WLC:** from a PC open `https://10.10.242.10` or `https://wlc.runa.com` in the browser (only works with HTTPS).

---

## Layer 2 & Switching

| Technology | Device | Purpose |
|---|---|---|
| VLAN segmentation (16 VLANs) | Access SW, Core SW | Traffic isolation by department and function (Admin, Sales, Dev, NOC, SOC, Guest, VoIP, etc.) |
| 802.1Q Trunking | Access SW, Core SW | Carry multiple VLANs across switch uplinks; native VLAN and allowed-VLAN pruning on AP uplinks |
| Voice VLAN (VLAN 240) | Access SW | Separate IP phone traffic from data traffic |
| VTP Transparent mode | Access SW, Core SW | Prevent unintended VLAN database propagation |
| PVST+ with PortFast | Access SW, Core SW | Loop prevention with fast port activation on end-user ports |
| EtherChannel (LACP, 4 links) | Core SW | Link aggregation for bandwidth and redundancy between switches |

## Layer 3 & Routing

| Technology | Device | Purpose |
|---|---|---|
| Inter-VLAN routing (SVIs) | Core SW | Route traffic between internal VLANs |
| Router-on-a-stick (802.1Q subinterface) | Voice Gateway | Route the VoIP VLAN (240) through a dedicated subinterface |
| HSRP (v1/v2) | Core SW | First-hop gateway redundancy for all VLANs |
| OSPF (Process 35, Area 0) | Core SW, Firewall | Dynamic routing between core and perimeter |
| Passive-interface default | Core SW | Restrict OSPF adjacencies to intended interfaces only |
| Routed /30 point-to-point links | Core SW, Firewall | Efficient addressing between core and firewall |
| Static and floating static routes | Core SW, Firewall | Primary/backup default routes |
| DHCP Relay (`ip helper-address`) | Core SW | Forward DHCP requests to the centralized DHCP server |
| NetFlow v9 | Core SW, Voice Gateway | Traffic visibility and monitoring |
| Subnetting / VLSM | Network-wide | Efficient IP plan (/22, /23, /24, /25, /27, /30) |

## Perimeter Security (Cisco ASA 9.6)

| Technology | Device | Purpose |
|---|---|---|
| Security zones and levels | Firewall | OUTSIDE (0), GUEST (10), DMZ (50), INSIDE (100) |
| Dual-ISP design | Firewall | Internet redundancy with primary/backup routes by administrative distance |
| Dual INSIDE links | Firewall, Core SW | Redundant path to the internal network |
| Dynamic NAT / PAT | Firewall | Internet access for internal subnets via interface overload, per ISP and inside pair |
| Static NAT | Firewall | Publish DMZ servers (Web via ISP1, NAS via ISP2) |
| Inbound ACLs (OUTSIDE_IN) | Firewall | Allow only HTTP/HTTPS to DMZ services |
| Stateful inspection | Firewall | Inspect DNS, FTP, HTTP, ICMP and TFTP |
| DMZ | Firewall | Isolate public-facing servers from the internal network |
| Isolated Guest network | Firewall | Keep guest traffic separated from corporate resources |

## Layer 2 Security

| Technology | Device | Purpose |
|---|---|---|
| DHCP Snooping | Access SW | Block rogue DHCP servers; trusted uplinks and rate limiting on access ports |
| BPDU Guard | Access SW | Protect STP against rogue switches on access ports |
| Unused ports shutdown + Blackhole VLAN (2000) | Access SW | Reduce attack surface from unused ports |

## Device Hardening & Management

| Technology | Device | Purpose |
|---|---|---|
| AAA with TACACS+ (local fallback) | Access SW, Core SW | Centralized authentication |
| SSH v2 only | Access SW, Core SW | Secure remote management, Telnet disabled |
| VTY access-class (ACL) | Access SW, Core SW | Restrict management access to IT-Infra subnets |
| Management VLANs (VLAN 500 network, VLAN 242 wireless) | Access SW, Core SW, WLC | Dedicated management networks separated from user traffic |
| Password encryption, `enable secret`, console timeout | Access SW, Core SW | Credential protection and session hygiene |
| MOTD legal banner | Access SW, Core SW | Unauthorized-access warning |

## Wireless (Cisco WLC + Access Points)

| Technology | Device | Purpose |
|---|---|---|
| Centralized WLAN management | WLC | Single point of control for all access points |
| Dual SSID design (CORP-WIFI / GUEST-WIFI) | WLC, APs | Separate employee and guest wireless access |
| WPA2-PSK security (CORP-WIFI) | WLC | Protect corporate wireless traffic |
| Open guest SSID mapped to an isolated VLAN (216) | WLC, Firewall | Guest internet access with no path to internal resources |
| Dynamic interfaces mapped to VLANs (CORP: 208, GUEST: 216) | WLC | Tie each WLAN to its own VLAN and subnet |
| FlexConnect Local Switching | WLC, APs | Switch client traffic locally at the AP/switch instead of tunneling it back to the WLC |
| AP Groups (RUNA_PP, Parque Patricios HQ) | WLC | Apply the right WLANs to the APs of each site |
| AP trunk uplinks (native VLAN 242, allowed 208/216/242) | Access SW | Transport management and client VLANs to the APs |
| Internal DHCP scope (GUEST-WIFI-Pool) | WLC | Local addressing for guest clients (10.10.216.0/22) |
| External DHCP via relay | DHCP Server, Core SW | Addressing for corporate clients |
| NTP synchronization | WLC, NTP Server | Consistent time across wireless infrastructure |

## Voice (VoIP / Cisco CME)

| Technology | Device | Purpose |
|---|---|---|
| Cisco Unified Communications Manager Express (CME) | Voice Gateway (Cisco 2811) | Call control for the IP phones |
| SCCP (Skinny), port 2000 | Voice Gateway, IP Phones | Phone registration and signaling |
| Ephone / Ephone-DN configuration (extensions 1001-1042) | Voice Gateway | Extension and phone-to-line mapping |
| Auto-assign for DNs | Voice Gateway | Automatic extension assignment to phones |
| Dedicated Voice VLAN (240) with /23 addressing | Voice Gateway, Access SW | Isolated voice traffic with room for 500+ devices |
| Local DHCP pool with Option 150 | Voice Gateway | Automatic IP assignment and TFTP/CME server discovery for phones |
| 802.1Q subinterface (Fa0/0.240) | Voice Gateway | Connect the voice VLAN to the gateway |

## Servers & Services

| Technology | Device | Purpose |
|---|---|---|
| Centralized DHCP | DHCP Server | Dynamic addressing for internal VLANs through relay |
| TACACS+ | AAA Server | Centralized authentication for network devices |
| NTP | NTP Server (ntp01.runa.com) | Time synchronization for logs and troubleshooting |
| DNS | DNS Server | Internal name resolution |
| Web hosting | Web Server (DMZ) | Public-facing service |
| Web hosting | Web Server (Intranet / LAN) | Internal Portal Server |
| File storage | NAS (DMZ) | Published file service over HTTPS |
| WAN / ISP connectivity | Routers | Dual-ISP edge connectivity |
