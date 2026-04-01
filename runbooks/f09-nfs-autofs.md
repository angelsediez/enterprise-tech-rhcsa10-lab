## 🛠️ Phase 09 Runbook: NFS and autofs

## 🎯 Objective
Execute and validate a practical Phase 09 workflow focused on shared storage access using NFS and automatic mount behavior with `autofs`.

This runbook establishes a **Server & Clients** architecture where `srv-storage` acts as the central provider and `srv-admin`, `srv-web`, and `srv-db` act as the consuming client nodes.

The workflow was validated using:

- `srv-storage` as the **reference NFS server**
- `srv-admin` as the **reference client**
- `srv-web` and `srv-db` as **replicated client validations**

---

## 🔍 Scope

### **Included**
* **NFS Server:** Installation, enablement, and validation on `srv-storage`
* **Export Management:** Controlled export creation using `/etc/exports.d/`
* **Discovery:** Client-side export discovery using `showmount`
* **Manual Mount Workflow:** Validation of direct NFS mounts on client nodes
* **autofs Workflow:** Validation of on-demand automatic mounts
* **Firewall Validation:** Review of NFS-related services and listeners
* **Evidence:** Capture of export visibility, mount state, and final workspace evidence

### **Excluded**
* Kerberos-secured NFS
* Complex export permission matrices
* CIFS/SMB integration
* High-availability storage clustering

---

## 💻 Prerequisites

Before using this runbook, confirm:

| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Lab Nodes** | All 4 guests operational | `virsh list --all` |
| **Phase Baseline** | Phase 08 complete | Review Phase 08 report |
| **Working Access** | Local user with sudo rights | `sudo -v` |
| **Server IP** | `srv-storage` reachable on lab network | `ping -c 2 192.168.150.115` |

> [!IMPORTANT]
> In this phase, `srv-storage` acts as the **reference NFS server**.  
> `srv-admin`, `srv-web`, and `srv-db` act as the **client nodes**.

> [!IMPORTANT]
> The client-side validation in this phase was executed using the server IP directly: **192.168.150.115**.

---

## 📍 Primary Workspace

All work in this phase is organized under:
`~/lab/f09-nfs-autofs`

**Structure**
```text
~/lab/f09-nfs-autofs/
├── server/       # NFS server logs and export evidence
├── clients/      # Manual client mount evidence
├── autofs/       # autofs configuration and trigger evidence
└── tmp/          # Temporary testing area
```

---

## 🚀 Full Workflow on srv-storage (Reference NFS Server)

### 1) Create Workspace and Validate Context
```bash
mkdir -p ~/lab/f09-nfs-autofs/{server,clients,autofs,tmp}
cd ~/lab/f09-nfs-autofs
pwd && whoami && hostnamectl --static
find ~/lab/f09-nfs-autofs -maxdepth 2 -type d | sort
```

### 2) NFS Server Baseline
```bash
sudo dnf install -y nfs-utils
rpm -q nfs-utils
sudo systemctl enable --now rpcbind
sudo systemctl enable --now nfs-server
systemctl status rpcbind --no-pager -l
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
ls -l /srv/nfs/phase09-share
```

### 4) Configure Exports
```bash
echo '/srv/nfs/phase09-share 192.168.150.0/24(rw,sync,no_subtree_check)' | sudo tee /etc/exports.d/phase09.exports
cat /etc/exports.d/phase09.exports
sudo exportfs -rav
sudo exportfs -v
showmount -e localhost
```

### 5) Service and Firewall Validation
```bash
sudo firewall-cmd --add-service=nfs
sudo firewall-cmd --add-service=mountd
sudo firewall-cmd --add-service=rpc-bind
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-services
ss -tulpn | grep -E '2049|111'
```

### 6) Save Server Evidence
```bash
rpm -q nfs-utils > server/nfs-utils.txt
systemctl status rpcbind --no-pager -l > server/rpcbind-status.txt
systemctl status nfs-server --no-pager -l > server/nfs-server-status.txt
systemctl is-enabled nfs-server > server/nfs-server-enabled.txt
systemctl is-active nfs-server > server/nfs-server-active.txt
cat /etc/exports.d/phase09.exports > server/phase09-exports.txt
sudo exportfs -v > server/exportfs-v.txt
showmount -e localhost > server/showmount-localhost.txt
sudo firewall-cmd --list-services > server/firewall-services.txt
find ~/lab/f09-nfs-autofs -maxdepth 3 -type f | sort
```

