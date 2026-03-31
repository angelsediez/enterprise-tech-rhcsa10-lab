# ✅ Phase 07 Validation Checklist - Local Storage and Filesystems

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 07 across the four planned RHEL 10.1 guests.

It confirms that local storage inspection, filesystem identification, controlled XFS filesystem creation, temporary and persistent mount validation, and storage evidence capture were executed successfully and documented with evidence.

---

## 💻 Guest Coverage

| Guest | Status |
| :--- | :--- |
| `srv-admin` | ✅ Complete |
| `srv-web` | ✅ Complete |
| `srv-db` | ✅ Complete |
| `srv-storage` | ✅ Complete |

---

## 🧪 Detailed Validation - `srv-admin`

### Workspace and Context
- [x] Phase 07 workspace created under `~/lab/f07-local-storage-filesystems`
- [x] Working directory validated with `pwd`
- [x] Active user validated with `whoami`
- [x] Hostname validated with `hostnamectl --static`

### Storage Baseline
- [x] `lsblk` executed successfully
- [x] `sudo fdisk -l` executed successfully
- [x] `blkid` executed successfully
- [x] `df -hT` executed successfully
- [x] `mount | head -n 20` executed successfully

### Stored Storage Evidence
- [x] `devices/lsblk.txt` created successfully
- [x] `devices/blkid.txt` created successfully
- [x] `mounts/df-ht.txt` created successfully
- [x] `mounts/mounts.txt` created successfully
- [x] baseline evidence files listed successfully

### Dedicated Lab Disk Validation
- [x] dedicated secondary lab disk identified as `vdb`
- [x] `sudo fdisk -l /dev/vdb` executed successfully
- [x] `sudo parted -l` executed successfully
- [x] GPT label created on `/dev/vdb`
- [x] primary partition created on `/dev/vdb`
- [x] `sudo partprobe /dev/vdb` executed successfully
- [x] partition `vdb1` confirmed successfully

### Controlled Filesystem Workflow
- [x] `sudo mkfs.xfs /dev/vdb1` executed successfully
- [x] `sudo blkid /dev/vdb1` executed successfully
- [x] XFS filesystem confirmed on `vdb1`

### Temporary Mount Validation
- [x] `/mnt/phase07-demo` created successfully
- [x] `/dev/vdb1` mounted successfully on `/mnt/phase07-demo`
- [x] `df -hT /mnt/phase07-demo` validated successfully
- [x] `mount | grep phase07-demo` validated successfully

### Persistent Mount Validation
- [x] UUID captured from `/dev/vdb1`
- [x] `/etc/fstab` entry added successfully
- [x] `tail -n 5 /etc/fstab` validated successfully
- [x] `sudo mount -a` executed successfully
- [x] `findmnt /mnt/phase07-demo` validated successfully

### Final Filesystem Evidence
- [x] `filesystems/phase07-blkid.txt` created successfully
- [x] `mounts/phase07-mounted.txt` created successfully
- [x] `mounts/phase07-findmnt.txt` created successfully
- [x] `mounts/fstab-tail.txt` created successfully
- [x] final workspace listing completed successfully

---

## 🔁 Short Validation - `srv-web`
- [x] workspace created
- [x] `devices/lsblk.txt` created
- [x] `devices/blkid.txt` created
- [x] `mounts/df-ht.txt` created
- [x] `mounts/mounts.txt` created
- [x] `cat devices/lsblk.txt | head -n 20` executed successfully
- [x] `cat mounts/df-ht.txt | head -n 20` executed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P07-06-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [x] workspace created
- [x] `devices/lsblk.txt` created
- [x] `devices/blkid.txt` created
- [x] `mounts/df-ht.txt` created
- [x] `mounts/mounts.txt` created
- [x] `cat devices/lsblk.txt | head -n 20` executed successfully
- [x] `cat mounts/df-ht.txt | head -n 20` executed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P07-07-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [x] workspace created
- [x] `devices/lsblk.txt` created
- [x] `devices/blkid.txt` created
- [x] `mounts/df-ht.txt` created
- [x] `mounts/mounts.txt` created
- [x] `cat devices/lsblk.txt | head -n 20` executed successfully
- [x] `cat mounts/df-ht.txt | head -n 20` executed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P07-08-final-workspace-srv-storage.png`

---

## 📸 Screenshot Inventory
- [x] `P07-01-block-device-baseline-srv-admin.png`
- [x] `P07-02-filesystem-identification-srv-admin.png`
- [x] `P07-03-mount-state-srv-admin.png`
- [x] `P07-04-controlled-filesystem-workflow-srv-admin.png`
- [x] `P07-05-persistent-mount-validation-srv-admin.png`
- [x] `P07-06-final-workspace-srv-web.png`
- [x] `P07-07-final-workspace-srv-db.png`
- [x] `P07-08-final-workspace-srv-storage.png`

### Optional Extra Evidence
- [x] `P07-extra-lsblk-mounted-vdb1-srv-admin.png`
- [x] `P07-extra-temporary-mount-srv-admin.png`

---

## 🏁 Phase 07 Exit Criteria
Phase 07 is considered complete when:

- [x] `srv-admin` full workflow completed and validated
- [x] `srv-web` short validation completed
- [x] `srv-db` short validation completed
- [x] `srv-storage` short validation completed
- [x] all expected screenshots captured
- [x] `phases/07-local-storage-filesystems/README.md` updated
- [x] `runbooks/f07-local-storage-filesystems.md` updated
- [x] this checklist reviewed and completed

---

## 📝 Notes
- [x] the dedicated lab disk used on `srv-admin` was `vdb`
- [x] the controlled filesystem was created on `vdb1`
- [x] the filesystem type used for the Phase 07 lab workflow was `xfs`
- [x] the secondary guests used the inspection-only validation pattern
- [x] no extra disk, no `mkfs`, and no `/etc/fstab` changes were applied on `srv-web`, `srv-db`, or `srv-storage`
