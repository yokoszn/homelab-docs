---
title: Should I use Multiple Reverse Proxies or Tunnels?
tags:
  - self-hosted
  - article
  - AI-Response
---


> [!NOTE]
> Response from Mr GPT (4o) regarding usage of one or more reverse proxies, who it would be useful for, when it is not worthwhile. Take with a grain of salt and confirm accuracy!

## **Should You Have Multiple Tunnel VLANs?**

### 🧠 **Usually, one TUNNEL VLAN is enough**, _unless_:

| Case                                                                               | Should You Split Tunnel VLANs?       |
| ---------------------------------------------------------------------------------- | ------------------------------------ |
| You want **separate tunnels** for **business** vs **homelab** environments         | ✅ Yes — better logging, blast radius |
| You want to **strictly isolate** routing paths (e.g. different DNS, IP sets, etc.) | ✅ Yes — clearer routing ACLs         |
| You’re fine with a single outbound path that connects all external services        | ❌ No — one is fine (simpler)         |
### Recommended:

| Tunnel Purpose      | VLAN ID | Subnet         |
| ------------------- | ------- | -------------- |
| `TUNNEL-HOMELAB`    | 50      | `10.0.50.0/28` |
| `TUNNEL-BIZ` (opt.) | 51      | `10.0.51.0/28` |
To properly and securely allow a **reverse proxy in one subnet** to access a **server in another subnet**, follow these **best-practice steps**. This setup is common when isolating services for security, scalability, or performance (e.g., a reverse proxy in a DMZ or dedicated frontend VLAN accessing backend services in a trusted VLAN).

---
## 🔧 Network Setup Example
- **Reverse Proxy** (e.g., NGINX, Traefik, HAProxy): `10.0.1.10` on `VLAN 11 (10.0.1.0/24)`
- **Backend Server** (e.g., web app): `10.0.2.20` on `VLAN 12 (10.0.2.0/24)`
- **Router/firewall**: Handles inter-VLAN routing and firewall rules

---