# 📖 RHEL 10.1 Guest Installation Runbook

## 🎯 Objective
Deploy the Phase 02 RHEL 10.1 guest set on the Fedora 43 virtualization host using the existing libvirt storage pool, internal lab network, and stable ISO path.

This runbook documents the validated installation workflow first used for `srv-admin` and then successfully reused for:
* `srv-web`
* `srv-db`
* `srv-storage`

---

## 🔍 Scope

### **Included in this runbook:**
* Host-side preflight checks and ISO validation.
* Storage pool and network validation.
* Guest disk creation & `virt-install` execution.
* Base RHEL 10.1 installation flow & first boot validation.
* Host-side validation with `virsh`.
* Autostart enablement after successful deployment.

### **Excluded (Later Phases):**
* Advanced post-install configuration (SSH hardening, SELinux tuning).
* Firewalld policy & service-specific configuration.
* NFS/autofs setup.

---

## 💻 Host & Guest Context

### Host Environment
| Component | Details |
| :--- | :--- |
| **Host OS** | Fedora Linux 43 (KDE Plasma Desktop Edition) |
| **Hypervisor** | KVM + QEMU + libvirt + virt-manager |
| **Connection** | `qemu:///system` |
| **Storage Pool** | `enterprise-tech-images` |
| **Network** | `lab-int` |
| **ISO Path** | `/var/lib/libvirt/boot/rhel-10.1-x86_64-dvd.iso` |

### Reference Guest Model (`srv-admin`)
| Resource | Specification |
| :--- | :--- |
| **vCPU** | 2 |
| **RAM** | 4096 MiB |
| **Disk** | 60 GiB (qcow2) |
| **Firmware** | UEFI / OVMF |
| **OS Info** | `rhel10.1` |

> [!NOTE]
> The same baseline guest model was reused for `srv-web`, `srv-db`, and `srv-storage` during Phase 02.

> [!TIP]
> Adjust guest resources later only if the workload justifies it.

---

## 🛠️ Installation Variables
Run these commands on the **Fedora 43 host** to prepare the environment:

```bash
export ISO="/var/lib/libvirt/boot/rhel-10.1-x86_64-dvd.iso"
export VM_NAME="srv-admin"
export VM_RAM="4096"
export VM_VCPUS="2"
export VM_DISK="60G"
export VM_POOL="enterprise-tech-images"
export VM_NET="lab-int"
export VM_OSINFO="rhel10.1"
```

* **Why:** Reduces typing mistakes and keeps the guest deployment workflow consistent.
* **Verification:** Run `echo "$VM_NAME" && echo "$VM_NET"` to confirm.
* **Persistence:** Temporary for the current shell session only.

---

## 🚀 Installation Steps

### Step 1: Preflight Validation
```bash
ls -lh "$ISO"
virsh -c qemu:///system pool-info "$VM_POOL"
virsh -c qemu:///system net-info "$VM_NET"
virt-install --osinfo list | grep -i rhel10
```
* **Goal:** Confirm ISO existence, active storage pool, and network status.

### Step 2: Collision Check
```bash
virsh -c qemu:///system dominfo "$VM_NAME" || true
virsh -c qemu:///system vol-list "$VM_POOL"
```
* **Goal:** Avoid creating a VM or disk with an already-used name.

### Step 3: Create Guest Disk
```bash
virsh -c qemu:///system vol-create-as "$VM_POOL" "${VM_NAME}.qcow2" "$VM_DISK" --format qcow2
virsh -c qemu:///system vol-info --pool "$VM_POOL" "${VM_NAME}.qcow2"
```
* **Goal:** Provision a persistent 60 GiB qcow2 disk in the storage pool.

### Step 4: Launch Installation
```bash
sudo virt-install \
  --connect qemu:///system \
  --name "$VM_NAME" \
  --memory "$VM_RAM" \
  --vcpus "$VM_VCPUS" \
  --cpu host-passthrough \
  --osinfo "$VM_OSINFO" \
  --boot uefi \
  --disk vol="${VM_POOL}/${VM_NAME}.qcow2",bus=virtio \
  --network network="$VM_NET",model=virtio \
  --graphics spice \
  --video qxl \
  --cdrom "$ISO"
```
* **Note:** If the console does not appear, use `virt-manager` to connect manually.
* **Console fallback:** If `virt-viewer` freezes or the graphical console becomes unresponsive, relaunch the guest connection through `virt-manager` or use `--noautoconsole` in a new install attempt.
* **Phase 02 reuse:** The same `virt-install` workflow was reused for `srv-web`, `srv-db`, and `srv-storage`, changing only the guest name and in-guest hostname.

---

## 📝 Anaconda Installer Guidelines
During installation, the following baseline choices were used:

* **Hostname:** `srv-admin` for the reference deployment, then `srv-web`, `srv-db`, and `srv-storage` for the remaining guests.
* **Installation Destination:** Automatic layout on the 60 GiB VirtIO disk.
* **Software Selection:** Server (Keep it simple, avoid optional roles for now).
* **Network:** Enabled and connected to `lab-int` using DHCP.
* **Security:** Root account enabled; initial local user `jdoe` created with administrative privileges.
* **Pattern Consistency:** The same baseline installation choices were reused across all four Phase 02 guests.

