# ✅ Phase 08 Validation Checklist - Networking and Firewall

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 08 across the four planned RHEL 10.1 guests.

It confirms that network inspection, routing visibility, hostname and resolver review, listener inspection, `firewalld` baseline validation, a controlled firewall rule change on `srv-admin`, and replicated inspection-only validation on the secondary guests were executed successfully and documented with evidence.

---

## 💻 Guest Coverage

| Guest | Status |
| :--- | :--- |
| `srv-admin` | ✅ Complete |
| `srv-web` | ✅ Complete |
| `srv-db` | ✅ Complete |
| `srv-storage` | ✅ Complete |

---

## 🧪 Detailed Validation - `srv-admin`

### Workspace and Context
- [x] Phase 08 workspace created under `~/lab/f08-networking-firewall`
- [x] Working directory validated with `pwd`
- [x] Active user validated with `whoami`
- [x] Hostname validated with `hostnamectl --static`

### Network Baseline
- [x] `ip -brief address` executed successfully
- [x] `ip route` executed successfully
- [x] `hostnamectl` executed successfully
- [x] `nmcli device status` executed successfully
- [x] `/etc/resolv.conf` reviewed successfully

### Stored Network Evidence
- [x] `network/ip-brief-address.txt` created successfully
- [x] `network/ip-route.txt` created successfully
- [x] `network/hostnamectl.txt` created successfully
- [x] `network/nmcli-device-status.txt` created successfully
- [x] `network/resolv-conf.txt` created successfully
- [x] network evidence listing completed successfully

### Listener and Connectivity Validation
- [x] `ss -tulpn` executed successfully
- [x] `ping -c 2 192.168.150.1` executed successfully
- [x] listener state reviewed successfully
- [x] lab gateway reachability validated successfully

### Firewall Baseline
- [x] `systemctl status firewalld --no-pager -l` executed successfully
- [x] `firewall-cmd --state` executed successfully
- [x] `firewall-cmd --get-active-zones` executed successfully
- [x] `firewall-cmd --list-all` executed successfully
- [x] active zone and assigned interface reviewed successfully

### Stored Firewall Evidence
- [x] `firewall/firewalld-status.txt` created successfully
- [x] `firewall/firewalld-state.txt` created successfully
- [x] `firewall/active-zones.txt` created successfully
- [x] `firewall/list-all.txt` created successfully
- [x] firewall evidence listing completed successfully

### Controlled Firewall Validation
- [x] `http` service added successfully to runtime configuration
- [x] runtime service list reviewed successfully
- [x] runtime configuration persisted successfully with `runtime-to-permanent`
- [x] firewall reloaded successfully
- [x] `http` service remained present after reload
- [x] `firewall/list-services-after-http.txt` created successfully

### Final Validation Evidence
- [x] `validation/ss-tulpn.txt` created successfully
- [x] `validation/ping-gateway.txt` created successfully
- [x] final workspace recursive listing completed successfully
- [x] final firewall state reviewed successfully
- [x] final active zone review completed successfully
- [x] final post-change service list reviewed successfully

---

## 🔁 Short Validation - `srv-web`
- [x] workspace created
- [x] `network/ip-brief-address.txt` created
- [x] `network/ip-route.txt` created
- [x] `network/nmcli-device-status.txt` created
- [x] `firewall/firewalld-state.txt` created
- [x] `firewall/active-zones.txt` created
- [x] `firewall/list-all.txt` created
- [x] final workspace listing completed
- [x] interface state reviewed successfully
- [x] firewall state reviewed successfully
- [x] active zone reviewed successfully
- [x] screenshot captured as `P08-06-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [x] workspace created
- [x] `network/ip-brief-address.txt` created
- [x] `network/ip-route.txt` created
- [x] `network/nmcli-device-status.txt` created
- [x] `firewall/firewalld-state.txt` created
- [x] `firewall/active-zones.txt` created
- [x] `firewall/list-all.txt` created
- [x] final workspace listing completed
- [x] interface state reviewed successfully
- [x] firewall state reviewed successfully
- [x] active zone reviewed successfully
- [x] screenshot captured as `P08-07-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [x] workspace created
- [x] `network/ip-brief-address.txt` created
- [x] `network/ip-route.txt` created
- [x] `network/nmcli-device-status.txt` created
- [x] `firewall/firewalld-state.txt` created
- [x] `firewall/active-zones.txt` created
- [x] `firewall/list-all.txt` created
- [x] final workspace listing completed
- [x] interface state reviewed successfully
- [x] firewall state reviewed successfully
- [x] active zone reviewed successfully
- [x] screenshot captured as `P08-08-final-workspace-srv-storage.png`

---

## 📸 Screenshot Inventory
- [x] `P08-01-interface-address-baseline-srv-admin.png`
- [x] `P08-02-routing-hostname-baseline-srv-admin.png`
- [x] `P08-03-firewalld-state-zones-srv-admin.png`
- [x] `P08-04-controlled-firewall-rule-srv-admin.png`
- [x] `P08-05-listener-connectivity-srv-admin.png`
- [x] `P08-06-final-workspace-srv-web.png`
- [x] `P08-07-final-workspace-srv-db.png`
- [x] `P08-08-final-workspace-srv-storage.png`

---

## 🏁 Phase 08 Exit Criteria
Phase 08 is considered complete when:

- [x] `srv-admin` full workflow completed and validated
- [x] `srv-web` short validation completed
- [x] `srv-db` short validation completed
- [x] `srv-storage` short validation completed
- [x] all expected screenshots captured
- [x] `phases/08-networking-firewall/README.md` updated
- [x] `runbooks/f08-networking-firewall.md` updated
- [x] this checklist reviewed and completed

---

## 📝 Notes
- [x] the active interface validated across the guests was `enp1s0`
- [x] the active firewall zone observed in the lab was `public`
- [x] the controlled firewall service added on `srv-admin` was `http`
- [x] the service list after the controlled change included `http`
- [x] the lab gateway used for connectivity validation was `192.168.150.1`
- [x] the secondary guests used the inspection-only validation pattern
- [x] the controlled firewall rule change was documented only on `srv-admin`
