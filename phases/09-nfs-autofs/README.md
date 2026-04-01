# 📂 Phase 09 - NFS and Autofs

## 🎯 Objective
Establish and validate a controlled Phase 09 baseline for shared storage using **Network File System (NFS)** and **Autofs**.

This phase focuses on configuring `srv-storage` as a central NFS server, exporting controlled directories, and managing on-demand mounting on client nodes (`srv-admin`, `srv-web`, `srv-db`) via `autofs`. This ensures efficient resource sharing and automated mount lifecycle management.

This phase begins with the **server-side configuration on `srv-storage`** and follows with a **replicated validation pattern** across the client set.

---

## 🔍 Scope

### **Included in this phase:**
* **NFS Server Baseline:** Installation and service hardening of `nfs-utils` on `srv-storage`.
* **Export Management:** Configuration of `/etc/exports` with controlled sync and permission parameters.
* **Firewall Awareness:** Opening necessary RPC and NFS ports on the server side.
* **NFS Client Access:** Manual mount validation and permission testing from client nodes.
* **Autofs Implementation:** Automating mounts via master and direct/indirect maps.
* **Evidence:** Capturing export visibility (`showmount`), mount states, and service logs.

### **Excluded (Later Phases):**
* NFS Kerberos authentication (RPCSEC_GSS).
* Complex multi-pathing for storage.
* High Availability (HA) NFS clusters.
* Quota management for shared storage.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | NFS Server and Autofs Clients |
| **Primary Server** | `srv-storage` (Reference NFS Server) |
| **Primary Clients** | `srv-admin`, `srv-web`, `srv-db` |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f09-nfs-autofs` |
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows a **Server & Clients** model:

1.  **Reference Workflow on `srv-storage`:** Configure and validate the NFS server role (services, exports, and firewall).
2.  **Export Validation:** Confirm export visibility and correctness from the server side using `exportfs`.
3.  **Client Validation Pattern:** Mount and validate manual access from `srv-admin`, `srv-web`, and `srv-db`.
4.  **Autofs Validation:** Implement and confirm on-demand mount behavior from client nodes.
5.  **Closure:** Confirm that the validated Phase 09 shared-storage baseline exists across the lab.

---

## 📐 Workspace Design
Phase 09 operations are isolated within a dedicated directory structure:

```text
~/lab/f09-nfs-autofs/
├── server/       # Export and NFS server evidence (srv-storage)
├── clients/      # Client-side mount and access evidence
├── autofs/       # Autofs map and validation evidence
└── tmp/          # Temporary testing area
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-storage` | Reference NFS Server | ⚪ Pending |
| `srv-admin` | Client Node | ⚪ Pending |
| `srv-web` | Client Node | ⚪ Pending |
| `srv-db` | Client Node | ⚪ Pending |

### **Phase 09 Planned Milestones**
* [ ] Phase 09 workspace created on `srv-storage`.
* [ ] NFS server baseline (`nfs-utils`) validated on `srv-storage`.
* [ ] Controlled `/etc/exports` configuration created and validated.
* [ ] Firewall rules for NFS/RPC services applied and verified.
* [ ] Client-side NFS manual access validated on all client nodes.
* [ ] `autofs` maps configured and validated (on-demand mounting).
* [ ] Screenshot evidence captured for all planned validation points.

---

## 🧪 Planned srv-storage Validation Areas

### 1. NFS Server Baseline
```bash
rpm -q nfs-utils
systemctl status nfs-server --no-pager -l
systemctl is-enabled nfs-server
systemctl is-active nfs-server
```

### 2. Export Configuration
```bash
cat /etc/exports
sudo exportfs -rav
exportfs -v
showmount -e localhost
```

### 3. Service and Firewall Validation
```bash
systemctl status rpcbind --no-pager -l
firewall-cmd --list-services
ss -tulpn | grep -E '2049|111'
```

---

## 🧪 Planned Client Validation Areas

### 1. NFS Client Access
```bash
showmount -e srv-storage
sudo mount -t nfs srv-storage:/export/path /mnt/test-nfs
ls -la /mnt/test-nfs
```

### 2. Autofs Validation
```bash
systemctl status autofs --no-pager -l
ls /mnt/auto-share
mount | grep auto-share
```

---

## 📸 Planned Screenshot Inventory
Evidence stored in `assets/screenshots/phase-09/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P09-01** | NFS server baseline (Service/Status) | `srv-storage` |
| **P09-02** | Export configuration and visibility | `srv-storage` |
| **P09-03** | NFS service and firewall validation | `srv-storage` |
| **P09-04** | Client manual mount validation | `srv-admin` |
| **P09-05** | Autofs on-demand mount validation | `srv-admin` |
| **P09-06** | Client validation workspace | `srv-web` |
| **P09-07** | Client validation workspace | `srv-db` |

---

## 🔗 Related Files
* `runbooks/f09-nfs-autofs.md` — Detailed step-by-step guide.
* `phases/08-networking-firewall/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 09 is **Not Started** ⚪.
