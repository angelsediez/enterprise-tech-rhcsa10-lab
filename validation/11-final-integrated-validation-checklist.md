# ✅ Phase 11 Validation Checklist: Final Integrated Validation

## 🎯 Validation Goal
Confirm that the **EnterpriseTech RHEL 10 Lab** remains healthy, integrated, and fully functional after completing all documented phases.

This checklist validates:

- host-side libvirt state
- guest readiness across all four nodes
- core service availability
- persistent local storage on `srv-admin`
- NFS export health on `srv-storage`
- `autofs` client behavior on `srv-admin`, `srv-web`, and `srv-db`
- SELinux state across the full lab

---

## 🖥️ Host-Side Validation — Fedora Host

### Libvirt Inventory
- [x] `virsh list --all` executed successfully
- [x] `srv-admin` visible in libvirt
- [x] `srv-web` visible in libvirt
- [x] `srv-db` visible in libvirt
- [x] `srv-storage` visible in libvirt
- [x] all required guests in `running` state

### Host Infrastructure State
- [x] `virsh pool-list --all` executed successfully
- [x] `enterprise-tech-images` storage pool active
- [x] `virsh net-list --all` executed successfully
- [x] `lab-int` network active

### Host Detail Checks
- [x] `domblklist srv-admin` reviewed successfully
- [x] `domblklist srv-storage` reviewed successfully
- [x] attached guest disk layout reviewed successfully in `virt-manager`

---

## 🛠️ Integrated Validation — `srv-admin`

### System and Network
- [x] hostname validated as `srv-admin`
- [x] `ip -brief address` executed successfully
- [x] lab interface state reviewed successfully

### Core Services
- [x] `systemctl is-active sshd` returned `active`
- [x] `systemctl is-active firewalld` returned `active`

### Security
- [x] `getenforce` executed successfully
- [x] SELinux state reviewed successfully
- [x] SELinux remained in `Enforcing`

### Local Storage Persistence
- [x] `lsblk` executed successfully
- [x] extra Phase 07 disk still visible
- [x] `findmnt /mnt/phase07-demo` executed successfully
- [x] persistent Phase 07 mount still present

### NFS/autofs Client Validation
- [x] `cat /mnt/phase09-auto/README.txt` succeeded
- [x] `mount | grep phase09-auto` confirms automount
- [x] shared content readable from `srv-storage`

### Evidence Export
- [x] `system/hostname.txt` created
- [x] `network/ip-brief-address.txt` created
- [x] `services/sshd-active.txt` created
- [x] `services/firewalld-active.txt` created
- [x] `security/getenforce.txt` created
- [x] `storage/lsblk.txt` created
- [x] `storage/findmnt-phase07-demo.txt` created
- [x] `storage/phase09-auto-readme.txt` created
- [x] `storage/phase09-auto-mount.txt` created

---

## 💾 Integrated Validation — `srv-storage`

### System and Network
- [x] hostname validated as `srv-storage`
- [x] `ip -brief address` executed successfully

### NFS Server Services
- [x] `systemctl is-active rpcbind` returned `active`
- [x] `systemctl is-active nfs-server` returned `active`

### Firewall and Export State
- [x] `firewall-cmd --list-services` executed successfully
- [x] firewall still includes NFS-related services
- [x] `showmount -e localhost` executed successfully
- [x] export visible locally
- [x] `exportfs -v` executed successfully
- [x] export configuration still valid

### Security
- [x] `getenforce` executed successfully
- [x] SELinux state reviewed successfully
- [x] SELinux remained in `Enforcing`

### Evidence Export
- [x] `system/hostname.txt` created
- [x] `network/ip-brief-address.txt` created
- [x] `services/rpcbind-active.txt` created
- [x] `services/nfs-server-active.txt` created
- [x] `services/firewall-services.txt` created
- [x] `storage/showmount-localhost.txt` created
- [x] `storage/exportfs-v.txt` created
- [x] `security/getenforce.txt` created

---

## 🌐 Integrated Validation — `srv-web`

### System and Network
- [x] hostname validated as `srv-web`
- [x] `ip -brief address` executed successfully

### Core Services
- [x] `systemctl is-active sshd` returned `active`
- [x] `systemctl is-active firewalld` returned `active`

### Security
- [x] `getenforce` executed successfully
- [x] SELinux state reviewed successfully
- [x] SELinux remained in `Enforcing`

### NFS/autofs Client Validation
- [x] `cat /mnt/phase09-auto/README.txt` succeeded
- [x] `mount | grep phase09-auto` confirms automount
- [x] sharedx] shared content readable successfully

### Evidence Export
- [x] `system/hostname.txt` created
- [x] `network/ip-brief-address.txt` created
- [x] `services/sshd-active.txt` created
- [x] `services/firewalld-active.txt` created
- [x] `security/getenforce.txt` created
- [x] `storage/phase09-auto-readme.txt` created
- [x] `storage/phase09-auto-mount.txt` created

---

## 🗄️ Integrated Validation — `srv-db`

### System and Network
- [x] hostname validated as `srv-db`
- [x] `ip -brief address` executed successfully

### Core Services
- [x] `systemctl is-active sshd` returned `active`
- [x] `systemctl is-active firewalld` returned `active`

### Security
- [x] `getenforce` executed successfully
- [x] SELinux state reviewed successfully
- [x] SELinux remained in `Enforcing`

### NFS/autofs Client Validation
- [x] `cat /mnt/phase09-auto/README.txt` succeeded
- [x] `mount | grep phase09-auto` confirms automount
- [x] shared content readable successfully

### Evidence Export
- [x] `system/hostname.txt` created
- [x] `network/ip-brief-address.txt` created
- [x] `services/sshd-active.txt` created
- [x] `services/firewalld-active.txt` created
- [x] `security/getenforce.txt` created
- [x] `storage/phase09-auto-readme.txt` created
- [x] `storage/phase09-auto-mount.txt` created

---

## 🔄 Cross-Node Acceptance Check

### Connectivity and Shared Content
- [x] `ping -c 2 192.168.150.115` succeeded from client nodes
- [x] NFS export readable through `autofs` on `srv-admin`
- [x] NFS export readable through `autofs` on `srv-web`
- [x] NFS export readable through `autofs` on `srv-db`

### Final Integrated State
- [x] all guest hostnames validated correctly
- [x] all guest network checks completed
- [x] all required services validated
- [x] SELinux validated across all nodes
- [x] storage and NFS/autofs integration confirmed

---

## 📸 Screenshot Validation
- [x] `P11-01-host-libvirt-overview.png`
- [x] `P11-01a-host-libvirt-domblklist.png`
- [x] `P11-01b-virt-manager-running-guests.png`
- [x] `P11-02-srv-admin-integrated-validation.png`
- [x] `P11-03-srv-storage-nfs-validation.png`
- [x] `P11-04-srv-web-integrated-validation.png`
- [x] `P11-05-srv-db-integrated-validation.png`
- [x] `P11-06-final-cross-node-validation.png`

---

## 🏁 Final Phase 11 Outcome
- [x] host-side libvirt validation completed
- [x] `srv-admin` integrated validation completed
- [x] `srv-storage` integrated validation completed
- [x] `srv-web` integrated validation completed
- [x] `srv-db` integrated validation completed
- [x] cross-node acceptance validation completed
- [x] final screenshot evidence captured
- [x] final lab integration confirmed
- [x] lab ready for final documentation closure
- [x] project ready to mark all phases complete
