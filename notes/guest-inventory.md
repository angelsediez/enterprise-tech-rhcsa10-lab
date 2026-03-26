# 📊 Guest Inventory - RHEL 10 Lab

## 🎯 Purpose
This file tracks the guest virtual machines used in the **EnterpriseTech RHEL 10 lab**. It provides a quick operational overview of resource allocation, installation progress, validation status, and screenshot coverage across the environment.

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

| Guest | Role | vCPU | RAM | Disk | Firmware | Install Status | Validation | Screenshots |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- | :--- | :--- |
| `srv-admin` | Administration and management node | 2 | 4096 | 60G qcow2 | UEFI / OVMF | ✅ Installed | ✅ Validated | ✅ Complete |
| `srv-web` | Web service node | 2 | 4096 | 60G qcow2 | UEFI / OVMF | ✅ Installed | ✅ Validated | ✅ Complete |
| `srv-db` | Database service node | 2 | 4096 | 60G qcow2 | UEFI / OVMF | ✅ Installed | ✅ Validated | ✅ Complete |
| `srv-storage` | Shared storage and NFS/autofs support node | 2 | 4096 | 60G qcow2 | UEFI / OVMF | ⚪ Pending | ⚪ Pending | ⚪ Pending |

> [!NOTE]
> All disks are stored in the `enterprise-tech-images` storage pool.

---

## 🚦 Status Definitions

### Install Status
- ⚪ Pending — Resource planned but not yet created.
- 🟡 In progress — Currently being installed or configured.
- ✅ Installed — OS installation completed successfully.
- 🔴 Rebuild required — Existing guest needs to be destroyed and re-deployed.

### Validation Status
- ⚪ Pending — No validation performed yet.
- 🟡 Partially validated — Basic connectivity or services confirmed.
- ✅ Validated — Guest validated according to the current runbook scope.
- 🔴 Validation failed — Issues found during post-install checks.

### Screenshot Status
- ⚪ Pending — No screenshots captured yet.
- 🟡 Partial — Some evidence captured, but documentation is incomplete.
- ✅ Complete — Expected screenshot set captured for the current phase.

---

## 📓 Per-Guest Notes

### 🛠️ `srv-admin`
- First deployed guest and reference installation model for the lab.
- Successfully installed on **RHEL 10.1**.
- Hostname validated inside the guest as `srv-admin`.
- Connected to the `lab-int` network with DHCP-assigned connectivity.
- Visible and validated from the Fedora 43 host using `virsh`.
- Autostart enabled after successful validation.
- Screenshot set completed for Phase 02.
- This guest now serves as the baseline installation pattern for the remaining nodes.

### 🌐 `srv-web`
- Successfully deployed using the validated `srv-admin` installation pattern.
- Installed on **RHEL 10.1** with hostname `srv-web`.
- Connected to the `lab-int` network with DHCP-assigned connectivity.
- Validated from both guest-side and host-side.
- Autostart enabled after successful validation.
- Screenshot set completed for the Phase 02 deployment workflow.

### 🗄️ `srv-db`
- Successfully deployed using the validated `srv-admin` and `srv-web` installation pattern.
- Installed on **RHEL 10.1** with hostname `srv-db`.
- Connected to the `lab-int` network with DHCP-assigned connectivity.
- Validated from both guest-side and host-side.
- Autostart enabled after successful validation.
- Screenshot set completed for the Phase 02 deployment workflow.

### 💾 `srv-storage`
- Intended for shared storage workflows and later NFS/autofs exercises.
- May require additional virtual disk planning in future phases.

---

## 🔄 Tracking & Maintenance
Update this file whenever one of the following events occurs:

1. A guest is defined or created in libvirt
2. Hardware resources (RAM, vCPU, disk) are modified
3. Installation or validation phases are completed
4. Screenshots are captured and stored in the assets folder
5. Autostart or other persistent VM behavior is changed

---

## 🚩 Current Phase Alignment
- **Phase 00:** Repository Bootstrap ✅
- **Phase 01:** Virtualization Host Ready ✅
- **Phase 02:** First Guest Deployment ✅

**Current validated guests:** `srv-admin`, `srv-web`, `srv-db`  
**Next planned guest:** `srv-storage`

The remaining guest stays in `Pending` state until deployment begins.
