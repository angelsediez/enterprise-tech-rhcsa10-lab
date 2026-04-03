# 🛠️ Phase 11 Runbook: Final Integrated Validation

## 🎯 Objective
Execute and document the final integrated validation of the **EnterpriseTech RHEL 10 Lab**. 

This runbook acts as the **Final Acceptance Test (FAT)**, confirming that the environment behaves as a complete, functioning multi-node system. It validates host-side libvirt states, guest readiness, core services, storage persistence, NFS/autofs ecosystems, and SELinux enforcement across the entire lab.

---

## 🔍 Scope

### **Included**
* **Host-Side:** Libvirt guest state, storage pools, and network health.
* **Core Infrastructure:** Hostname resolution and static IP consistency.
* **Services:** Persistence of `sshd`, `firewalld`, and `rpc-bind`.
* **Storage:** Phase 07 local storage persistence on `srv-admin`.
* **Shared Storage:** Phase 09 NFS exports and `autofs` on-demand triggers.
* **Security:** Lab-wide SELinux `Enforcing` mode validation.
* **Evidence:** Final "Golden State" screenshot capture.

---

## 💻 Prerequisites

| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phases 01-10 complete | Main repository docs |
| **Guest Status** | All 4 guests operational | `virsh list --all` |
| **Access Control** | Working user has sudo rights | `sudo -v` |

> [!IMPORTANT]
> This phase is a **final acceptance workflow**. The goal is to confirm the lab remains healthy and integrated.

---

## 📍 Validation Model
**Host → srv-admin → srv-storage → srv-web → srv-db → Final Cross-Node Check**

---

## 📐 Suggested Workspace on Guests
```text
~/lab/f11-final-integrated-validation/
├── system/      # Hostname and system identity evidence
├── network/     # IP addressing and reachability evidence
├── services/    # Core service state evidence
├── storage/     # Local disk, NFS, and autofs evidence
└── security/    # SELinux state evidence
```

---

## 🚀 Host-Side Validation (Fedora Host)
1. **Validate libvirt guest inventory:**
```bash
sudo virsh -c qemu:///system list --all
sudo virsh -c qemu:///system pool-list --all
sudo virsh -c qemu:///system net-list --all
```
2. **Validate guest disk attachment:**
```bash
sudo virsh -c qemu:///system domblklist srv-admin
sudo virsh -c qemu:///system domblklist srv-storage
```
**Screenshots:** `P11-01-host-libvirt-overview.png`, `P11-01a-host-libvirt-domblklist.png`, `P11-01b-virt-manager-running-guests.png`

---

## 🚀 Integrated Validation on srv-admin
1. **Create workspace and save evidence:**
```bash
mkdir -p ~/lab/f11-final-integrated-validation/{system,network,services,storage,security,tmp}
cd ~/lab/f11-final-integrated-validation
hostnamectl --static > system/hostname.txt
ip -brief address > network/ip-brief-address.txt
systemctl is-active sshd firewalld > services/core-services.txt
getenforce > security/getenforce.txt
lsblk > storage/lsblk.txt
findmnt /mnt/phase07-demo > storage/findmnt-phase07-demo.txt
ls /mnt/phase09-auto/README.txt > /dev/null
mount | grep -E 'phase07|phase09' > storage/mounts-integrated.txt
```
**Screenshot:** `P11-02-srv-admin-integrated-validation.png`

---

## 🚀 Final Cross-Node Acceptance Check (from srv-admin)
```bash
echo "=== INTEGRATED LAB CHECK ==="
ping -c 2 192.168.150.115
showmount -e 192.168.150.115
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto
```
**Screenshot:** `P11-06-final-cross-node-validation.png`

---

## 🧪 Validation Targets (Status: VERIFIED)

### **Infrastructure Baseline**
- [x] All guests visible in libvirt and in `Running` state.
- [x] Storage pool `enterprise-tech-images` is active.
- [x] Static hostnames match the Lab Inventory.

### **Service & Storage Integrity**
- [x] `srv-admin`: Persistent XFS mount (`vdb1`) verified on `/mnt/phase07-demo`.
- [x] `srv-storage`: NFS exports visible locally and via RPC.
- [x] Clients: `autofs` triggers correctly without manual mount commands.

### **Security Baseline**
- [x] `firewalld` is active and filtering on all nodes.
- [x] `sshd` is active and accepting connections.
- [x] SELinux is in `Enforcing` mode lab-wide.

---

## 🏁 Outcome
Successful execution of this runbook confirms that the full lab remains healthy and integrated across all core infrastructure areas.

**Status: Final lab validation complete ✅**
