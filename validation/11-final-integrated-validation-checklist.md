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
- [ ] `virsh list --all` executed successfully
- [ ] `srv-admin` visible in libvirt
- [ ] `srv-web` visible in libvirt
- [ ] `srv-db` visible in libvirt
- [ ] `srv-storage` visible in libvirt
- [ ] all required guests in `running` state

### Host Infrastructure State
- [ ] `virsh pool-list --all` executed successfully
- [ ] `enterprise-tech-images` storage pool active
- [ ] `virsh net-list --all` executed successfully
- [ ] `lab-int` network active

### Optional Host Detail Checks
- [ ] `domblklist srv-admin` reviewed successfully
- [ ] `domblklist srv-storage` reviewed successfully

---

## 🛠️ Integrated Validation — `srv-admin`

### System and Network
- [ ] hostname validated as `srv-admin`
- [ ] `ip -brief address` executed successfully
- [ ] lab interface state reviewed successfully

### Core Services
- [ ] `systemctl is-active sshd` returned `active`
- [ ] `systemctl is-active firewalld` returned `active`

### Security
- [ ] `getenforce` executed successfully
- [ ] SELinux state reviewed successfully

### Local Storage Persistence
- [ ] `lsblk` executed successfully
- [ ] extra Phase 07 disk still visible
- [ ] `findmnt /mnt/phase07-demo` executed successfully
- [ ] persistent Phase 07 mount still present

### NFS/autofs Client Validation
- [ ] `cat /mnt/phase09-auto/README.txt` succeeded
- [ ] `mount | grep phase09-auto` confirms automount
- [ ] shared content readable from `srv-storage`

### Evidence Export
- [ ] `system/hostname.txt` created
- [ ] `network/ip-brief-address.txt` created
- [ ] `services/sshd-active.txt` created
- [ ] `services/firewalld-active.txt` created
- [ ] `security/getenforce.txt` created
- [ ] `storage/lsblk.txt` created
- [ ] `storage/findmnt-phase07-demo.txt` created
- [ ] `storage/phase09-auto-readme.txt` created
- [ ] `storage/phase09-auto-mount.txt` created

---

## 💾 Integrated Validation — `srv-storage`

### System and Network
- [ ] hostname validated as `srv-storage`
- [ ] `ip -brief address` executed successfully

### NFS Server Services
- [ ] `systemctl is-active rpcbind` returned `active`
- [ ] `systemctl is-active nfs-server` returned `active`

### Firewall and Export State
- [ ] `firewall-cmd --list-services` executed successfully
- [ ] firewall still includes NFS-related services
- [ ] `showmount -e localhost` executed successfully
- [ ] export visible locally
- [ ] `exportfs -v` executed successfully
- [ ] export configuration still valid

### Security
- [ ] `getenforce` executed successfully
- [ ] SELinux state reviewed successfully

### Evidence Export
- [ ] `system/hostname.txt` created
- [ ] `network/ip-brief-address.txt` created
- [ ] `services/rpcbind-active.txt` created
- [ ] `services/nfs-server-active.txt` created
- [ ] `services/firewall-services.txt` created
- [ ] `storage/showmount-localhost.txt` created
- [ ] `storage/exportfs-v.txt` created
- [ ] `security/getenforce.txt` created

---

## 🌐 Integrated Validation — `srv-web`

### System and Network
- [ ] hostname validated as `srv-web`
- [ ] `ip -brief address` executed successfully

### Core Services
- [ ] `systemctl is-active sshd` returned `active`
- [ ] `systemctl is-active firewalld` returned `active`

### Security
- [ ] `getenforce` executed successfully
- [ ] SELinux state reviewed successfully

### NFS/autofs Client Validation
- [ ] `cat /mnt/phase09-auto/README.txt` succeeded
- [ ] `mount | grep phase09-auto` confirms automount
- [ ] shared content readable successfully

### Evidence Export
- [ ] `system/hostname.txt` created
- [ ] `network/ip-brief-address.txt` created
- [ ] `services/sshd-active.txt` created
- [ ] `services/firewalld-active.txt` created
- [ ] `security/getenforce.txt` created
- [ ] `storage/phase09-auto-readme.txt` created
- [ ] `storage/phase09-auto-mount.txt` created

---

## 🗄️ Integrated Validation — `srv-db`

### System and Network
- [ ] hostname validated as `srv-db`
- [ ] `ip -brief address` executed successfully

### Core Services
- [ ] `systemctl is-active sshd` returned `active`
- [ ] `systemctl is-active firewalld` returned `active`

### Security
- [ ] `getenforce` executed successfully
- [ ] SELinux state reviewed successfully

### NFS/autofs Client Validation
- [ ] `cat /mnt/phase09-auto/README.txt` succeeded
- [ ] `mount | grep phase09-auto` confirms automount
- [ ] shared content readable successfully

### Evidence Export
- [ ] `system/hostname.txt` created
- [ ] `network/ip-brief-address.txt` created
- [ ] `services/sshd-active.txt` created
- [ ] `services/firewalld-active.txt` created
- [ ] `security/getenforce.txt` created
- [ ] `storage/phase09-auto-readme.txt` created
- [ ] `storage/phase09-auto-mount.txt` created

---

## 🔄 Cross-Node Acceptance Check

### Connectivity and Shared Content
- [ ] `ping -c 2 192.168.150.115` succeeded from client nodes
- [ ] NFS export readable through `autofs` on `srv-admin`
- [ ] NFS export readable through `autofs` on `srv-web`
- [ ] NFS export readable through `autofs` on `srv-db`

### Final Integrated State
- [ ] all guest hostnames validated correctly
- [ ] all guest network checks completed
- [ ] all required services validated
- [ ] SELinux validated across all nodes
- [ ] storage and NFS/autofs integration confirmed

---

## 📸 Screenshot Validation
- [ ] `P11-01-host-libvirt-overview.png`
- [ ] `P11-02-srv-admin-integrated-validation.png`
- [ ] `P11-03-srv-storage-nfs-validation.png`
- [ ] `P11-04-srv-web-integrated-validation.png`
- [ ] `P11-05-srv-db-integrated-validation.png`
- [ ] `P11-06-final-cross-node-validation.png`

---

## 🏁 Final Phase 11 Outcome
- [ ] host-side libvirt validation completed
- [ ] `srv-admin` integrated validation completed
- [ ] `srv-storage` integrated validation completed
- [ ] `srv-web` integrated validation completed
- [ ] `srv-db` integrated validation completed
- [ ] cross-node acceptance validation completed
- [ ] final screenshot evidence captured
- [ ] final lab integration confirmed
- [ ] lab ready for final documentation closure
