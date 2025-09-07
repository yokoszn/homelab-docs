---
title: HomeLab Labs Directory
---

# HomeLab Labs

Hands-on guides for building your own infrastructure and practicing IT skills. Each lab includes clear objectives, requirements, step-by-step instructions, and validation steps.

## Networking

Master network infrastructure and security fundamentals.

### VPN & Remote Access
- **[OpenVPN Setup Lab](/labs/networking/openvpn)** - Deploy a self-hosted VPN server
- **[WireGuard Configuration Lab](/labs/networking/wireguard)** - Modern VPN with minimal configuration

### Reverse Proxy & SSL
- **[Reverse Proxy with SSL Lab](/labs/networking/reverse-proxy-ssl)** - Secure web services with automated certificates
- **[Nginx Proxy Manager Lab](/labs/networking/nginx-proxy-manager)** - Web-based proxy management

### Firewalls & Security
- **[pfSense Firewall Lab](/labs/networking/pfsense)** - Enterprise-grade firewall configuration
- **[OPNsense Setup Lab](/labs/networking/opnsense)** - Open-source firewall deployment

## Virtualization

Learn containerization and virtualization technologies.

### Hypervisors
- **[Proxmox VE Setup Lab](/labs/virtualization/proxmox-ve)** - Full virtualization platform deployment
- **[LXC Container Management Lab](/labs/virtualization/lxc-containers)** - Lightweight Linux containers

### Container Platforms
- **[Docker Installation Lab](/labs/virtualization/docker-install)** - Container runtime setup on Ubuntu/Debian
- **[Docker Compose Deployment Lab](/labs/virtualization/docker-compose)** - Multi-container application orchestration

## Security

Implement defense-in-depth security strategies.

### Identity & Access Management
- **[FreeIPA Identity Provider Lab](/labs/security/freeipa)** - Centralized authentication and authorization
- **[Single Sign-On (SSO) Lab](/labs/security/sso)** - SAML and OpenID Connect integration

### Monitoring & Detection
- **[SIEM with Wazuh Lab](/labs/security/wazuh-siem)** - Security information and event management
- **[Intrusion Detection Lab](/labs/security/suricata)** - Network-based threat detection

## Self-Hosting

Replace cloud services with self-hosted alternatives.

### Media & Entertainment
- **[Jellyfin Media Server Lab](/labs/self-hosting/jellyfin)** - Open-source media streaming
- **[Immich Photo Gallery Lab](/labs/self-hosting/immich)** - Self-hosted photo management

### Productivity & Collaboration
- **[Nextcloud File Sync Lab](/labs/self-hosting/nextcloud)** - Private cloud storage and collaboration
- **[Vaultwarden Password Manager Lab](/labs/self-hosting/vaultwarden)** - Self-hosted Bitwarden compatible server

### Development Tools
- **[Gitea Git Server Lab](/labs/self-hosting/gitea)** - Lightweight self-hosted Git service
- **[Code Server Lab](/labs/self-hosting/code-server)** - VS Code in the browser

---

## Before You Start

### Prerequisites
- Basic Linux command line knowledge
- Understanding of networking concepts (IP addresses, ports, DNS)
- A lab environment (virtual machines, containers, or spare hardware)

### Lab Environment Setup
Most labs assume you have:
- Ubuntu/Debian-based systems (adapts to other distros with package manager changes)
- Root or sudo access
- Internet connectivity for package downloads
- Basic text editor familiarity (nano, vim, or VS Code)

### Getting Help
Stuck on a lab? Don't give up! The TWN community is here to help:

> [!HELP]
> **Need Support?**
> 
> Join the [TWN Commons Discord](https://discord.gg/kgaMm6WJya) to:
> - Share screenshots of errors
> - Get troubleshooting help
> - Discuss lab variations and improvements
> - Connect with other homelab enthusiasts

### Lab Status Legend
- ✅ **Complete**: Fully tested and documented
- 🚧 **In Progress**: Content being developed
- 📋 **Planned**: Scheduled for creation

---

*Want to contribute a lab? Check out our [lab template](/templates/lab-template) and submit a pull request!*