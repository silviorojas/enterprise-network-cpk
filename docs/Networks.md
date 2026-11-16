# Networks
| Subnet | VLAN | Zone | Description |
|---|---|---|---|
| 8.0.0.0/8 | n/a | OUTSIDE | INTERNET / CLOUD |
| 20.20.20.0/30 | n/a | OUTSIDE | TELECOM-TO-INTERNET |
| 30.30.30.0/30 | n/a | OUTSIDE | TELEFÓNICA-TO-INTERNET |
| 190.16.89.0/30 | n/a | OUTSIDE | TELECOM-ISP |
| 181.24.50.4/30 | n/a | OUTSIDE | TELEFÓNICA-ISP |
| 190.16.89.8/29 | n/a | OUTSIDE | TELECOM - DMZ Public IP Address Allocation |
| 181.24.50.8/29 | n/a | OUTSIDE | TELEFÓNICA - DMZ Public IP Address Allocation |
| 10.10.0.0/24 | 500 | INSIDE | NETWORK-MGMT |
| 10.10.10.0/24 | 10 | INSIDE | CORP-ADMIN |
| 10.10.11.0/24 | 11 | INSIDE | CONSULTING |
| 10.10.12.0/24 | 12 | INSIDE | CUSTOMER-SUPPORT |
| 10.10.13.0/24 | 13 | INSIDE | EXEC-MGMT |
| 10.10.14.0/24 | 14 | INSIDE | DEV-ENGINEERING |
| 10.10.15.0/24 | 15 | INSIDE | SALES |
| 10.10.50.0/25 | 50 | INSIDE | IT-INFRA |
| 10.10.60.0/25 | 60 | INSIDE | NOC |
| 10.10.70.0/25 | 70 | INSIDE | SOC |
| 10.10.200.0/27 | 200 | INSIDE | INTERNAL-SERVERS |
| 10.10.201.0/27 | 201 | DMZ | DMZ |
| 10.10.208.0/22 | 208 | INSIDE | CORP-WIFI |
| 10.10.216.0/22 | 216 | GUEST | WIFI-GUEST |
| 10.10.240.0/23 | 240 | INSIDE | VOIP |
| 10.10.242.0/24 | 242 | INSIDE | WIFI-MGMT |
| 10.10.254.0/30 | n/a | INSIDE | CORE-SW-1 to FIREWALL-1 |
| 10.10.254.4/30 | n/a | INSIDE | CORE-SW-2 to FIREWALL-1 |

**The public IP addresses are for illustrative purposes only and do not correspond to any real infrastructure.**