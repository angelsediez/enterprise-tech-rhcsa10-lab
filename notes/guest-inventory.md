# 📊 Guest Inventory - RHEL 10 Lab

## 🎯 Purpose
This file tracks the guest virtual machines used in the **EnterpriseTech RHEL 10 lab**. It provides a quick operational overview of resource allocation, guest roles, storage layout, validation scope, and current phase alignment across the environment.

---

## 💻 Host Context
The following environment settings apply to the virtualization host where these guests reside:

| Component | Specification |
| :--- | :--- |
| **Host Platform** | Fedora Linux 43 (KDE Plasma Desktop Edition) |
| **Hypervisor Stack** | KVM + QEMU + libvirt + virt-manager |
| **Libvirt Connection** | `qemu:///system` |
| **Storage Pool** | `enterprise-tech-images` |
| **Internal Network** | `lab-int` |
| **Guest OS Target** | Red Hat Enterprise Linux 10.1 |
| **ISO Path** | `/var/lib/libvirt/boot/rhel-10.1-x86_64-dvd.iso` |

---

## 📑 Guest Inventory Table

| Guest | Role | vCPU | RAM | Disk Layout | Firmware | Install Status | Validation | Screenshots |
| :--- | :--- | :---: | :---: | :--- | :---: | :--- | :--- | :--- |
| `srv-admin` | Administration and management node | 2 | 4096 MiB | `vda` = 60G qcow2 OS disk + `vdb` = 10G qcow2 lab disk | UEFI / OVMF | ✅ Installed | ✅ Validated | ✅ Complete |
| `srv-web` | Web service client node | 2 | 4096 MiB | `vda` = 60G qcow2 OS disk | UEFI / OVMF | ✅ Installed | ✅ Validated | ✅ Complete |
| `srv-db` | Database client node | 2 | 4096 MiB | `vda` = 60G qcow2 OS disk | UEFI / OVMF | ✅ Installed | ✅ Validated | ✅ Complete |
| `srv-storage` | Shared storage and NFS server node | 2 | 4096 MiB | `vda` = 60G qcow2 OS disk | UEFI / OVMF | ✅ Installed | ✅ Validated | ✅ Complete |

> [!NOTE]
> All virtual disks are stored in the `enterprise-tech-images` storage pool.

---

## 🚦 Status Definitions

### Install Status
- ⚪ Pending — Resource planned but not yet created.
- 🟡 In progress — Currently being installed or configured.
- ✅ Installed — OS installation completed successfully.
- 🔴 Rebuild required — Existing guest needs to be destroyed and re-deployed.

### Validation Status
- ⚪ Pending — No validation performed yet.
- 🟡 Partially validated — Basic connectivity or service checks completed.
- ✅ Validated — Guest validated according to the current documented phase scope.
- 🔴 Validation failed — Issues found during validation.

### Screenshot Status
- ⚪ Pending — No screenshots captured yet.
- 🟡 Partial — Some evidence captured, but documentation is incomplete.
- ✅ Complete — Expected screenshot set captured for the current completed phases.

---

## 📓 Per-Guest Notes

### 🛠️ `srv-admin`
- First deployed guest and reference installation model for the lab.
- Installed on **Red Hat Enterprise Linux 10.1** with validated hostname `srv-admin`.
- Connected to the `lab-int` network with DHCP-assigned lab connectivity.
- Acts as the primary **reference node** for most phase workflows.
- Received an additional **10G qcow2 lab disk** as `vdb` during **Phase 07** for local storage and filesystem validation.
- Validated through:
  - Phase 02 — deployment baseline
  - Phase 03 — shell, files, and local documentation
  - Phase 04 — identity, SSH, and permissions
  - Phase 05 — software and scripting
  - Phase 06 — running systems and service management
  - Phase 07 — local storage and filesystems
  - Phase 08 — networking and firewall
  - Phase 09 — NFS client and `autofs` validation
- Autostart enabled after successful validation.
- Screenshot coverage complete through the current documented phase set.

