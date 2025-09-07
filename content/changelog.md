---
title: HomeLab Changelog
---

# HomeLab Changelog

Track updates, lab verifications, and new content additions.

## 2025-09-07 - Site Restructure

### Added
- **New Homepage Design**: Clear hero section with "hands-on labs" positioning
- **Labs Directory**: Organized lab structure at `/labs` with categories:
  - Networking (VPN, Reverse Proxy, Firewalls)
  - Virtualization (Proxmox, Docker, LXC)
  - Security (Identity, SIEM/SOAR)
  - Self-Hosting (Media, Productivity, Dev Tools)
- **Standardized Lab Format**: Template with objectives, requirements, steps, validation
- **Docker Installation Lab**: Complete beginner-friendly container setup guide
- **Community Integration**: TWN Commons callouts in all labs
- **Lab Template**: Standardized format for future lab development

### Changed
- Homepage transformed from "raw notes journal" to structured lab guide
- Draft content properly marked to hide incomplete pages
- ITLearn integration messaging throughout

### Technical
- Fixed draft frontmatter format (`draft: true` vs `draft: "true"`)
- Added proper frontmatter to empty content files
- Created labs directory structure
- Established changelog for version tracking

---

## Lab Status Legend

- ✅ **Complete**: Fully tested and documented
- 🚧 **In Progress**: Content being developed  
- 📋 **Planned**: Scheduled for creation
- 🔄 **Needs Update**: Requires verification/refresh

---

## Recent Lab Verifications

| Lab | Last Verified | Status | Notes |
|-----|---------------|--------|-------|
| Docker Installation | 2025-09-07 | ✅ | Tested on Ubuntu 22.04 LTS |

---

## Planned Labs

### Priority Queue
1. **Proxmox VE Setup Lab** - Virtualization platform deployment
2. **Reverse Proxy with SSL Lab** - Consolidate NPM/Traefik/HAProxy guides  
3. **Nextcloud File Sync Lab** - Private cloud storage
4. **Jellyfin Media Server Lab** - Open-source streaming
5. **Immich Photo Gallery Lab** - Self-hosted photo management

### Community Requests
*Submit lab requests in [TWN Commons Discord](https://discord.gg/kgaMm6WJya)*

---

## Contributing

Want to help improve HomeLab?

1. **Report Issues**: Found outdated instructions? Let us know in Commons
2. **Lab Verification**: Test existing labs and report results
3. **New Labs**: Use our [lab template](/templates/lab-template) for contributions
4. **Documentation**: Help improve clarity and completeness

---

*This changelog follows [Keep a Changelog](https://keepachangelog.com/) principles.*