# 🌐 Phase 08 - Networking and Firewall

## 🎯 Objective
Establish and validate a controlled Phase 08 baseline for networking and firewall management across the guest set.

This phase focuses on documenting interface inspection, IP addressing, routing visibility, hostname resolution, and `firewalld` state validation to ensure a predictable network baseline for later service exposure and inter-node communication.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Network Baseline:** Inspecting interfaces, addresses, routes, and hostname configuration.
* **Network Evidence:** Capturing `ip`, `hostnamectl`, `nmcli`, and socket/listener state into persistent files.
* **Firewall Baseline:** Inspecting `firewalld` state, active zones, assigned interfaces, and allowed services.
* **Controlled Firewall Validation:** Testing a small, documented firewall change and validating the resulting state.
* **Persistence:** Confirming network/firewall configuration remains applied after reload or restart where appropriate.
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
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1. **Full Reference Workflow:** Execute the complete networking and firewall workflow on `srv-admin`.
2. **Validation Pattern:** Apply a shorter validation block on `srv-web`, `srv-db`, and `srv-storage`.
3. **Evidence:** Capture screenshots of interface state, IP addressing, routes, firewall zones, and controlled rule validation.
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
| `srv-admin` | Reference Node | ⚪ Pending |
| `srv-web` | Web Service | ⚪ Pending |
| `srv-db` | Database Node | ⚪ Pending |
| `srv-storage` | Storage Node | ⚪ Pending |

### **Phase 08 Achievements**
* [ ] Phase 08 workspace created on `srv-admin`.
* [ ] Network baseline validated and exported.
* [ ] Interface, address, and route evidence captured.
* [ ] `firewalld` state, zone mapping, and allowed service evidence captured.
* [ ] Controlled firewall change documented and validated on `srv-admin`.
* [ ] Validation pattern replicated on secondary guests.
* [ ] Screenshot evidence captured for all planned points.

---

## 🧪 Planned srv-admin Validation Areas

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

### 3. Connectivity and Listener Validation
Inspecting listeners and basic connectivity:
```bash
ss -tulpn
ping -c 2 192.168.150.1
```

### 4. Firewall Baseline
Inspecting the firewall service and active zone mapping:
```bash
systemctl status firewalld --no-pager -l
firewall-cmd --state
firewall-cmd --get-active-zones
firewall-cmd --list-all
```

### 5. Controlled Firewall Validation
Testing a small, documented firewall service rule:
```bash
sudo firewall-cmd --add-service=http
sudo firewall-cmd --runtime-to-permanent
firewall-cmd --list-services
sudo firewall-cmd --reload
firewall-cmd --list-services
```

---

## 📸 Planned Screenshot Inventory
Evidence stored in `assets/screenshots/phase-08/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P08-01** | Interface and address baseline | `srv-admin` |
| **P08-02** | Routing and hostname baseline | `srv-admin` |
| **P08-03** | Firewalld state and active zone validation | `srv-admin` |
| **P08-04** | Controlled firewall rule validation | `srv-admin` |
| **P08-05** | Listener and connectivity validation | `srv-admin` |
| **P08-06..08** | Final replicated network/firewall workspace | `srv-web/db/storage` |

---

## 🔗 Related Files
* `runbooks/f08-networking-firewall.md` — Detailed step-by-step guide.
* `phases/07-local-storage-filesystems/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 08 is **Not Started** ⚪.

The lab is currently awaiting the execution of the networking and firewall management workflow to establish a manageable connectivity baseline across all nodes.
