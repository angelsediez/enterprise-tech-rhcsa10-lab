# 🛠️ Phase 09 Runbook: NFS and autofs

## 🎯 Objective
Execute and validate a practical Phase 09 workflow focused on shared storage access using **Network File System (NFS)** and automatic mount behavior with **`autofs`**. 

This runbook establishes a **Server & Clients** architecture where `srv-storage` acts as the central provider and the rest of the nodes act as on-demand consumers.

---

## 🔍 Scope

### **Included**
* **NFS Server:** Inspection, enablement, and hardening on `srv-storage`.
* **Export Management:** Controlled creation of exports using modern `/etc/exports.d/` structures.
* **Discovery:** Client-side export discovery using `showmount`.
* **Mount Workflows:** Validation of both manual mounts and automated `autofs` triggers.
* **Security:** Firewall and listener inspection (Ports 2049, 111).
* **Evidence:** Systematic capture of export tables and mount states.

### **Excluded**
* Kerberos-secured NFS (RPCSEC_GSS).
* Complex multi-node export permission matrices.
* CIFS/SMB (Samba) integration.
* High Availability (HA) storage clustering.

---

## 💻 Prerequisites

Before using this runbook, confirm:
| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Lab Nodes** | All 4 VMs operational | `virsh list --all` |
| **Connectivity** | Internal network functional | `ping srv-storage` |
| **Baseline** | Phase 08 Complete | Review Phase 08 report |
| **Permissions** | Sudo access for working user | `sudo -v` |

> [!IMPORTANT]
> En esta fase, `srv-storage` actúa como el **Servidor de Referencia**. Los nodos `srv-admin`, `srv-web` y `srv-db` actúan como los **Clientes**. Keep all NFS exports controlled and documented.

---

## 📍 Primary Workspace

All work in this phase is organized under:
`~/lab/f09-nfs-autofs`

**Structure:**
```text
~/lab/f09-nfs-autofs/
├── server/       # NFS Server logs and config exports
├── clients/      # Manual mount discovery evidence
├── autofs/       # Autofs maps and trigger logs
└── tmp/          # Temporary testing area
```

---

## 🚀 Full Workflow on srv-storage (NFS Server)

### 1) Create Workspace and Validate Context
```bash
mkdir -p ~/lab/f09-nfs-autofs/{server,clients,autofs,tmp}
cd ~/lab/f09-nfs-autofs
pwd && whoami && hostnamectl --static
find ~/lab/f09-nfs-autofs -maxdepth 2 -type d | sort
```

### 2) NFS Server Baseline
```bash
rpm -q nfs-utils
systemctl status nfs-server --no-pager -l
systemctl is-enabled nfs-server
systemctl is-active nfs-server
```

### 3) Prepare Export Directory
```bash
sudo mkdir -p /srv/nfs/phase09-share
echo "phase09 nfs export from $(hostnamectl --static)" | sudo tee /srv/nfs/phase09-share/README.txt
sudo chown -R root:root /srv/nfs/phase09-share
sudo chmod 755 /srv/nfs/phase09-share
ls -ld /srv/nfs/phase09-share
```

### 4) Configure Exports
```bash
echo '/srv/nfs/phase09-share 192.168.150.0/24(rw,sync,no_subtree_check)' | sudo tee /etc/exports.d/phase09.exports
sudo exportfs -rav
exportfs -v
showmount -e localhost
```

### 5) Service and Firewall Validation
```bash
sudo firewall-cmd --add-service={nfs,mountd,rpc-bind}
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-services
ss -tulpn | grep -E '2049|111'
```

### 6) Save Server Evidence
```bash
rpm -q nfs-utils > server/nfs-utils.txt
systemctl status nfs-server --no-pager -l > server/nfs-server-status.txt
exportfs -v > server/exportfs-v.txt
showmount -e localhost > server/showmount-localhost.txt
sudo firewall-cmd --list-services > server/firewall-services.txt
find server -maxdepth 1 -type f | sort
```

---

## 🔁 Client Validation Pattern (srv-admin, srv-web, srv-db)

### 1) Discovery and Manual Mount
```bash
mkdir -p ~/lab/f09-nfs-autofs/{clients,autofs,tmp}
cd ~/lab/f09-nfs-autofs
showmount -e srv-storage
sudo mkdir -p /mnt/phase09-nfs
sudo mount -t nfs srv-storage:/srv/nfs/phase09-share /mnt/phase09-nfs
cat /mnt/phase09-nfs/README.txt
mount | grep phase09-nfs > clients/mount-manual.txt
```

### 2) autofs Configuration
```bash
# Configure Master Map and Direct Map
echo '/- /etc/auto.phase09' | sudo tee -a /etc/auto.master.d/phase09.autofs
echo '/mnt/phase09-auto -fstype=nfs,rw,sync srv-storage:/srv/nfs/phase09-share' | sudo tee /etc/auto.phase09
sudo systemctl enable --now autofs
```

### 3) Validate autofs Trigger
```bash
# On-demand trigger validation
ls /mnt/phase09-auto
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto > autofs/mount-trigger.txt
```

### 4) Save Client Evidence
```bash
rpm -q autofs > autofs/package.txt
cat /etc/auto.phase09 > autofs/map-file.txt
find clients autofs -maxdepth 1 -type f | sort
```

---

## 🧪 Validation Targets

### **srv-storage (Server)**
- [ ] `nfs-server` service is active and enabled.
- [ ] Export directory `/srv/nfs/phase09-share` has correct permissions.
- [ ] `exportfs -v` confirms the network mask `192.168.150.0/24`.
- [ ] Firewall allows `nfs`, `mountd`, and `rpc-bind`.

### **Clients (srv-admin, srv-web, srv-db)**
- [ ] `showmount -e srv-storage` lists the export successfully.
- [ ] Manual mount is successful and readable.
- [ ] `autofs` service is active and enabled.
- [ ] On-demand mount triggers upon accessing `/mnt/phase09-auto`.

---

## 🧯 Short Troubleshooting

1. **Mount Access Denied:** Confirm the client IP is within the mask and `exportfs -rav` was executed.
2. **`showmount` RPC Failure:** Ensure `rpc-bind` is allowed in the server's firewall.
3. **Autofs Not Triggering:** Check `/var/log/messages`. Confirm the path in `/etc/auto.phase09` matches the real export.

---

## 📸 Expected Screenshots
Store in: `assets/screenshots/phase-09/`

| ID | Description | Target |
| :--- | :--- | :--- |
| **P09-01** | NFS Server Baseline & Status | `srv-storage` |
| **P09-02** | Export Visibility (`showmount`) | `srv-storage` |
| **P09-04** | Manual Client Mount Validation | `srv-admin` |
| **P09-05** | Autofs Trigger & Read Validation | `srv-admin` |
| **P09-06..07**| Replicated Client Workspaces | `srv-web/db` |

---

## 🏁 Outcome
Successful execution confirms a functional shared-storage architecture. El entorno está listo para **Phase 10: SELinux and Troubleshooting**.
