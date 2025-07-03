[[My Home Network]]

https://github.com/dnburgess/vpshardening

![[My Home Network Sub-netting]]

# VM/Container IDs

| Range     | Purpose                                                                                                |     |
| --------- | ------------------------------------------------------------------------------------------------------ | --- |
| `100–199` | Core infrastructure (DNS, NTP, etc.)<br>Security stack (Vaultwarden, Wazuh<br>Reverse proxies, tunnels |     |
| `200–299` | Homelab services                                                                                       |     |
| `300–399` |                                                                                                        |     |
| `400–499` | Public/EXPOSED apps                                                                                    |     |
| `500–599` | Business apps (internal)                                                                               |     |
| `600–699` | Business DMZ                                                                                           |     |
| `700–799` | VPN services                                                                                           |     |
| `800–899` |                                                                                                        |     |
| `900–999` | Temporary/lab/testing                                                                                  |     |

# VLANs

| VLAN | Name          | Subnet         | Purpose                                 | Notes                          |
| ---- | ------------- | -------------- | --------------------------------------- | ------------------------------ |
| 1    | MGMT          | `10.0.0.0/24`  | Switches, APs, controllers, out-of-band | No Internet, VPN access only   |
| 5    | HOMELAB       | `10.0.5.0/24`  | Proxmox nodes, test infra               | Non-exposed apps, dev, lab     |
| 10   | EXPOSED       | `10.0.10.0/24` | Public-facing apps via tunnel           | No direct WAN access           |
| 15   | VAULT         | `10.0.15.0/28` | Bitwarden, Vault, secrets               | Highly restricted              |
| 20   | LAN           | `10.0.20.0/24` | Laptops, desktops, mobile devices       | Full access w/ outbound DNS    |
| 30   | IOT           | `10.0.30.0/24` | Cameras, smart TVs, Google Home, etc.   | No inter-VLAN access           |
| 40   | GUEST         | `10.0.40.0/24` | Guest Wi-Fi                             | Internet only via NAT          |
| 50   | TUNNEL        | `10.0.50.0/28` | Cloudflared, reverse proxy, auth layer  | Only inbound to EXPOSED/VAULT  |
| 60   | BUSINESS-DMZ  | `10.0.60.0/24` | Public-facing biz apps/APIs             | Reverse proxy target zone      |
| 61   | BUSINESS-SERV | `10.0.61.0/24` | Internal-only business services         | No WAN unless whitelisted      |
| 70   | VPN-ADMIN     | `10.0.70.0/24` | Admin VPN clients                       | Can access all zones           |
| 71   | VPN-GUEST     | `10.0.71.0/24` | Guest VPN clients                       | Internet + optional LAN access |
| 72   | VPN-BIZ       | `10.0.72.0/24` | Business site-to-site or remote access  | Access to biz zones only       |


# Network Zones

| Zone Group        | VLANs                        | Notes                                    |
| ----------------- | ---------------------------- | ---------------------------------------- |
| `Zone_Infra`      | 1 (MGMT), 5 (HOMELAB)        | Proxmox, DNS, logging, NTP, etc.         |
| `Zone_Public`     | 10 (EXPOSED), 60 (DMZ)       | Accessible from TUNNEL only              |
| `Zone_Private`    | 20 (LAN), 61 (BUSINESS-SERV) | Workstations, Laptops, Biz apps          |
| `Zone_Restricted` | 15 (VAULT), 30 (IOT)         | Minimal access, logging required         |
| `Zone_VPN`        | 70–72                        | VPN routes (mapped based on user intent) |
|                   |                              |                                          |

# Reverse Proxies

![[Pangolin Compared to NPM+]]

## 🔥 UniFi Firewall Rules (Sample Logic)

### 🔐 Deny by Default