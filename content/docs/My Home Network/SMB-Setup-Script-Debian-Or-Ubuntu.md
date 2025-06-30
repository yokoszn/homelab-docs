---
title: SMB / Samba Jellyfin Quick Setup Script
tags:
  - debian
  - ubuntu
  - samba
  - bash-script
  - setup
  - media-server
  - linux
draft: "true"
---

Script I use to setup a rudimentary SMB share for [[Jellyfin]] to share a network drive from a virtual machine to [[windows]]/[[MacOS]]/[[linux]] machines on a specific subnet.

| Step                    | Detail                                                         |
| :---------------------- | :------------------------------------------------------------- |
| Install Samba           | Installs only what's needed                                    |
| Set correct permissions | Directory is `nobody:nogroup` owned for generic service access |
| Create a Samba user     | No system login allowed (`/usr/sbin/nologin`)                  |
| Stronger share config   | SMB3 minimum, no guest, encryption forced                      |
| IP restrict             | Only your LAN can access it                                    |
| File/Dir masks          | Secure default perms (0660/0770)                               |
| Backup config           | Always saves a backup of `smb.conf`                            |
**When you mount on Windows:**
```
New-PSDrive -Name "JellyfinMedia" -PSProvider FileSystem -Root "\\mediaservarr\media" -Credential (Get-Credential) -Persist
```

**Or via GUI:**
- Right-click **This PC** > **Map Network Drive**
- Enter `\\mediaservarr\media`
- Use the Samba username/password (`mediauser`)

>Save credentials if you want persistent mounting.


# Scaling to More Shares Later?

When you add new drives or nodes, you just
- Re-run this script with **different**:
    - `SHARE_NAME`
    - `SHARE_PATH`
    - (optional) `SAMBA_USER` if you want
- Each share will automatically be hardened in the same way.

Example for `/mnt/storage/movies` and `/mnt/storage/tv` would just be two runs of this with slight edits.

```
#!/bin/bash
smb-setup.sh
# Sets up a secure SMB share for Jellyfin media.

# === CONFIGURATION ===
SHARE_NAME="data"
SHARE_PATH="/mnt/storage/data"
SAMBA_USER="mediauser"
SAMBA_PASS="strongpasswordhere"  # CHANGE THIS
NETWORK_ALLOW="192.168.10.0/24"   # Adjust for your LAN
COMMENT="Jellyfin Media Share"

# === INSTALL NEEDED PACKAGES ===
apt update
apt install -y samba

# === CREATE SHARE DIRECTORY (if not exist) ===
mkdir -p "$SHARE_PATH"
chown nobody:nogroup "$SHARE_PATH"
chmod 0770 "$SHARE_PATH"

# === CREATE SAMBA USER ===
# Create system user if not exists
id "$SAMBA_USER" &>/dev/null || useradd -M -d /samba/"$SAMBA_USER" -s /usr/sbin/nologin "$SAMBA_USER"

# Set Samba password
(echo "$SAMBA_PASS"; echo "$SAMBA_PASS") | smbpasswd -s -a "$SAMBA_USER"

# === BACKUP AND CONFIGURE SAMBA ===
cp /etc/samba/smb.conf /etc/samba/smb.conf.backup.$(date +%F-%T)

cat <<EOF >> /etc/samba/smb.conf

[$SHARE_NAME]
   path = $SHARE_PATH
   comment = $COMMENT
   browseable = yes
   read only = no
   guest ok = no
   valid users = $SAMBA_USER
   force user = $SAMBA_USER
   force group = nogroup
   create mask = 0660
   directory mask = 0770
   hosts allow = $NETWORK_ALLOW
   hosts deny = 0.0.0.0/0
   server min protocol = SMB3
   smb encrypt = required
EOF

# === RESTART SAMBA ===
systemctl restart smbd

echo "✅ SMB Share [$SHARE_NAME] created successfully!"
```


> [!Important!]
> - **Firewall** (e.g., `ufw`) must allow ports `445/tcp` and `139/tcp` for SMB.
> - **Windows Defender Firewall** sometimes blocks SMB — add the network as **Private**.
> - **Keep your passwords strong**. Don't expose Samba shares over WAN.


if you want Linux user _and_ SMB user to cooperate.
```
# Create a dedicated group
groupadd mediafiles

# Add both Samba user and your Linux user to it
usermod -aG mediafiles mediaadmin
usermod -aG mediafiles mediauser

# Fix permissions on the share
chown -R root:mediafiles /mnt/storage/data
chmod -R 2770 /mnt/storage/data  # important: the "2" forces group inheritance on new files/dirs

# Restart Samba to be sure
systemctl restart smbd

```