# 🛠️ Phase 07 Runbook: Local Storage and Filesystems

## 🎯 Objective
Execute and validate a practical Phase 07 workflow focused on local storage and filesystem management. This runbook documents the transition from raw block devices to persistent, mounted filesystems.

**Key Goals:**
- Local block device inspection.
- Filesystem identification and metadata capture.
- Controlled **XFS** filesystem creation on a dedicated lab disk (`vdb`).
- Temporary and persistent mount validation through `/etc/fstab`.
- Standardized storage evidence capture across the entire guest set.

This runbook uses `srv-admin` as the **full reference workflow** and applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included**
* **Device Inspection:** Guest-side analysis of block devices and partitions.
* **Metadata Validation:** Using `lsblk`, `blkid`, `df`, and `mount` to identify storage attributes.
* **Filesystem Creation:** Building a controlled XFS filesystem on the dedicated lab disk in `srv-admin`.
* **Mount Operations:** Testing both temporary and persistent mounting.
* **Persistence Check:** Validating that storage configuration survives `mount -a`.
* **Evidence Storage:** Capturing storage states into the Phase 07 workspace.

### **Excluded**
* Network-based storage (`NFS`, `CIFS`).
* Advanced LVM workflows (unless added in later phases).
* Disk performance benchmarking or I/O tuning.
* Multi-host storage clustering.

---

## 💻 Prerequisites

Before using this runbook, confirm:

| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phase 06 complete | Review `phases/06-running-systems-service-management/README.md` |
| **Guest Availability** | All four guests operational | `virsh list --all` |
| **Disk Target** | Safe lab disk identified | `lsblk` and `sudo fdisk -l` |
| **Access Control** | Working user has sudo access | `sudo -v` |

> [!IMPORTANT]
> **CRITICAL:** Do **not** format or repartition the active root disk. In this lab, the controlled writable target used on **srv-admin** is the dedicated secondary disk **vdb**, with the filesystem created on **vdb1**.

---

## 📍 Primary Workspace

All work in this phase is performed under:
`~/lab/f07-local-storage-filesystems`

**Expected Structure:**
```text
~/lab/f07-local-storage-filesystems/
├── devices/      # Block device and partition logs
├── filesystems/  # Filesystem ID and metadata
├── mounts/       # df, mount, and fstab logs
└── tmp/          # Temporary testing area
```

---

## 🚀 Full Workflow on srv-admin

### 1) Create Workspace and Validate Context
```bash
mkdir -p ~/lab/f07-local-storage-filesystems/{devices,filesystems,mounts,tmp}
cd ~/lab/f07-local-storage-filesystems
pwd && whoami && hostnamectl --static
find ~/lab/f07-local-storage-filesystems -maxdepth 2 -type d | sort
```

### 2) Storage Baseline
```bash
lsblk
sudo fdisk -l
blkid
df -hT
mount | head -n 20
```

### 3) Save Storage Evidence
```bash
lsblk > devices/lsblk.txt
blkid > devices/blkid.txt
df -hT > mounts/df-ht.txt
mount > mounts/mounts.txt
find devices mounts -maxdepth 1 -type f | sort
```

### 4) Inspect Writable Storage Target
In this lab, the dedicated secondary disk for **srv-admin** is:
- **/dev/vdb** → New raw lab disk.
- **/dev/vdb1** → Partition created for the Phase 07 filesystem.

```bash
sudo fdisk -l /dev/vdb
sudo parted -l || true
```

### 5) Create GPT Partition on the Lab Disk
```bash
sudo parted /dev/vdb --script mklabel gpt
sudo parted /dev/vdb --script mkpart primary xfs 1MiB 100%
sudo partprobe /dev/vdb
lsblk /dev/vdb
```

### 6) Controlled Filesystem Workflow
```bash
sudo mkfs.xfs /dev/vdb1
sudo blkid /dev/vdb1
```

### 7) Temporary Mount Validation
```bash
sudo mkdir -p /mnt/phase07-demo
sudo mount /dev/vdb1 /mnt/phase07-demo
df -hT /mnt/phase07-demo
mount | grep phase07-demo
```