### 🌐 `srv-web`
- Successfully deployed using the validated guest pattern established by `srv-admin`.
- Installed on **Red Hat Enterprise Linux 10.1** with validated hostname `srv-web`.
- Connected to the `lab-int` network with DHCP-assigned connectivity.
- Acts as a replicated **client validation node** for later service and infrastructure phases.
- Validated through:
  - Phase 02 — deployment baseline
  - Phase 03 — shell, files, and local documentation
  - Phase 04 — identity, SSH, and permissions
  - Phase 05 — software and scripting
  - Phase 06 — running systems and service management replication
  - Phase 07 — storage inspection replication
  - Phase 08 — networking and firewall replication
  - Phase 09 — NFS and `autofs` client replication
- Autostart enabled after successful validation.
- Screenshot coverage complete through the current documented phase set.

### 🗄️ `srv-db`
- Successfully deployed using the validated guest pattern reused across the lab.
- Installed on **Red Hat Enterprise Linux 10.1** with validated hostname `srv-db`.
- Connected to the `lab-int` network with DHCP-assigned connectivity.
- Acts as a replicated **client validation node** for later infrastructure and storage-consumer phases.
- Validated through:
  - Phase 02 — deployment baseline
  - Phase 03 — shell, files, and local documentation
  - Phase 04 — identity, SSH, and permissions
  - Phase 05 — software and scripting
  - Phase 06 — running systems and service management replication
  - Phase 07 — storage inspection replication
  - Phase 08 — networking and firewall replication
  - Phase 09 — NFS and `autofs` client replication
- Autostart enabled after successful validation.
- Screenshot coverage complete through the current documented phase set.

### 💾 `srv-storage`
- Successfully deployed using the validated installation pattern established by the previous guests.
- Installed on **Red Hat Enterprise Linux 10.1** with validated hostname `srv-storage`.
- Connected to the `lab-int` network with DHCP-assigned connectivity.
- Acts as the lab’s **shared storage provider** and the **reference NFS server** in Phase 09.
- Validated through:
  - Phase 02 — deployment baseline
  - Phase 03 — shell, files, and local documentation
  - Phase 04 — identity, SSH, and permissions
  - Phase 05 — software and scripting
  - Phase 06 — running systems and service management replication
  - Phase 07 — storage inspection replication
  - Phase 08 — networking and firewall replication
  - Phase 09 — NFS server export, firewall, and shared-storage validation
- Autostart enabled after successful validation.
- Screenshot coverage complete through the current documented phase set.

---

## 🔄 Tracking & Maintenance
Update this file whenever one of the following events occurs:

1. A guest is defined or recreated in libvirt
2. Hardware resources (RAM, vCPU, disk, firmware) are modified
3. A guest role changes within the lab architecture
4. A new phase changes validation scope for one or more guests
5. Screenshot coverage expands for completed phases
6. Persistent VM behavior such as autostart changes

---

## 🚩 Current Phase Alignment
- **Phase 00:** Repository Bootstrap ✅
- **Phase 01:** Virtualization Host Ready ✅
- **Phase 02:** Guest Deployment ✅
- **Phase 03:** Shell, Files, and Local Documentation ✅
- **Phase 04:** Identity, SSH, and Permissions ✅
- **Phase 05:** Software and Scripting ✅
- **Phase 06:** Running Systems and Service Management ✅
- **Phase 07:** Local Storage and Filesystems ✅
- **Phase 08:** Networking and Firewall ✅
- **Phase 09:** NFS and autofs ✅

**Current validated guests:** `srv-admin`, `srv-web`, `srv-db`, `srv-storage`  
**Current reference infrastructure roles:**  
- `srv-admin` → reference operations node  
- `srv-storage` → reference NFS server  
- `srv-web`, `srv-db` → replicated client validation nodes  

**Next planned phase:** **Phase 10 — SELinux and troubleshooting**

---

## 🏁 Current Inventory Outcome
All planned guests are deployed, documented, and validated through the current lab scope.

The lab now includes:

- a stable four-node RHEL 10.1 guest set
- a dedicated extra lab disk on `srv-admin` for filesystem practice
- validated networking and firewall baselines across all nodes
- a working NFS server on `srv-storage`
- validated NFS and `autofs` client behavior on `srv-admin`, `srv-web`, and `srv-db`
