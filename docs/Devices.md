# RUNA SRL - Network Equipment

Portfolio project: Enterprise Network Architecture (v1.0, november 2026).

## Summary

| Category | Quantity |
|---|---|
| Firewall | 1 |
| Switches L3 (core) | 2 |
| Access Switches | 9 |
| Routers | 4 (1 Internet + 2 ISP + 1 Voice GW) |
| WLAN Controller | 1 |
| Access Points | 5 |
| Servers | 9 |
| End devices | 37 (includes 2 Internet users) |

## Security, routing and core

| Name | Model | Function | IP / VLAN |
|---|---|---|---|
| PP-FW-1 | Cisco ASA 5506-X | Perimeter firewall. GUEST and DMZ gateway| .1 in VLANs 216 and 201, 10.10.254.1 and .5 toward the cores
| CORE-SW-1 | Catalyst 3650-24PS | Core, active HSRP, root bridge STP | .2 in each VLAN, 10.10.254.2/30 |
| CORE-SW-2 | Catalyst 3650-24PS | Core, HSRP standby | .3 in each VLAN, 10.10.254.6/30 |
| Voice GW | Cisco 2811 | VoIP Gateway | 10.10.240.1 (VLAN 240) |
| TELECOM-ISP | Cisco 2911 | ISP 1 Router | 190.16.89.1/30 |
| TELEFÓNICA-ISP | Cisco 2911 | ISP 2 Router | 181.24.50.5/30 |
| INTERNET / CLOUD | Cisco 2911 | Simulate the Internet | 8.0.0.1/8 |


## Access Switches (Cisco 2960-24TT)

| Name | Location | User VLANs | Uplinks |
|---|---|---|---|
| SW-F1-1 | 1° Floor | 10 (CORP-ADMIN) | CORE-SW-1 and CORE-SW-2 |
| SW-F1-2 | 1° Floor | 11 (CONSULTING), 12 (CUSTOMER-SUPPORT) | CORE-SW-1 and CORE-SW-2 |
| SW-F2-1 | 2° Floor | 13 (EXEC-MGMT) | CORE-SW-1 and CORE-SW-2 |
| SW-F2-2 | 2° Floor | 14 (DEV-ENGINEERING), 15 (SALES) | CORE-SW-1 and CORE-SW-2 |
| SW-F3-1 | 3° Floor | 50 (IT-INFRA), 60 (NOC), 70 (SOC) | CORE-SW-1 and CORE-SW-2 |
| SW-F3-SERVER | 3° Floor | 200 (INTERNAL-SERVERS) | CORE-SW-1 and CORE-SW-2 |
| SW-GUEST | 3° Floor | 216 (WIFI-GUEST) | CORE-SW-1 and CORE-SW-2 (trunk) and Fa0/1 to firewall |
| SW-DMZ | 3° Floor | 201 (DMZ) | Gi0/1 to firewall (access VLAN 201) |
| SW-INTERNET | Internet / Cloud | - | Router-Internet | 

## Wireless

| Name | Model | Location | Notes |
|---|---|---|---|
| PP-WLC | Cisco WLC-3504 | 3° Floor | 10.10.208.10 (CORP-WIFI), 10.10.216.10 (GUEST), 10.10.242.10 (WIFI-MGMT) |
| PP-AP-F1-1 | Cisco 3702i | 1° Floor | Trunk, native VLAN 242, allowed 208, 216, 242 |
| PP-AP-F1-2 | Cisco 3702i | 1° Floor | Trunk, native VLAN 242, allowed 208, 216, 242 |
| PP-AP-F2-1 | Cisco 3702i | 2° Floor | Trunk, native VLAN 242, allowed 208, 216, 242 |
| PP-AP-F2-2 | Cisco 3702i | 2° Floor | Trunk, native VLAN 242, allowed 208, 216, 242 |
| PP-AP-F3-1 | Cisco 3702i | 3° Floor | Trunk, native VLAN 242, allowed 208, 216, 242 |

## Servers

### DMZ (VLAN 201, 10.10.201.0/27)

| Name | Private IP | Public IP |
|---|---|---|
| WEB-01 (runa.com) | 10.10.201.11 | 190.16.89.9 |
| FTP | 10.10.201.10 | - |
| NAS-STORAGE (nas.runa.com) | 10.10.201.13 | 181.24.50.9 |

### Internal (VLAN 200, 10.10.200.0/27)

| Name | IP |
|---|---|
| DHCP | 10.10.200.4 |
| DNS | 10.10.200.5 |
| AAA | 10.10.200.6 |
| NTP | 10.10.200.7 |
| WEB-02 (portal.runa.com) | 10.10.200.8 |

### External (Internet / Cloud 8.0.0.0/8)

| Name | IP |
|---|---|
| Google (DNS, WEB) | 8.8.8.8 |

## End devices

| Type | Model in Packet Tracer | Quantity |
|---|---|---|
| Corporate PCs | PC-PT | 5 |
| Printers | Printer-PT | 5 |
| IP phones | Cisco 7960 | 10 |
| Laptops | Laptop-PT | 5 |
| Tablets | TabletPC-PT | 5 |
| Smartphones | Smartphone-PT | 5 |
| Internet Users | PC-PT (USA 8.50.25.125 and JAPAN 8.253.235.10) | 2 |
