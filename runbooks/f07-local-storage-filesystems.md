# 🛠️ Phase 07 Runbook: Local Storage and Filesystems

## 🎯 Objective
Execute and validate a practical Phase 07 workflow focused on local storage management. This includes block device inspection, filesystem creation, and the implementation of persistent mount points to ensure data consistency across the guest set.

This runbook uses `srv-admin` as the **full reference workflow** and then applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included**
* **Device Inspection:** Guest-side analysis of block devices and partitions.
* **Metadata Validation:** Using `lsblk` and `blkid` to identify storage attributes.
* **Filesystem Creation:** Building controlled XFS/Ext4 filesystems on lab targets.
* **Mount Operations:** Testing both temporary and persistent (`/etc/fstab`) mounting.
* **Persistence Check:** Validating that storage configuration survives `mount -a`.
* **Evidence Storage:** Capturing storage states into the Phase 07 workspace.

### **Excluded**
* Network-based storage (`NFS`, `CIFS`).
* Advanced LVM (Logical Volume Manager) volume group/logical volume expansion.
* Disk performance benchmarking or I/O tuning.
* Multi-host storage clustering.

---

## 💻 Prerequisites

Before using this runbook, confirm:
| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phase 06 complete | Review `phases/06-running-systems-service-management/README.md` |
| **Guest Availability** | All four guests operational | `virsh list --all` |
| **Disk Target** | Safe lab disk identified | `lsblk` (identify non-OS disks) |
| **Access Control** | Working user has sudo access | `sudo -v` |

> [!IMPORTANT]
> **CRITICAL:** Do not format or repartition the active root disk (`/`). Use only dedicated lab disks (e.g., `/dev/vdb` or `/dev/sdX`) identified during the inspection phase.

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

### 4) Inspect Writable Storage Targets
```bash
sudo fdisk -l
sudo parted -l || true
```

### 5) Controlled Filesystem Workflow
> [!IMPORTANT]
> Replace `/dev/DEVICE_OR_PARTITION` with your identified lab target.

```bash
sudo mkfs.xfs /dev/DEVICE_OR_PARTITION
sudo blkid /dev/DEVICE_OR_PARTITION
```

### 6) Temporary Mount Validation
```bash
sudo mkdir -p /mnt/phase07-demo
sudo mount /dev/DEVICE_OR_PARTITION /mnt/phase07-demo
df -hT /mnt/phase07-demo
mount | grep phase07-demo
```

### 7) Persistent Mount Preparation
> [!IMPORTANT]
> Replace `UUID=REPLACE_ME` with the real UUID returned by `blkid`.

```bash
# Capture the UUID
sudo blkid /dev/DEVICE_OR_PARTITION

# Add to fstab
echo 'UUID=REPLACE_ME /mnt/phase07-demo xfs defaults 0 0' | sudo tee -a /etc/fstab
tail -n 5 /etc/fstab

# Test the configuration
sudo umount /mnt/phase07-demo
sudo mount -a
findmnt /mnt/phase07-demo
```

### 8) Filesystem Evidence Capture
```bash
sudo blkid /dev/DEVICE_OR_PARTITION > filesystems/phase07-blkid.txt
df -hT /mnt/phase07-demo > mounts/phase07-mounted.txt
findmnt /mnt/phase07-demo > mounts/phase07-findmnt.txt
tail -n 5 /etc/fstab > mounts/fstab-tail.txt
find filesystems mounts -maxdepth 1 -type f | sort
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
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

## 🏁 Outcome
Successful execution leaves a validated Phase 07 workspace on all nodes, providing evidence of storage inspection, filesystem creation, and persistent configuration. The lab is now prepared for **Phase 08: Networking and Firewall**.