### 8) Persistent Mount Preparation
```bash
# Extract UUID automatically
UUID=$(sudo blkid -s UUID -o value /dev/vdb1)
echo "Extracted UUID: $UUID"

# Append to /etc/fstab
echo "UUID=$UUID /mnt/phase07-demo xfs defaults 0 0" | sudo tee -a /etc/fstab
tail -n 5 /etc/fstab

# Test fstab integrity
sudo umount /mnt/phase07-demo
sudo mount -a
findmnt /mnt/phase07-demo
```

### 9) Filesystem Evidence Capture
```bash
sudo blkid /dev/vdb1 > filesystems/phase07-blkid.txt
df -hT /mnt/phase07-demo > mounts/phase07-mounted.txt
findmnt /mnt/phase07-demo > mounts/phase07-findmnt.txt
tail -n 5 /etc/fstab > mounts/fstab-tail.txt
```

### 10) Final Phase 07 Validation
```bash
find ~/lab/f07-local-storage-filesystems -maxdepth 3 -type f | sort
cat mounts/fstab-tail.txt
cat mounts/phase07-findmnt.txt
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)

The following inspection-only block was executed on each secondary guest to document the baseline without modifying disks:

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

> [!NOTE]
> Secondary guests used the inspection-only validation pattern. No additional disks, `mkfs`, or `/etc/fstab` changes were applied to these nodes.

---

## 🧪 Validation Targets

### **srv-admin**
- [x] Workspace created successfully.
- [x] `lsblk`, `fdisk`, `blkid`, `df`, and `mount` baseline inspected.
- [x] `devices/lsblk.txt` and `devices/blkid.txt` created.
- [x] `mounts/df-ht.txt` and `mounts/mounts.txt` created.
- [x] Dedicated lab disk **vdb** identified.
- [x] Partition **vdb1** created successfully (GPT).
- [x] **XFS** filesystem created on `vdb1`.
- [x] Temporary mount validated on `/mnt/phase07-demo`.
- [x] Persistent mount added to `/etc/fstab` using UUID.
- [x] `mount -a` validation successful.
- [x] `findmnt /mnt/phase07-demo` validation successful.
- [x] Final filesystem evidence stored in workspace files.

### **Secondary Guests (srv-web, srv-db, srv-storage)**
- [x] Workspace created successfully.
- [x] `devices/lsblk.txt` created.
- [x] `devices/blkid.txt` created.
- [x] `mounts/df-ht.txt` created.
- [x] `mounts/mounts.txt` created.
- [x] Final workspace listing completed on **srv-web**.
- [x] Final workspace listing completed on **srv-db**.
- [x] Final workspace listing completed on **srv-storage**.

---

## 🧯 Short Troubleshooting

1. **Wrong device selected:** Always confirm the lab target with `lsblk` and `sudo fdisk -l` before running `mkfs`.
2. **`mount -a` fails:** Check `tail -n 5 /etc/fstab`. Confirm the UUID is exact, the mount point exists, and the filesystem type is `xfs`.
3. **Device is busy:** If `umount` fails, ensure no processes (including your shell) are using `/mnt/phase07-demo`.
4. **Mount point missing:** If the mount fails, ensure the directory was created: `sudo mkdir -p /mnt/phase07-demo`.

---

## 📸 Expected Screenshots
Store evidence in: `assets/screenshots/phase-07/`

* `P07-01-block-device-baseline-srv-admin.png`
* `P07-02-filesystem-identification-srv-admin.png`
* `P07-03-mount-state-srv-admin.png`
* `P07-04-controlled-filesystem-workflow-srv-admin.png`
* `P07-05-persistent-mount-validation-srv-admin.png`
* `P07-06-final-workspace-srv-web.png`
* `P07-07-final-workspace-srv-db.png`
* `P07-08-final-workspace-srv-storage.png`

---

## 🏁 Outcome
Successful execution leaves a validated Phase 07 workspace across all guests, providing visual and technical evidence of block device inspection, filesystem creation, and persistent configuration. The lab is now prepared for **Phase 08: Networking and Firewall**.
