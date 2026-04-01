# 🌐 Phase 08 - Networking and Firewall

## 🎯 Objective
Establish and validate a controlled Phase 08 baseline for networking and firewall management across the guest set.

This phase focuses on documenting interface inspection, IP addressing, routing visibility, hostname resolution, listener inspection, and `firewalld` state validation to ensure a predictable network baseline for later service exposure and inter-node communication.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Network Baseline:** Inspecting interfaces, addresses, routes, and hostname configuration.
* **Network Evidence:** Capturing `ip`, `hostnamectl`, `nmcli`, and resolver state into persistent files.
* **Listener Validation:** Inspecting active listeners and socket state with `ss`.
* **Firewall Baseline:** Inspecting `firewalld` state, active zones, assigned interfaces, and allowed services.
* **Controlled Firewall Validation:** Testing a small, documented firewall change and validating the resulting state.
* **Persistence:** Confirming that the documented firewall configuration remains applied after reload.
* **Evidence:** Capturing screenshots and command validation results.

### **Excluded (Later Phases):**
* Advanced routing or policy routing.
* Network bonding, teaming, or bridges inside the guests.
* VPN configuration.
* Public service publishing beyond the controlled Phase 08 scope.
* Complex SELinux and firewall integration for production apps.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Networking and Firewall |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation Pattern) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f08-networking-firewall` |
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1. **Full Reference Workflow:** Execute the complete networking and firewall workflow on `srv-admin`.
2. **Validation Pattern:** Apply a shorter inspection-focused validation block on `srv-web`, `srv-db`, and `srv-storage`.
3. **Evidence:** Capture screenshots of interface state, IP addressing, routes, firewall zones, controlled firewall rule persistence, and listener/connectivity validation.
4. **Closure:** Confirm that the validated Phase 08 baseline exists across all four guests.

---

## 📐 Workspace Design
Phase 08 operations are isolated within a dedicated directory structure:

```text
~/lab/f08-networking-firewall/
├── network/     # Interface, address, route, and DNS evidence
├── firewall/    # firewalld state, zones, and rules
├── validation/  # Connectivity and listener checks
└── tmp/         # Temporary tests
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-admin` | Reference Node | ✅ Completed & Validated |
| `srv-web` | Web Service | ✅ Validation Pattern Completed |
| `srv-db` | Database Node | ✅ Validation Pattern Completed |
| `srv-storage` | Storage Node | ✅ Validation Pattern Completed |

### **Phase 08 Achievements**
* [x] Phase 08 workspace created on `srv-admin`.
* [x] Network baseline validated and exported.
* [x] Interface, address, and route evidence captured.
* [x] `firewalld` state, zone mapping, and allowed service evidence captured.
* [x] Controlled firewall change documented and validated on `srv-admin`.
* [x] Validation pattern replicated on `srv-web`, `srv-db`, and `srv-storage`.
* [x] Screenshot evidence captured for all planned points.

---

## 🧪 srv-admin Validation Areas

### 1. Network Baseline
Inspecting the active guest network state:
```bash
ip -brief address
ip route
hostnamectl
nmcli device status
cat /etc/resolv.conf
```

### 2. Network Evidence Capture
Capturing the baseline state into the workspace:
```bash
ip -brief address > network/ip-brief-address.txt
ip route > network/ip-route.txt
hostnamectl > network/hostnamectl.txt
nmcli device status > network/nmcli-device-status.txt
cat /etc/resolv.conf > network/resolv-conf.txt
find network -maxdepth 1 -type f | sort
```

### 3. Listener and Connectivity Validation
```bash
ss -tulpn
ping -c 2 192.168.150.1
```

### 4. Firewall Baseline & Evidence Capture
```bash
systemctl status firewalld --no-pager -l
sudo firewall-cmd --state
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all

systemctl status firewalld --no-pager -l > firewall/firewalld-status.txt
sudo firewall-cmd --state > firewall/firewalld-state.txt
sudo firewall-cmd --get-active-zones > firewall/active-zones.txt
sudo firewall-cmd --list-all > firewall/list-all.txt
```

### 5. Controlled Firewall Validation
```bash
sudo firewall-cmd --add-service=http
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-services
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
The following inspection-focused validation block was executed on each secondary guest:

```bash
mkdir -p ~/lab/f08-networking-firewall/{network,firewall,validation}
cd ~/lab/f08-networking-firewall
ip -brief address > network/ip-brief-address.txt
ip route > network/ip-route.txt
nmcli device status > network/nmcli-device-status.txt
sudo firewall-cmd --state > firewall/firewalld-state.txt
sudo firewall-cmd --get-active-zones > firewall/active-zones.txt
sudo firewall-cmd --list-all > firewall/list-all.txt
find ~/lab/f08-networking-firewall -maxdepth 3 -type f | sort
cat network/ip-brief-address.txt
cat firewall/firewalld-state.txt
cat firewall/active-zones.txt
```

> [!NOTE]
> The secondary guests used the inspection-only validation pattern. The controlled http firewall service addition was documented only on srv-admin as the full reference workflow.

---

## 📸 Screenshot Inventory
Evidence stored in `assets/screenshots/phase-08/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P08-01** | Interface and address baseline | `srv-admin` |
| **P08-02** | Routing and hostname baseline | `srv-admin` |
| **P08-03** | Firewalld state and active zone validation | `srv-admin` |
| **P08-04** | Controlled firewall rule validation | `srv-admin` |
| **P08-05** | Listener and connectivity validation | `srv-admin` |
| **P08-06** | Final replicated network/firewall workspace | `srv-web` |
| **P08-07** | Final replicated network/firewall workspace | `srv-db` |
| **P08-08** | Final replicated network/firewall workspace | `srv-storage` |

---

## 🔗 Related Files
* `runbooks/f08-networking-firewall.md` — Detailed step-by-step guide.
* `validation/08-networking-firewall-checklist.md` — Phase 08 validation checklist.
* `phases/07-local-storage-filesystems/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 08 is complete ✅.
A full networking and firewall workflow was executed and validated on srv-admin. A shorter inspection-focused validation pattern was then successfully reused on srv-web, srv-db, and srv-storage.
The lab now has a documented and validated Phase 08 networking and firewall baseline across all four guests, preparing the environment for Phase 09 — NFS and Autofs.