---

## 🧪 Post-Install Validation

### Guest-Side Check (Inside the VM)
```bash
cat /etc/os-release
hostnamectl
ip -brief address
systemctl is-system-running || true
systemctl get-default
lsblk
```

### Host-Side Check (Fedora 43 Host)
```bash
virsh -c qemu:///system list --all
virsh -c qemu:///system dominfo "$VM_NAME"
virsh -c qemu:///system domifaddr "$VM_NAME" || true
virsh -c qemu:///system net-dhcp-leases "$VM_NET" || true
```

---

### Step 5: Enable Autostart & Verify
Once validation is complete, ensure the VM starts with the host:
```bash
virsh -c qemu:///system autostart "$VM_NAME"
virsh -c qemu:///system dominfo "$VM_NAME"
```

> [!IMPORTANT]
> Enable autostart only after confirming that the installation completed successfully and the guest boots normally without installer media.

---

## 🧹 Cleanup (If things go wrong)
To remove the guest and rebuild from scratch:
```bash
virsh -c qemu:///system destroy "$VM_NAME" || true
virsh -c qemu:///system undefine "$VM_NAME" --nvram || true
virsh -c qemu:///system vol-delete --pool "$VM_POOL" "${VM_NAME}.qcow2" || true
virsh -c qemu:///system dominfo "$VM_NAME" || true
virsh -c qemu:///system vol-list "$VM_POOL"
```

---

## 📸 Artifacts & Documentation
### Screenshots to Capture (`assets/screenshots/phase-02/`)

**srv-admin Core Evidence:**
* `P02-01-iso-pool-network-ready.png`
* `P02-02-srv-admin-volume-created.png`
* `P02-03-virt-install-srv-admin.png`
* `P02-04-anaconda-installation-summary-srv-admin.png`
* `P02-05-anaconda-installation-destination-srv-admin.png`
* `P02-06-anaconda-network-hostname-srv-admin.png`
* `P02-08-srv-admin-host-side-validation.png`

**srv-admin Additional Detail:**
* `P02-04b-anaconda-software-selection-srv-admin.png`
* `P02-07a-srv-admin-os-and-hostname-validation.png`
* `P02-07b-srv-admin-network-target-storage-validation.png`
* `P02-06b-anaconda-root-account-srv-admin.png`

**srv-web Core Evidence:**
* `P02-09-virt-install-srv-web.png`
* `P02-11-anaconda-network-hostname-srv-web.png`
* `P02-14-srv-web-first-boot-validation.png`
* `P02-15-srv-web-host-side-validation.png`

**srv-web Additional Detail:**
* `P02-10-anaconda-installation-destination-srv-web.png`
* `P02-12-anaconda-installation-summary-srv-web.png`
* `P02-13-installation-complete-srv-web.png`

**srv-db Core Evidence:**
* `P02-16-virt-install-srv-db.png`
* `P02-17-anaconda-installation-destination-srv-db.png`
* `P02-18-anaconda-network-hostname-srv-db.png`
* `P02-19-anaconda-installation-summary-srv-db.png`
* `P02-20-installation-complete-srv-db.png`
* `P02-21-srv-db-first-boot-validation.png`
* `P02-22-srv-db-host-side-validation.png`

**srv-db Additional Detail:**
* `P02-19b-anaconda-software-selection-srv-db.png`
* `P02-19c-anaconda-root-account-srv-db.png`
* `P02-19d-anaconda-user-creation-srv-db.png`
* `P02-22b-srv-db-autostart-and-dominfo.png`

**srv-storage Core Evidence:**
* `P02-23-virt-install-srv-storage.png`
* `P02-24-anaconda-installation-destination-srv-storage.png`
* `P02-25-anaconda-network-hostname-srv-storage.png`
* `P02-26-anaconda-installation-summary-srv-storage.png`
* `P02-27-installation-complete-srv-storage.png`
* `P02-28-srv-storage-first-boot-validation.png`
* `P02-29-srv-storage-host-side-validation.png`

**srv-storage Additional Detail:**
* `P02-26b-anaconda-software-selection-srv-storage.png`
* `P02-26c-anaconda-root-account-srv-storage.png`
* `P02-26d-anaconda-user-creation-srv-storage.png`
* `P02-29b-srv-storage-autostart-and-dominfo.png`

---

## ✅ Phase 02 Completion Note

The validated installation pattern documented in this runbook was successfully reused to deploy:

* `srv-web`
* `srv-db`
* `srv-storage`

All four planned Phase 02 guests were installed on RHEL 10.1, attached to `lab-int`, validated from both guest-side and host-side, and configured with libvirt autostart after successful validation.

---

## 🏁 Outcome
Successful completion of this runbook produced:

* A working `srv-admin` reference guest
* A validated RHEL 10.1 installation pattern
* Confirmed connectivity on `lab-int` and host-side visibility
* Autostart enabled after successful validation
* Successful reuse of the same deployment pattern for `srv-web`, `srv-db`, and `srv-storage`

The lab now has a complete four-node RHEL 10.1 baseline for the next infrastructure and service configuration phases.
