# ✅ Phase 06 Validation Checklist - Running Systems and Service Management

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 06 across the four planned RHEL 10.1 guests.

It confirms that running service inspection, `systemd` state validation, journal review, and controlled custom service management were executed successfully and documented with evidence.

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
- [x] Phase 06 workspace created under `~/lab/f06-running-systems-service-management`
- [x] Working directory validated with `pwd`
- [x] Active user validated with `whoami`
- [x] Hostname validated with `hostnamectl --static`

### Running Service Baseline
- [x] `systemctl list-units --type=service --state=running | head -n 20` executed successfully
- [x] `systemctl status sshd --no-pager -l` executed successfully
- [x] `systemctl is-active sshd` executed successfully
- [x] `systemctl is-enabled sshd` executed successfully

### Stored Service Evidence
- [x] `services/running-services.txt` created successfully
- [x] `services/sshd-status.txt` created successfully
- [x] `services/sshd-active.txt` created successfully
- [x] `services/sshd-enabled.txt` created successfully
- [x] service evidence files listed successfully

### Journal Review
- [x] `journalctl -u sshd --no-pager | tail -n 30` executed successfully
- [x] `journalctl -b --no-pager | tail -n 30` executed successfully

### Custom Service Script
- [x] `systemd/phase06-demo.sh` created successfully
- [x] `systemd/phase06-demo.sh` marked executable with `chmod 750`
- [x] `/usr/local/bin/phase06-demo.sh` copied successfully
- [x] `/usr/local/bin/phase06-demo.sh` marked executable

### Custom Systemd Unit
- [x] `systemd/phase06-demo.service` created successfully
- [x] unit includes `Type=oneshot`
- [x] unit includes `RemainAfterExit=yes`
- [x] unit includes `WantedBy=multi-user.target`
- [x] unit copied successfully to `/etc/systemd/system/`

### Custom Service Validation
- [x] `sudo systemctl daemon-reload` executed successfully
- [x] `sudo systemctl start phase06-demo.service` executed successfully
- [x] `sudo systemctl enable phase06-demo.service` executed successfully
- [x] `systemctl status phase06-demo.service --no-pager -l` executed successfully
- [x] `systemctl is-active phase06-demo.service` returned a valid state
- [x] `systemctl is-enabled phase06-demo.service` returned `enabled`
- [x] `/tmp/phase06-demo.log` confirms service execution

### Final Workspace Validation
- [x] final workspace file listing completed
- [x] `systemd/` contents reviewed successfully
- [x] stored `sshd` active state reviewed successfully
- [x] stored `sshd` enabled state reviewed successfully

---

## 🔁 Short Validation - `srv-web`
- [x] workspace created
- [x] `services/sshd-status.txt` created
- [x] `services/sshd-active.txt` created
- [x] `services/sshd-enabled.txt` created
- [x] `logs/sshd-journal-tail.txt` created
- [x] `cat services/sshd-active.txt` executed successfully
- [x] `cat services/sshd-enabled.txt` executed successfully
- [x] journal tail output reviewed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P06-06-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [x] workspace created
- [x] `services/sshd-status.txt` created
- [x] `services/sshd-active.txt` created
- [x] `services/sshd-enabled.txt` created
- [x] `logs/sshd-journal-tail.txt` created
- [x] `cat services/sshd-active.txt` executed successfully
- [x] `cat services/sshd-enabled.txt` executed successfully
- [x] journal tail output reviewed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P06-07-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [x] workspace created
- [x] `services/sshd-status.txt` created
- [x] `services/sshd-active.txt` created
- [x] `services/sshd-enabled.txt` created
- [x] `logs/sshd-journal-tail.txt` created
- [x] `cat services/sshd-active.txt` executed successfully
- [x] `cat services/sshd-enabled.txt` executed successfully
- [x] journal tail output reviewed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P06-08-final-workspace-srv-storage.png`

---

## 📸 Screenshot Inventory
- [x] `P06-01-running-services-srv-admin.png`
- [x] `P06-02-sshd-status-srv-admin.png`
- [x] `P06-03-journalctl-srv-admin.png`
- [x] `P06-04-custom-service-status-srv-admin.png`
- [x] `P06-05-custom-service-log-srv-admin.png`
- [x] `P06-06-final-workspace-srv-web.png`
- [x] `P06-07-final-workspace-srv-db.png`
- [x] `P06-08-final-workspace-srv-storage.png`

---

## 🏁 Phase 06 Exit Criteria
Phase 06 is considered complete when:

- [x] `srv-admin` full workflow completed and validated
- [x] `srv-web` short validation completed
- [x] `srv-db` short validation completed
- [x] `srv-storage` short validation completed
- [x] all expected screenshots captured
- [x] `phases/06-running-systems-service-management/README.md` updated
- [x] `runbooks/f06-running-systems-service-management.md` updated
- [x] this checklist reviewed and completed

---

## 📝 Notes
- [x] `phase06-demo.service` was validated as `active (exited)` with `RemainAfterExit=yes`
- [x] the custom demo service was only deployed on `srv-admin`
- [x] secondary guests used the shorter `sshd` service-management validation pattern
