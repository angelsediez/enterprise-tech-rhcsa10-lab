# 💾 Phase 07 - Local Storage and Filesystems

## 🎯 Objective
Establish and validate a controlled Phase 07 baseline for local storage and filesystem management across the guest set. This phase focuses on documenting the inspection of block devices, partition creation, filesystem identification, and the validation of persistent mount configurations to ensure a robust storage foundation.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Storage Baseline:** Inspecting current block devices, partitions, and active mount points.
* **Filesystem Evidence:** Capturing `lsblk`, `blkid`, `df`, and mount states into persistent files.
* **Local Filesystem Work:** Preparing a controlled storage workflow on dedicated lab block devices.
* **Mount Validation:** Testing temporary and persistent mount points via `/etc/fstab`.
* **Persistence:** Ensuring storage configuration survives reboots and `mount -a` triggers.
* **Evidence:** Capturing screenshots and command validation results.

### **Excluded (Later Phases):**
* Network-attached storage (`NFS`, `autofs`).
* Advanced LVM (Logical Volume Manager) beyond basic discovery.
* Storage performance benchmarking or tuning.
* Multi-host storage orchestration.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Local Storage and Filesystems |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation Pattern) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f07-local-storage-filesystems` |
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1.  **Full Reference Workflow:** Execute the complete storage inspection and filesystem management workflow on `srv-admin`.
2.  **Validation Pattern:** Apply a condensed validation block on `srv-web`, `srv-db`, and `srv-storage`.
3.  **Evidence:** Capture screenshots of device hierarchies, UUID identification, and successful mount persistence.
4.  **Closure:** Confirm that the validated Phase 07 baseline exists across all four guests.

---

## 📐 Workspace Design
Phase 07 operations are isolated within a dedicated directory structure:

```text
~/lab/f07-local-storage-filesystems/
├── devices/      # Block device and partition evidence
├── filesystems/  # Filesystem creation and ID evidence
├── mounts/       # Mount state and fstab validation exports
└── tmp/          # Temporary testing area
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-admin` | Reference Node | ⚪ Pending |
| `srv-web` | Web Service | ⚪ Pending |
| `srv-db` | Database Node | ⚪ Pending |
| `srv-storage` | Storage Node | ⚪ Pending |

### **Phase 07 Planned Milestones**
* [ ] Phase 07 workspace created on `srv-admin`.
* [ ] Local storage baseline validated and exported to `devices/`.
* [ ] Block device and filesystem UUID evidence captured.
* [ ] Controlled filesystem creation and mount validation on `srv-admin`.
* [ ] Persistent mount configuration (`/etc/fstab`) verified.
* [ ] Validation pattern replicated on secondary guests.
* [ ] Screenshot evidence captured for all planned points.

---

## 🧪 Planned srv-admin Validation Areas

### 1. Storage Baseline
Inspecting the physical and logical layout of the guest:
```bash
lsblk
blkid
df -hT
mount | head -n 20
```

### 2. Storage Evidence Capture
Capturing the baseline state into the workspace:
```bash
lsblk > devices/lsblk.txt
blkid > devices/blkid.txt
df -hT > mounts/df-ht.txt
mount > mounts/mounts.txt
find devices mounts -maxdepth 1 -type f | sort
```

### 3. Controlled Local Filesystem Workflow
Validating partition visibility and preparation:
```bash
sudo fdisk -l
sudo parted -l || true
```

### 4. Mount and Persistence Validation
Verifying the integrity of the filesystem table:
```bash
cat /etc/fstab
sudo mount -a
findmnt
```

---

## 📸 Planned Screenshot Inventory
Evidence stored in `assets/screenshots/phase-07/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P07-01** | Block device baseline (`lsblk`) | `srv-admin` |
| **P07-02** | Filesystem identification (`blkid`) | `srv-admin` |
| **P07-03** | Mount state and filesystem usage | `srv-admin` |
| **P07-04** | Controlled filesystem workflow (fdisk/parted) | `srv-admin` |
| **P07-05** | Persistent mount validation (`mount -a`) | `srv-admin` |
| **P07-06..08** | Final replicated storage workspace | `srv-web/db/storage` |

---

## 🔗 Related Files
* `runbooks/f07-local-storage-filesystems.md` — Detailed step-by-step guide.
* `phases/06-running-systems-service-management/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 07 is **Not Started** ⚪.

The lab is currently awaiting the execution of the storage and filesystem management workflow to establish a manageable data persistence baseline across all nodes.
