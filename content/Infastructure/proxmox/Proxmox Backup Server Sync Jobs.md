> [!NOTE]
> Rough Guide from Mr GPT (4o) on Backup Server Sync Jobs, Confirm Accuracy!

---

# 🛠 Step-by-Step: PBS → PBS Sync Setup

## 1. Add the **second PBS** as a *Remote* on your first PBS
- On your **main PBS** (where the backups originate):
  - Go to **Datastore** → **Remotes** → **Add**.
  - Enter:
    - **Remote name**: e.g., `pbs-secondary`
    - **Server**: IP or DNS name of second PBS
    - **Username**: PBS user account (needs `Datastore.Sync` permission)
    - **Password/API Token**: authenticate
    - **Fingerprint**: (accept the fingerprint if prompted — security)

> 🔥 Tip: Use a **dedicated PBS user** just for syncing if you want to be clean and secure (`Role: DatastoreSync`).

---

## 2. Create a **Sync Job**  
- On your **main PBS** again:
  - Go to **Datastore** → **Sync Jobs** → **Add**.
  - Configure:
    - **Remote**: Select the remote you just added (e.g., `pbs-secondary`)
    - **Remote Datastore**: Select the datastore name on the second PBS.
    - **Local Datastore**: Where to pull the backups into.
    - **Schedule**: How often? (hourly, daily, etc.)
    - **Namespace**: (optional — if you use namespaces)
    - **Options**:
      - **Remove vanished** (optional, usually *no* for backups)
      - **Prune afterwards** (optional, depends if you want old copies gone)

Done. 🔥

---

# 📋 Quick Example

| Setting | Value |
|:--|:--|
| Remote Name | pbs-secondary |
| Server | 192.168.10.20 |
| User | syncuser@pbs |
| Remote Datastore | backups |
| Local Datastore | backups |
| Schedule | Daily at 2AM |

Result = Your first PBS **pulls** backups from the second PBS (or vice versa depending how you set it).

---

# 📦 Important Notes
- **PBS Sync is pull-based** — *main server pulls from the remote.*
- **PBS backups are de-duplicated** — syncing is efficient.
- Sync only **chunks**, not full raw VM images.
- You can **sync both ways** if you want, *but it's better to treat one PBS as "primary" and one as "replica"*.

---

# 🔥 Quick Command Line Version (if you prefer shell)

**Add a Remote** (shell):
```bash
proxmox-backup-manager remote create pbs-secondary --server 192.168.10.20:8007 --auth-id syncuser@pbs
```

**Create a Sync Job** (shell):
```bash
proxmox-backup-manager sync-job create my-sync-job --remote pbs-secondary --remote-store backups --store backups --schedule "daily"
```

---

# 🎯 TL;DR
- **Add second PBS as remote** ➔ **Create sync job** ➔ **Set schedule**.
- **1 PBS pulls backups from the other** on a schedule.
- **Efficient, encrypted, deduplicated**.

---
