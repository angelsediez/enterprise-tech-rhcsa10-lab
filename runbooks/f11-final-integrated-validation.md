# 🛠️ Phase 11 Runbook: Final Integrated Validation

## 🎯 Objective
Execute and document the final integrated validation of the **EnterpriseTech RHEL 10 Lab**.

This runbook confirms that the environment behaves as a complete, functioning multi-node lab after all previous phases have been completed. It validates host-side libvirt state, guest readiness, core services, storage persistence, NFS/autofs functionality, and SELinux state as a final acceptance check for the project.

This phase uses **all four guests together**:
- `srv-admin`
- `srv-web`
- `srv-db`
- `srv-storage`

---

## 🔍 Scope

### **Included**
- Host-side libvirt guest validation.
- Guest hostname and network validation.
- Core service state verification (sshd, firewalld, nfs).
- Local storage persistence verification on `srv-admin` (Phase 07).
- NFS server validation on `srv-storage` (Phase 09).
- NFS/autofs client validation on `srv-admin`, `srv-web`, and `srv-db`.
- SELinux state validation across all guests (Phase 10).
- Final evidence capture.

### **Excluded**
- New service deployment or configuration changes.
- Performance testing or load benchmarking.

---

## 💻 Prerequisites

| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phases 01-10 Complete | main README.md |
| **Guest Status** | All 4 VMs running | `virsh list` |
| **Permissions** | Sudo access on host and guests | `sudo -v` |

> [!IMPORTANT]
> This phase is a **final acceptance workflow**. The goal is to confirm the lab remains healthy and integrated using the state already built.

---

## 📍 Validation Model
**Host → srv-admin → srv-storage → srv-web → srv-db → Final Cross-Node Check**

---

## 🚀 Host-Side Validation (Fedora Host)
1. **Validate libvirt inventory:**
```bash
sudo virsh -c qemu:///system list --all
sudo virsh -c qemu:///system pool-list --all
sudo virsh -c qemu:///system net-list --all
```
**Screenshot:** `P11-01-host-libvirt-overview.png`

---

## 🚀 Integrated Validation on srv-admin
1. **Create workspace:**
```bash
mkdir -p ~/lab/f11-final-integrated-validation/{system,network,services,storage,security,tmp}
cd ~/lab/f11-final-integrated-validation
```
2. **Validate and save evidence:**
```bash
hostnamectl --static > system/hostname.txt
ip -brief address > network/ip-brief-address.txt
systemctl is-active sshd > services/sshd-active.txt
systemctl is-active firewalld > services/firewalld-active.txt
getenforce > security/getenforce.txt
lsblk > storage/lsblk.txt
findmnt /mnt/phase07-demo > storage/findmnt-phase07-demo.txt
ls /mnt/phase09-auto/README.txt > /dev/null
mount | grep phase09-auto > storage/phase09-auto-mount.txt
```
**Screenshot:** `P11-02-srv-admin-integrated-validation.png`

---

## 🚀 Integrated Validation on srv-storage
1. **Validate NFS server state:**
```bash
systemctl is-active rpcbind nfs-server
sudo firewall-cmd --list-services
showmount -e localhost
sudo exportfs -v
```
**Screenshot:** `P11-03-srv-storage-nfs-validation.png`

---

## 🚀 Integrated Validation on Clients (srv-web & srv-db)
1. **Validate client state and automount:**
```bash
hostnamectl --static
systemctl is-active sshd firewalld
ls /mnt/phase09-auto/README.txt
getenforce
```
**Screenshots:** `P11-04-srv-web-integrated-validation.png` | `P11-05-srv-db-integrated-validation.png`

---

## 🚀 Final Cross-Node Acceptance Check
Execute from **srv-admin**:
```bash
echo "=== Cross-Node Acceptance Check ==="
ping -c 2 192.168.150.115
showmount -e 192.168.150.115
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto
```
**Screenshot:** `P11-06-final-cross-node-validation.png`

---

## 🧯 Short Troubleshooting
1. **autofs path not readable:** `systemctl restart autofs` and check `srv-storage` firewall.
2. **Phase 07 disk missing:** Verify `vdb` presence with `lsblk` and check `/etc/fstab`.
3. **SELinux denials:** Check `ausearch -m AVC -ts recent`.

---

## 🏁 Outcome
The lab is officially validated and integrated.
**Status: READY FOR ARCHIVE ✅**