---

## 🔁 Reference Client Workflow on srv-admin

### 1) Discovery and Manual Mount Validation
```bash
showmount -e 192.168.150.115
sudo mkdir -p /mnt/phase09-nfs
sudo mount -t nfs 192.168.150.115:/srv/nfs/phase09-share /mnt/phase09-nfs
ls -la /mnt/phase09-nfs
cat /mnt/phase09-nfs/README.txt
mount | grep phase09-nfs
```

### 2) Save Manual Client Evidence
```bash
showmount -e 192.168.150.115 > clients/showmount-srv-storage.txt
mount | grep phase09-nfs > clients/mount-phase09-nfs.txt
ls -la /mnt/phase09-nfs > clients/ls-phase09-nfs.txt
cat /mnt/phase09-nfs/README.txt > clients/readme-phase09-nfs.txt
find clients -maxdepth 1 -type f | sort
```

### 3) Configure autofs
```bash
sudo tee /etc/auto.master.d/phase09.autofs > /dev/null <<'EOF'
/- /etc/auto.phase09
EOF

sudo tee /etc/auto.phase09 > /dev/null <<'EOF'
/mnt/phase09-auto -fstype=nfs,rw,sync 192.168.150.115:/srv/nfs/phase09-share
EOF

cat /etc/auto.master.d/phase09.autofs
cat /etc/auto.phase09
sudo systemctl enable --now autofs
systemctl status autofs --no-pager -l
```

### 4) Validate autofs Trigger
```bash
ls /mnt/phase09-auto
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto
```

### 5) Save autofs Evidence
```bash
rpm -q autofs > autofs/autofs-package.txt
systemctl status autofs --no-pager -l > autofs/autofs-status.txt
cat /etc/auto.master.d/phase09.autofs > autofs/auto-master-phase09.txt
cat /etc/auto.phase09 > autofs/auto-phase09.txt
mount | grep phase09-auto > autofs/mount-phase09-auto.txt
find autofs -maxdepth 1 -type f | sort
```

---

## 🔁 Replicated Client Validation Pattern (srv-web, srv-db)

### 1) Manual NFS and autofs Validation
```bash
showmount -e 192.168.150.115
sudo mkdir -p /mnt/phase09-nfs
sudo mount -t nfs 192.168.150.115:/srv/nfs/phase09-share /mnt/phase09-nfs
cat /mnt/phase09-nfs/README.txt

sudo tee /etc/auto.master.d/phase09.autofs > /dev/null <<'EOF'
/- /etc/auto.phase09
EOF

sudo tee /etc/auto.phase09 > /dev/null <<'EOF'
/mnt/phase09-auto -fstype=nfs,rw,sync 192.168.150.115:/srv/nfs/phase09-share
EOF

sudo systemctl enable --now autofs
ls /mnt/phase09-auto
cat /mnt/phase09-auto/README.txt
```

---

## 🧪 Validation Targets

### **srv-storage**
- [x] nfs-utils installed
- [x] nfs-server active and enabled
- [x] export visible with showmount -e localhost
- [x] firewall allows nfs, mountd, and rpc-bind

### **Clients (srv-admin, srv-web, srv-db)**
- [x] export visible with showmount -e 192.168.150.115
- [x] manual NFS mount validated
- [x] autofs on-demand mount trigger validated
- [x] evidence saved to workspace

---

## 🧯 Short Troubleshooting
1) **showmount command missing:** Install `nfs-utils`.
2) **Export not visible:** Check `sudo exportfs -v` on server.
3) **autofs does not trigger:** Check `systemctl status autofs`.

---

## 📸 Expected Screenshots
Store in: `assets/screenshots/phase-09/`

| ID | Description | Target |
| :--- | :--- | :--- |
| **P09-01** | NFS server baseline | `srv-storage` |
| **P09-02** | Export configuration and visibility | `srv-storage` |
| **P09-03** | NFS service and firewall validation | `srv-storage` |
| **P09-04** | Manual client mount validation | `srv-admin` |
| **P09-05** | autofs trigger validation | `srv-admin` |
| **P09-06** | Replicated client validation | `srv-web` |
| **P09-07** | Replicated client validation | `srv-db` |

---

## 🏁 Outcome
Successful execution confirms a functional shared-storage architecture across the lab.
The environment is now prepared for Phase 10: SELinux and troubleshooting.
