# ✅ Phase 11 - Final Integrated Validation

## 🎯 Objective
Establish and document the final integrated validation of the **EnterpriseTech RHEL 10 Lab**.

This phase confirms that the environment works as a coherent multi-node lab after all previous phases have been completed. It validates guest readiness, service state, networking, storage, NFS/autofs functionality, and SELinux status as a final acceptance check for the whole project.

This phase uses **all four guests together**:
- `srv-admin` (Operations Reference Node)
- `srv-web` (Application Client Node)
- `srv-db` (Database Client Node)
- `srv-storage` (Storage and NFS Server Node)

---

## 🔍 Scope

### **Included in this phase:**
* **Host-Side Validation:** Confirming libvirt guest state, storage pool health, and internal network (`lab-int`) state from the Fedora host.
* **Guest Availability:** Verifying that all four guests boot correctly and report expected hostnames.
* **Core Services:** Confirming `sshd`, `firewalld`, and RPC services are active and persistent.
* **Storage Integrity:** Validating the extra disk (`vdb`) and persistent XFS filesystems on `srv-admin`.
* **NFS/Autofs Ecosystem:** Confirming `srv-storage` exports are visible and clients trigger on-demand mounts correctly.
* **Security Posture:** Confirming SELinux remains in `Enforcing` mode across the entire lab.
* **Final Evidence:** Capturing the ultimate set of screenshots proving lab-wide integration.

### **Excluded:**
* Implementation of new features or services.
* Remediation of major architectural changes.
* Application-layer stress testing.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Final Integrated Validation |
| **Validation Model** | Full-lab Integrated Acceptance Check |
| **Guest Set** | `srv-admin`, `srv-web`, `srv-db`, `srv-storage` |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f11-final-integrated-validation` |
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows a **Host → Reference Node → Server → Clients → Cross-Node** flow:

1.  **Host-Side Validation:** Confirm all libvirt resources and guests are healthy from the Fedora host.
2.  **Reference Node Validation (`srv-admin`):** Validate local storage, persistent mounts, and basic connectivity.
3.  **Server Validation (`srv-storage`):** Confirm NFS export health and server-side listener state.
4.  **Client Node Validation (`srv-web`, `srv-db`):** Confirm shared access and security contexts.
5.  **Cross-Node Acceptance Check:** Final ping and mount triggers to confirm unified behavior.
6.  **Closure:** Capture final evidence and finalize the project documentation.

---

## 📐 Workspace Design
Phase 11 operations are organized under a final integrated directory structure on each guest:

```text
~/lab/f11-final-integrated-validation/
├── system/      # Hostname, uptime, and kernel validation
├── network/     # IP addressing and reachability evidence
├── services/    # Core service state (sshd, firewalld)
├── storage/     # Local disk, NFS, and autofs evidence
├── security/    # SELinux state and security evidence
└── tmp/         # Temporary validation area
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-admin` | Operations Reference | ⚪ Pending |
| `srv-web` | Application Client | ⚪ Pending |
| `srv-db` | Database Client | ⚪ Pending |
| `srv-storage` | Storage & NFS Server | ⚪ Pending |

### **Phase 11 Planned Milestones**
* [ ] Host-side libvirt and network validation completed.
* [ ] `srv-admin` local storage and persistent mount health confirmed.
* [ ] `srv-storage` NFS export visibility validated.
* [ ] `autofs` on-demand mount triggers verified on all clients.
* [ ] SELinux `Enforcing` mode confirmed lab-wide.
* [ ] Lab-wide connectivity and service reachability verified.
* [ ] Final screenshot evidence captured for project closure.

---

## 🧪 Planned Validation Areas

### 1. Host-Side Validation (Fedora Host)
```bash
sudo virsh -c qemu:///system list --all
sudo virsh -c qemu:///system pool-list --all
sudo virsh -c qemu:///system net-list --all
```

### 2. Integrated Validation on srv-admin
```bash
hostnamectl --static
ip -brief address
systemctl is-active sshd firewalld
getenforce
lsblk
findmnt /mnt/phase07-demo
cat /mnt/phase09-auto/README.txt
```

### 3. Integrated Validation on srv-storage
```bash
hostnamectl --static
systemctl is-active nfs-server rpcbind
sudo exportfs -v
showmount -e localhost
getenforce
```

### 4. Integrated Validation on srv-web and srv-db
```bash
hostnamectl --static
systemctl is-active sshd firewalld
cat /mnt/phase09-auto/README.txt
getenforce
```

### 5. Cross-Node Acceptance Check
```bash
ping -c 2 192.168.150.115
mount | grep phase09-auto
```

---

## 📸 Planned Screenshot Inventory
Evidence stored in `assets/screenshots/phase-11/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P11-01** | Host-side libvirt and network overview | Fedora Host |
| **P11-02** | `srv-admin` integrated validation | `srv-admin` |
| **P11-03** | `srv-storage` NFS and service state | `srv-storage` |
| **P11-04** | `srv-web` integrated validation | `srv-web` |
| **P11-05** | `srv-db` integrated validation | `srv-db` |
| **P11-06** | Final cross-node connectivity check | Lab-wide |

---

## 🔗 Related Files
* `runbooks/f11-final-integrated-validation.md` — Final validation procedure.
* `phases/10-selinux-troubleshooting/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Final guest state tracker.

---

## 🏁 Current Outcome
Phase 11 is **Not Started** ⚪.
