# 🛠️ Phase 06 Runbook: Running Systems and Service Management

## 🎯 Objective
Execute and validate a practical Phase 06 workflow focused on the core operational management of RHEL 10.1. This runbook establishes a baseline for inspecting running processes, managing `systemd` units, analyzing logs with `journalctl`, and implementing controlled custom services.

This runbook uses `srv-admin` as the **full reference workflow** and then applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included**
* **Service Inspection:** Guest-side analysis of active and loaded services.
* **State Validation:** `systemctl` checks for status, activity, and boot-time enablement.
* **Evidence Persistence:** Capturing service metadata into the Phase 06 workspace.
* **Log Analysis:** `journalctl` inspection for unit-specific and boot-level logs.
* **Custom Service Lifecycle:** Creation, deployment, and validation of a custom `systemd` service.
* **Final Integrity:** Recursive workspace validation across the guest set.

### **Excluded**
* Advanced `systemd` dependency mapping or socket activation.
* Complex unit relationships, timers, or slices.
* Network-facing service exposure or firewall tuning.
* Orchestration or multi-guest service synchronization.

---

## 💻 Prerequisites

Before using this runbook, confirm:
| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phase 05 complete | Review `phases/05-software-and-scripting/README.md` |
| **Guest Availability** | All four guests operational | `virsh list --all` |
| **Access Control** | Working user has sudo/local access | Console or terminal login confirmed |
| **Identity Baseline** | `jdoe` and `wheel` available | `getent passwd jdoe` and `getent group wheel` |

---

## 📍 Primary Workspace

All work in this phase is performed under:
`~/lab/f06-running-systems-service-management`

**Expected Structure:**
```text
~/lab/f06-running-systems-service-management/
├── services/    # Service status and activity evidence
├── systemd/     # Custom unit files and execution scripts
├── logs/        # Journal and service log exports
└── tmp/         # Temporary testing logs
```

---

## 🚀 Full Workflow on srv-admin

### 1) Create Workspace and Validate Context
```bash
mkdir -p ~/lab/f06-running-systems-service-management/{services,systemd,logs,tmp}
cd ~/lab/f06-running-systems-service-management
pwd && whoami && hostnamectl --static
find ~/lab/f06-running-systems-service-management -maxdepth 1 -type d | sort
```

### 2) Running Service Baseline
```bash
systemctl list-units --type=service --state=running | head -n 20
systemctl status sshd --no-pager -l
systemctl is-active sshd
systemctl is-enabled sshd
```

### 3) Save Service Evidence
```bash
systemctl list-units --type=service --state=running > services/running-services.txt
systemctl status sshd --no-pager -l > services/sshd-status.txt
systemctl is-active sshd > services/sshd-active.txt
systemctl is-enabled sshd > services/sshd-enabled.txt
find services -maxdepth 1 -type f | sort
```

### 4) Journal Review
```bash
journalctl -u sshd --no-pager | tail -n 30
journalctl -b --no-pager | tail -n 30
```

### 5) Create Custom Service Script
```bash
cat > systemd/phase06-demo.sh <<'EOF'
#!/usr/bin/env bash
echo "phase06-demo execution at $(date)" >> /tmp/phase06-demo.log
EOF

chmod 750 systemd/phase06-demo.sh
sudo cp systemd/phase06-demo.sh /usr/local/bin/phase06-demo.sh
sudo chmod 755 /usr/local/bin/phase06-demo.sh
```

### 6) Create Custom systemd Unit
```bash
cat > systemd/phase06-demo.service <<'EOF'
[Unit]
Description=Phase 06 Demo Service
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/phase06-demo.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
EOF
```

### 7) Load, Start, and Enable Custom Service
```bash
sudo cp systemd/phase06-demo.service /etc/systemd/system/phase06-demo.service
sudo systemctl daemon-reload
sudo systemctl enable --now phase06-demo.service
systemctl status phase06-demo.service --no-pager -l
systemctl is-active phase06-demo.service
systemctl is-enabled phase06-demo.service
cat /tmp/phase06-demo.log
```

### 8) Final Phase 06 Validation
```bash
find ~/lab/f06-running-systems-service-management -maxdepth 3 -type f | sort
ls -l systemd/
cat services/sshd-active.txt
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)

Run this block on each guest:
```bash
mkdir -p ~/lab/f06-running-systems-service-management/{services,systemd,logs}
cd ~/lab/f06-running-systems-service-management
systemctl status sshd --no-pager -l | head -n 20 > services/sshd-status.txt
systemctl is-active sshd > services/sshd-active.txt
systemctl is-enabled sshd > services/sshd-enabled.txt
journalctl -u sshd --no-pager | tail -n 20 > logs/sshd-journal-tail.txt
find ~/lab/f06-running-systems-service-management -maxdepth 3 -type f | sort
```

---

## 🧪 Validation Targets

### **srv-admin**
- [ ] Workspace tree confirmed via `find`.
- [ ] `services/running-services.txt` contains valid unit listing.
- [ ] `sshd` status, activity, and enablement captured.
- [ ] Custom script `/usr/local/bin/phase06-demo.sh` exists and is executable.
- [ ] Custom unit `/etc/systemd/system/phase06-demo.service` is loaded and active.
- [ ] `/tmp/phase06-demo.log` confirms service execution.

### **Secondary Guests**
- [ ] Workspace and `services/` subfolder exist.
- [ ] `sshd-active.txt` and `sshd-enabled.txt` contain valid states.
- [ ] `sshd-journal-tail.txt` contains recent SSH log entries.
- [ ] Final recursive listing reflects the completed validation pattern.

---

## 🧯 Short Troubleshooting

1. **`systemctl daemon-reload`:** Always run this after modifying or adding files to `/etc/systemd/system/`.
2. **`Type=oneshot`:** The demo service will finish after running the script. This is normal; use `RemainAfterExit=yes` so the status appears as `active (exited)`.
3. **Journal Empty:** If `journalctl` shows no data, check that the service has had recent activity or remove the `-u` filter to view the general system log.

---

## 📸 Expected Screenshots
Store screenshots in: `assets/screenshots/phase-06/`

* `P06-01-running-services-srv-admin.png`
* `P06-02-sshd-status-srv-admin.png`
* `P06-03-journalctl-srv-admin.png`
* `P06-04-custom-service-status-srv-admin.png`
* `P06-05-custom-service-log-srv-admin.png`
* `P06-06-final-workspace-srv-web.png`
* `P06-07-final-workspace-srv-db.png`
* `P06-08-final-workspace-srv-storage.png`

---

## 🏁 Outcome
The successful completion of this runbook ensures a validated Phase 06 workspace across all nodes, with evidence of service inspection, `systemd` management, and log control. The lab is now prepared for **Phase 07: Local Storage and Filesystems**.
