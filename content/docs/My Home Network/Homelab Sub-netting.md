## ⚠️ Alternatives You _Could_ Use — But Should Avoid

| Subnet           | Why to avoid                                               |
| ---------------- | ---------------------------------------------------------- |
| `10.0.0.0/24`    | Used _everywhere_ by VPNs (e.g., Meraki, AWS default VPCs) |
| `192.168.1.0/24` | Default on 99% of routers                                  |
| `192.168.0.0/24` | Default for modems and IOT hubs                            |
| `172.17.0.0/16`  | Used by **Docker bridge networks**                         |
| `100.64.0.0/10`  | Not RFC1918 (CGNAT range — avoid)                          |
## 📦 TL;DR Summary
- ✅ **172.16.x.x**: Ideal for Proxmox, LXC, DNS, Vaultwarden, Tunnels — fewer overlaps
- ✅ **10.x.x.x**: Ideal for business, VPNs, and things you might interconnect externally
- ✅ **192.168.x.x**: Still great for local LANs, IOT, guest — but keep clean and limited

If you want to lean into **10.0.0.0/8** for **everything**, that’s totally fine — it’s RFC1918 and designed for **large internal networks** (up to 16 million IPs). You just need to **apply some structure** so it remains
- 🔍 **Human-readable**
- 🧱 **Hierarchically segmented**
- 🔐 **Secure and scalable**
## 🧠 Optional: Hierarchical Encoding

For large homelabs/MSPs, you can "encode" purpose into the second octet:

| Subnet Range | Meaning             | Examples                      |
| ------------ | ------------------- | ----------------------------- |
| `10.0.x.x`   | Core Infra          | Management, DNS, NTP          |
| `10.10.x.x`  | Application Layer   | Internal apps/services        |
| `10.20.x.x`  | Access Layer        | Admin LANs, Staff Devices     |
| `10.30.x.x`  | External Access     | Cloudflared, VPN, guest Wi-Fi |
| `10.40.x.x`  | Client Networks     | Business zones                |
| `10.90.x.x`  | IOT and junk        | Untrusted or limited devices  |
| `10.99.x.x`  | Monitoring/Alerting | Wazuh, Graylog, Prometheus    |
## 🔐 Firewalling and Routing
With this all-10.0.x.x structure:
- **Inter-VLAN routing** is deny-by-default; allow only what's explicitly needed
- Group firewall policies by subnet prefix:
    - `10.0.1.0/24 → 10.0.5.0/24` = DNS allowed
    - `10.0.70.0/24 → ANY` = VPN admin full access
    - `10.0.71.0/24 → 10.0.40.0/24` = VPN guest → Internet only
Use **UniFi groups** or OPNsense aliases to apply logic efficiently.