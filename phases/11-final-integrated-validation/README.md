# ✅ Phase 11 - Final Integrated Validation

## 🎯 Objective
Establish and document the final integrated validation of the **EnterpriseTech RHEL 10 Lab**.

This phase confirmed that the environment works as a coherent multi-node lab after all previous phases were completed. It validated guest readiness, service state, networking, storage, NFS/autofs functionality, and SELinux status as the final acceptance check for the whole project.

This phase used **all four guests together**:
- `srv-admin` (Operations Reference Node)
- `srv-web` (Application Client Node)
- `srv-db` (Database Client Node)
- `srv-storage` (Storage and NFS Server Node)

---

## 🔍 Scope

### **Included in this phase:**
* **Host-Side Validation:** Confirming libvirt guest state, storage pool health, guest disk attachment, and internal network (`lab-int`) state from the Fedora host.
* **Guest Availability:** Verifying that all four guests booted correctly and reported expected hostnames.
* **Core Services:** Confirming `sshd`, `firewalld`, `rpcbind`, and `nfs-server` remained active where expected.
* **Storage Integrity:** Validating the extra disk (`vdb`) and persistent XFS filesystem on `srv-admin`.
* **NFS/Autofs Ecosystem:** Confirming `srv-storage` exports remained visible and clients triggered on-demand mounts correctly.
* **Security Posture:** Confirming SELinux remained in `Enforcing` mode across the entire lab.
* **Final Evidence:** Capturing the final screenshot set proving lab-wide integration.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Final Integrated Validation |
| **Guest Set** | `srv-admin`, `srv-web`, `srv-db`, `srv-storage` |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f11-final-integrated-validation` |
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase followed a **Host → Reference Node → Server → Clients → Cross-Node** flow:

1. **Host-Side Validation:** Confirm all libvirt resources and guests are healthy from the Fedora host.
2. **Reference Node Validation (`srv-admin`):** Validate local storage, persistent mounts, services, security state, and shared-storage access.
3. **Server Validation (`srv-storage`):** Confirm NFS export health, firewall exposure, and server-side listener state.
4. **Client Node Validation (`srv-web`, `srv-db`):** Confirm shared access, core services, network identity, and SELinux state.
5. **Cross-Node Acceptance Check:** Final connectivity and automount verification.
6. **Closure:** Capture final evidence and finalize the lab documentation baseline.

---

## 📐 Workspace Design
Phase 11 operations were organized under a final integrated directory structure on each guest:

```text
~/lab/f11-final-integrated-validation/
├── system/      # Hostname and system identity evidence
├── network/     # IP addressing and reachability evidence
├── services/    # Core service state evidence
├── storage/     # Local disk, NFS, and autofs evidence
├── security/    # SELinux state evidence
└── tmp/         # Temporary validation area
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-admin` | Operations Reference | ✅ Complete |
| `srv-web` | Application Client | ✅ Complete |
| `srv-db` | Database Client | ✅ Complete |
| `srv-storage` | Storage & NFS Server | ✅ Complete |

### **Phase 11 Completed Milestones**
* [x] Host-side libvirt and network validation completed.
* [x] `srv-admin` local storage and persistent mount health confirmed.
* [x] `srv-storage` NFS export visibility validated.
* [x] autofs on-demand mount triggers verified on all clients.
* [x] SELinux Enforcing mode confirmed lab-wide.
* [x] Lab-wide connectivity and service reachability verified.
* [x] Final screenshot evidence captured for project closure.

---

## 🧪 Completed Validation Areas

### 1. Host-Side Validation (Fedora Host)
Validated with:
```bash
sudo virsh -c qemu:///system list --all
sudo virsh -c qemu:///system pool-list --all
sudo virsh -c qemu:///system net-list --all
sudo virsh -c qemu:///system domblklist srv-admin
sudo virsh -c qemu:///system domblklist srv-storage
```

### 2. Integrated Validation on srv-admin
Validated with:
```bash
hostnamectl --static
ip -brief address
systemctl is-active sshd
systemctl is-active firewalld
getenforce
lsblk
findmnt /mnt/phase07-demo
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto
```

### 3. Integrated Validation on srv-storage
Validated with:
```bash
hostnamectl --static
ip -brief address
systemctl is-active rpcbind
systemctl is-active nfs-server
sudo firewall-cmd --list-services
showmount -e localhost
sudo exportfs -v
getenforce
```

### 4. Integrated Validation on srv-web and srv-db
Validated with:
```bash
hostnamectl --static
ip -brief address
systemctl is-active sshd
systemctl is-active firewalld
getenforce
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto
```

### 5. Cross-Node Acceptance Check
Validated with:
```bash
ping -c 2 192.168.150.115
cat /mnt/phase09-auto/README.txt
mount | grep phase09-auto
showmount -e 192.168.150.115
```

---

## 📸 Screenshot Inventory
Evidence stored in assets/screenshots/phase-11/:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P11-01** | Host-side libvirt and network overview | Fedora Host |
| **P11-01a** | Host-side guest block-device attachment detail | Fedora Host |
| **P11-01b** | Virt-manager running guest overview | Fedora Host |
| **P11-02** | srv-admin integrated validation | srv-admin |
| **P11-03** | srv-storage NFS and service state | srv-storage |
| **P11-04** | srv-web integrated validation | srv-web |
| **P11-05** | srv-db integrated validation | srv-db |
| **P11-06** | Final cross-node connectivity check | Lab-wide |

---

## 🔗 Related Files
* `runbooks/f11-final-integrated-validation.md` — Final validation procedure.
* `validation/11-final-integrated-validation-checklist.md` — Final acceptance checklist.
* `phases/10-selinux-troubleshooting/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Final guest state tracker.

---

## 🏁 Current Outcome
Phase 11 is complete ✅.
The lab now has a full end-to-end integrated validation proving that:
- All four guests boot and run correctly under libvirt.
- Host-side storage and networking resources remain healthy.
- Persistent filesystems and shared storage (NFS/Autofs) function correctly.
- SELinux remains in Enforcing mode across the entire lab.
