# 📂 Phase 09 - NFS and autofs

## 🎯 Objective
Establish and validate a controlled Phase 09 baseline for shared storage access using NFS and automatic mount behavior with `autofs` across the guest set.

This phase focused on configuring `srv-storage` as the reference NFS server, exporting a controlled shared directory, validating manual client-side NFS mounts, and confirming on-demand automount behavior through `autofs` on the client nodes.

This phase used **`srv-storage` as the full reference workflow** and applied a **client validation pattern** to `srv-admin`, `srv-web`, and `srv-db`.

---

## 🔍 Scope

### **Included in this phase:**
* **NFS Server Baseline:** Installation and validation of `nfs-utils` on `srv-storage`.
* **Export Management:** Controlled NFS export configuration for the internal lab network.
* **Firewall Awareness:** Validation of NFS-related firewall services and listeners.
* **Client Validation:** Manual NFS access checks from `srv-admin`, `srv-web`, and `srv-db`.
* **autofs Workflow:** Configuration and validation of automatic NFS mounts on client nodes.
* **Evidence:** Capturing screenshots, export visibility, mount states, and workspace evidence.

### **Excluded (Later Phases):**
* Kerberos-secured NFS.
* Advanced export permission models.
* CIFS/SMB shares.
* High-availability storage clustering.
* Storage quota management.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | NFS and autofs |
| **Primary Guest** | `srv-storage` (Reference NFS Server) |
| **Client Guests** | `srv-admin`, `srv-web`, `srv-db` |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f09-nfs-autofs` |
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase followed a **Server & Clients** model:

1. **Reference Workflow on `srv-storage`:** Install and validate the NFS server role.
2. **Export Validation:** Confirm export visibility and correctness from the server side.
3. **Client Validation Pattern:** Validate manual NFS access from `srv-admin`, `srv-web`, and `srv-db`.
4. **autofs Validation:** Confirm on-demand mount behavior from the client nodes.
5. **Closure:** Confirm that the validated Phase 09 shared-storage baseline exists across the lab.

---

## 📐 Workspace Design
Phase 09 operations were isolated within a dedicated directory structure:

```text
~/lab/f09-nfs-autofs/
├── server/       # Export and NFS server evidence
├── clients/      # Client-side mount and access evidence
├── autofs/       # autofs map and validation evidence
└── tmp/          # Temporary testing area
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-storage` | Reference NFS Server | ✅ Complete |
| `srv-admin` | Client Node | ✅ Complete |
| `srv-web` | Client Node | ✅ Complete |
| `srv-db` | Client Node | ✅ Complete |

### **Phase 09 Completed Milestones**
* [x] Phase 09 workspace created on `srv-storage`.
* [x] NFS server baseline (`nfs-utils`) validated on `srv-storage`.
* [x] Controlled NFS export created and validated.
* [x] Firewall rules for NFS-related services applied and verified.
* [x] Client-side manual NFS access validated on `srv-admin`.
* [x] `autofs` configured and validated on `srv-admin`.
* [x] Replicated client validation completed on `srv-web` and `srv-db`.
* [x] Screenshot evidence captured for all planned validation points.

---

## 🧪 Completed srv-storage Validation Areas

### 1. NFS Server Baseline
The reference server validated:
```bash
rpm -q nfs-utils
systemctl status rpcbind --no-pager -l
systemctl status nfs-server --no-pager -l
systemctl is-enabled nfs-server
systemctl is-active nfs-server
```

### 2. Export Configuration
A controlled export was created for the internal lab subnet:
```bash
cat /etc/exports.d/phase09.exports
sudo exportfs -rav
sudo exportfs -v
showmount -e localhost
```

Validated export:
`/srv/nfs/phase09-share 192.168.150.0/24(rw,sync,no_subtree_check)`

### 3. Service and Firewall Validation
The server validated NFS-related firewall services and listeners:
```bash
sudo firewall-cmd --list-services
ss -tulpn | grep -E '2049|111'
```

---

## 🧪 Completed Client Validation Areas

### 1. Manual NFS Client Access
Client-side validation was completed against the NFS server IP:
* **NFS server:** 192.168.150.115
* **Export path:** /srv/nfs/phase09-share

Validated workflow:
```bash
showmount -e 192.168.150.115
sudo mkdir -p /mnt/phase09-nfs
sudo mount -t nfs 192.168.150.115:/srv/nfs/phase09-share /mnt/phase09-nfs
ls -la /mnt/phase09-nfs
cat /mnt/phase09-nfs/README.txt
mount | grep phase09-nfs
```

### 2. autofs Validation
On-demand mount behavior was validated using a direct map:
```bash
cat /etc/auto.master.d/phase09.autofs
cat /etc/auto.phase09
systemctl status autofs --no-pager -l
ls /mnt/phase09-auto
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto
```

Validated automount target:
`/mnt/phase09-auto`

### 3. Replicated Client Validation
The client validation pattern was successfully replicated on:
* **srv-web**
* **srv-db**

---

## 📸 Screenshot Inventory
Evidence stored in assets/screenshots/phase-09/:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P09-01** | NFS server baseline | `srv-storage` |
| **P09-02** | Export configuration and visibility | `srv-storage` |
| **P09-03** | NFS service and firewall validation | `srv-storage` |
| **P09-04** | Client manual mount validation | `srv-admin` |
| **P09-05** | autofs on-demand mount validation | `srv-admin` |
| **P09-06** | Replicated client validation | `srv-web` |
| **P09-07** | Replicated client validation | `srv-db` |

---

## 🔗 Related Files
* `runbooks/f09-nfs-autofs.md` — Detailed Phase 09 operational runbook.
* `phases/08-networking-firewall/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 09 is complete ✅.
The lab now has a validated shared-storage baseline built around:
- A working NFS server on `srv-storage`.
- A controlled export for the `192.168.150.0/24` lab network.
- Validated manual NFS access and `autofs` on-demand mounts across all client nodes.
