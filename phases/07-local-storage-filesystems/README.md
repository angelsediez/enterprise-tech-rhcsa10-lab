# 💾 Phase 07 - Local Storage and Filesystems

## 🎯 Objective
Establish and validate a controlled Phase 07 baseline for local storage and filesystem management across the guest set.

This phase focuses on documenting block device inspection, partition creation, filesystem identification, temporary and persistent mount validation, and the capture of structured storage evidence for reproducible administration.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Storage Baseline:** Inspecting current block devices, partitions, and active mount points.
* **Filesystem Evidence:** Capturing `lsblk`, `blkid`, `df`, and mount states into persistent files.
* **Controlled Filesystem Workflow:** Creating and validating a dedicated XFS filesystem on the lab disk attached to `srv-admin`.
* **Mount Validation:** Testing both temporary and persistent mount points via `/etc/fstab`.
* **Persistence:** Ensuring mount configuration is valid through `mount -a`.
* **Evidence:** Capturing screenshots and command validation results.

### **Excluded (Later Phases):**
* Network-attached storage (`NFS`, `autofs`).
* Advanced LVM workflows beyond discovery.
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
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1. **Full Reference Workflow:** Execute the complete storage inspection and filesystem workflow on `srv-admin`.
2. **Validation Pattern:** Apply a condensed inspection-only validation block on `srv-web`, `srv-db`, and `srv-storage`.
3. **Evidence:** Capture screenshots of device hierarchy, filesystem identification, mount state, and persistent mount validation.
4. **Closure:** Confirm that the validated Phase 07 baseline exists across all four guests.

---

## 📐 Workspace Design
Phase 07 operations are isolated within a dedicated directory structure:

```text
~/lab/f07-local-storage-filesystems/
├── devices/      # Block device and partition evidence
├── filesystems/  # Filesystem creation and identification evidence
├── mounts/       # Mount state and fstab validation exports
└── tmp/          # Temporary testing area
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-admin` | Reference Node | ✅ Completed & Validated |
| `srv-web` | Web Service | ✅ Validation Pattern Completed |
| `srv-db` | Database Node | ✅ Validation Pattern Completed |
| `srv-storage` | Storage Node | ✅ Validation Pattern Completed |

### **Phase 07 Achievements**
* [x] Phase 07 workspace created on `srv-admin`.
* [x] Local storage baseline validated and exported to `devices/` and `mounts/`.
* [x] Block device and filesystem UUID evidence captured.
* [x] Dedicated lab disk identified as `vdb` on `srv-admin`.
* [x] GPT partition table and `vdb1` partition created successfully.
* [x] Controlled XFS filesystem created and validated on `srv-admin`.
* [x] Temporary mount validation completed on `/mnt/phase07-demo`.
* [x] Persistent mount configuration added and validated through `/etc/fstab`.
* [x] Validation pattern replicated on `srv-web`, `srv-db`, and `srv-storage`.
* [x] Screenshot evidence captured for all planned validation points.

> [!NOTE]
> The secondary guests used the inspection-only validation pattern. No extra disk was attached there, and no `mkfs` or `/etc/fstab` changes were applied on `srv-web`, `srv-db`, or `srv-storage`.

---

## 🧪 srv-admin Validation Areas

### 1. Storage Baseline
```bash
lsblk
sudo fdisk -l
blkid
df -hT
mount | head -n 20
```

### 2. Storage Evidence Capture
```bash
lsblk > devices/lsblk.txt
blkid > devices/blkid.txt
df -hT > mounts/df-ht.txt
mount > mounts/mounts.txt
find devices mounts -maxdepth 1 -type f | sort
```

### 3. Dedicated Lab Disk Preparation
On `srv-admin`, a secondary disk was added for the lab workflow:
* `/dev/vdb` → dedicated lab disk
* `/dev/vdb1` → partition created for the Phase 07 filesystem

```bash
sudo parted /dev/vdb --script mklabel gpt
sudo parted /dev/vdb --script mkpart primary xfs 1MiB 100%
sudo partprobe /dev/vdb
lsblk
```

### 4. Controlled Filesystem Workflow
```bash
sudo mkfs.xfs /dev/vdb1
sudo blkid /dev/vdb1
```

### 5. Persistent Mount Validation
```bash
UUID=$(sudo blkid -s UUID -o value /dev/vdb1)
echo "UUID=$UUID /mnt/phase07-demo xfs defaults 0 0" | sudo tee -a /etc/fstab
sudo mount -a
findmnt /mnt/phase07-demo
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
The following condensed validation block was executed on each secondary guest:

```bash
mkdir -p ~/lab/f07-local-storage-filesystems/{devices,filesystems,mounts}
cd ~/lab/f07-local-storage-filesystems
lsblk > devices/lsblk.txt
blkid > devices/blkid.txt
df -hT > mounts/df-ht.txt
mount > mounts/mounts.txt
find ~/lab/f07-local-storage-filesystems -maxdepth 3 -type f | sort
cat devices/lsblk.txt | head -n 20
cat mounts/df-ht.txt | head -n 20
```

---

## 📸 Screenshot Inventory
Evidence stored in `assets/screenshots/phase-07/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P07-01** | Block device baseline (`lsblk`) | `srv-admin` |
| **P07-02** | Filesystem identification (`blkid`) | `srv-admin` |
| **P07-03** | Mount state and filesystem usage | `srv-admin` |
| **P07-04** | Controlled filesystem workflow | `srv-admin` |
| **P07-05** | Persistent mount validation (`mount -a`) | `srv-admin` |
| **P07-06** | Final replicated storage workspace | `srv-web` |
| **P07-07** | Final replicated storage workspace | `srv-db` |
| **P07-08** | Final replicated storage workspace | `srv-storage` |

---

## 🔗 Related Files
* `runbooks/f07-local-storage-filesystems.md` — Detailed step-by-step guide.
* `validation/07-local-storage-filesystems-checklist.md` — Phase 07 validation checklist.
* `phases/06-running-systems-service-management/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 07 is complete ✅.
A full local storage and filesystem workflow was executed and validated on `srv-admin`, including block device inspection, dedicated lab disk preparation, XFS filesystem creation, temporary mount validation, and persistent mount configuration through `/etc/fstab`.

The lab now has a documented and validated Phase 07 storage baseline across all four guests, preparing the environment for **Phase 08 — Networking and Firewall**.
