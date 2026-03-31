# 🛠️ Phase 08 Runbook: Networking and Firewall

## 🎯 Objective
Execute and validate a practical Phase 08 workflow focused on networking and firewall management across the guest set. This runbook ensures that every node has a predictable network configuration, valid hostname resolution, and a secured `firewalld` baseline.

**Key Goals:**
- Interface and IPv4 address inspection.
- Routing table and gateway visibility.
- Hostname and DNS resolver verification.
- Listener/Socket inspection (`ss`).
- `firewalld` state, zone, and service validation.
- Execution of a controlled, persistent firewall rule change.

This runbook uses `srv-admin` as the **full reference workflow** and applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included**
* **Network Inspection:** Analysis of interfaces, addresses, and kernel routing tables.
* **Service Discovery:** Identification of listening ports and active sockets.
* **Security Perimeter:** `firewalld` service status and active zone mapping.
* **Rule Management:** Documented addition of the `http` service and persistence validation.
* **Connectivity Testing:** ICMP verification to the local lab gateway.
* **Evidence Persistence:** Structured capture of network states into the Phase 08 workspace.

### **Excluded**
* Advanced routing protocols (OSPF, BGP) or policy routing.
* Link aggregation (Bonding/Teaming) or bridge configuration.
* VPN or encrypted tunnel setup.
* Public-facing service publishing or NAT rules.

---

## 💻 Prerequisites

Before using this runbook, confirm:
| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phase 07 complete | Review `phases/07-local-storage-filesystems/README.md` |
| **Guest Availability** | All four guests operational | `virsh list --all` |
| **Network Manager** | Active on all guests | `systemctl is-active NetworkManager` |
| **Firewall Service** | Active on all guests | `systemctl is-active firewalld` |

> [!IMPORTANT]
> This phase validates and documents the guest network baseline. Do **not** modify IP addressing or hostnames unless they deviate from your lab's static design. Keep all firewall changes controlled and documented.

---

## 📍 Primary Workspace

All work in this phase is performed under:
`~/lab/f08-networking-firewall`

**Expected Structure:**
```text
~/lab/f08-networking-firewall/
├── network/     # Interface, address, and DNS logs
├── firewall/    # firewalld status, zones, and rule logs
├── validation/  # Connectivity and listener evidence
└── tmp/          # Temporary testing area
```

---

## 🚀 Full Workflow on srv-admin

### 1) Create Workspace and Validate Context
```bash
mkdir -p ~/lab/f08-networking-firewall/{network,firewall,validation,tmp}
cd ~/lab/f08-networking-firewall
pwd && whoami && hostnamectl --static
find ~/lab/f08-networking-firewall -maxdepth 2 -type d | sort
```

### 2) Network Baseline
```bash
ip -brief address
ip route
hostnamectl
nmcli device status
cat /etc/resolv.conf
```

### 3) Save Network Evidence
```bash
ip -brief address > network/ip-brief-address.txt
ip route > network/ip-route.txt
hostnamectl > network/hostnamectl.txt
nmcli device status > network/nmcli-device-status.txt
cat /etc/resolv.conf > network/resolv-conf.txt
find network -maxdepth 1 -type f | sort
```

### 4) Listener and Connectivity Validation
```bash
ss -tulpn
ping -c 2 192.168.150.1
```

### 5) Firewall Baseline
```bash
systemctl status firewalld --no-pager -l
firewall-cmd --state
firewall-cmd --get-active-zones
firewall-cmd --list-all
```

### 6) Save Firewall Evidence
```bash
systemctl status firewalld --no-pager -l > firewall/firewalld-status.txt
firewall-cmd --state > firewall/firewalld-state.txt
firewall-cmd --get-active-zones > firewall/active-zones.txt
firewall-cmd --list-all > firewall/list-all.txt
```

### 7) Controlled Firewall Validation
```bash
# Add HTTP service to the runtime configuration
sudo firewall-cmd --add-service=http
firewall-cmd --list-services

# Persist the change
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --reload

# Verify persistence
firewall-cmd --list-services
```

### 8) Save Validation Evidence
```bash
ss -tulpn > validation/ss-tulpn.txt
ping -c 2 192.168.150.1 > validation/ping-gateway.txt
firewall-cmd --list-services > firewall/list-services-after-http.txt
find validation firewall -maxdepth 1 -type f | sort
```

### 9) Final Phase 08 Validation
```bash
find ~/lab/f08-networking-firewall -maxdepth 3 -type f | sort
cat firewall/firewalld-state.txt
cat firewall/list-services-after-http.txt
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)

Run this block on each secondary guest to document the network and security state:
```bash
mkdir -p ~/lab/f08-networking-firewall/{network,firewall,validation}
cd ~/lab/f08-networking-firewall
ip -brief address > network/ip-brief-address.txt
ip route > network/ip-route.txt
nmcli device status > network/nmcli-device-status.txt
firewall-cmd --state > firewall/firewalld-state.txt
firewall-cmd --get-active-zones > firewall/active-zones.txt
firewall-cmd --list-all > firewall/list-all.txt
find ~/lab/f08-networking-firewall -maxdepth 3 -type f | sort
cat network/ip-brief-address.txt
cat firewall/firewalld-state.txt
```

---

## 🧪 Validation Targets

### **srv-admin**
- [ ] Workspace created successfully.
- [ ] Interface, route, and hostname baseline inspected.
- [ ] DNS resolver (`resolv.conf`) evidence captured.
- [ ] Socket listener state (`ss`) captured.
- [ ] Gateway connectivity (ICMP) verified.
- [ ] `firewalld` service status and state validated.
- [ ] Active zone mapping confirmed.
- [ ] Controlled **HTTP** service rule added and persisted.
- [ ] Firewall evidence files stored in the workspace.
- [ ] Final workspace recursive validation completed.

### **Secondary Guests (srv-web, srv-db, srv-storage)**
- [ ] Workspace created successfully.
- [ ] `network/` and `firewall/` evidence files generated.
- [ ] Interface and firewall states reviewed successfully.
- [ ] Final workspace listing completed on **srv-web**.
- [ ] Final workspace listing completed on **srv-db**.
- [ ] Final workspace listing completed on **srv-storage**.

---

## 🧯 Short Troubleshooting

1.  **`firewall-cmd --state` fails:** Check if the service is active: `systemctl status firewalld`.
2.  **`nmcli` output is empty:** Ensure the `NetworkManager` service is running.
3.  **Ping fails:** Validate your `ip route` and ensure the gateway IP is correct.
4.  **Runtime-to-Permanent fails:** Ensure the runtime change was successful first.

---

## 📸 Expected Screenshots
Store evidence in: `assets/screenshots/phase-08/`

* `P08-01-interface-address-baseline-srv-admin.png`
* `P08-02-routing-hostname-baseline-srv-admin.png`
* `P08-03-firewalld-state-zones-srv-admin.png`
* `P08-04-controlled-firewall-rule-srv-admin.png`
* `P08-05-listener-connectivity-srv-admin.png`
* `P08-06-final-workspace-srv-web.png`
* `P08-07-final-workspace-srv-db.png`
* `P08-08-final-workspace-srv-storage.png`

---

## 🏁 Outcome
Successful execution leaves a validated Phase 08 networking and firewall workspace across all nodes. The lab is now prepared for **Phase 09: NFS and Autofs**.
