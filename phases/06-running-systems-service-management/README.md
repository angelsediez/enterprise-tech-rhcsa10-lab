# ⚙️ Phase 06 - Running Systems and Service Management

## 🎯 Objective
Establish and validate a controlled Phase 06 baseline for running systems and service management across the guest set. This phase focuses on documenting active service inspection, `systemd` unit management, log analysis via `journalctl`, and the lifecycle of custom service units.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Service Baseline:** Inspecting currently running services and critical unit states.
* **Systemd Evidence:** Querying `systemctl` for status, enablement, and activity state.
* **Journal Review:** Log inspection and filtering with `journalctl`.
* **Custom Service:** Developing, deploying, and validating a simple `systemd` service unit.
* **Service Persistence:** Verifying boot-time enablement and persistence.
* **Evidence:** Capturing screenshots and command validation results.

### **Excluded (Later Phases):**
* Complex multi-service application stacks.
* Network-facing service exposure and Firewall/SELinux tuning for services.
* Advanced `systemd` features (dependency graphs, timers, slices).
* Service orchestration (Ansible/Podman).

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Running Systems and Service Management |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation Pattern) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f06-running-systems-service-management` |
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1.  **Full Reference Workflow:** Execute complete service-management tests and custom unit deployment on `srv-admin`.
2.  **Validation Pattern:** Apply a condensed validation block on `srv-web`, `srv-db`, and `srv-storage`.
3.  **Evidence:** Capture screenshots of unit states, journals, and custom service logs.
4.  **Closure:** Confirm that the validated Phase 06 baseline exists across all four guests.

---

## 📐 Workspace Design
Phase 06 operations are isolated within a dedicated directory structure:

```text
~/lab/f06-running-systems-service-management/
├── services/    # Service status and running-unit evidence
├── systemd/     # Custom scripts and unit file templates
├── logs/        # Journal and service log exports
└── tmp/         # Temporary testing area
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

### **Phase 06 Planned Milestones**
* [ ] Phase 06 workspace created on `srv-admin`.
* [ ] Running service baseline validated and exported.
* [ ] `sshd` service state and enablement verified.
* [ ] `journalctl` evidence captured for system logs.
* [ ] Custom `systemd` service created, enabled, and validated.
* [ ] Validation pattern replicated on secondary guests.
* [ ] Screenshot evidence captured for all planned points.

---

## 🧪 Planned srv-admin Validation Areas

### 1. Running Service Baseline
Inspecting the active state of the guest:
```bash
systemctl list-units --type=service --state=running | head -n 20
systemctl status sshd --no-pager -l
systemctl is-active sshd
systemctl is-enabled sshd
```

### 2. Service Evidence Storage
Capturing the baseline state into the workspace:
```bash
systemctl list-units --type=service --state=running > services/running-services.txt
systemctl status sshd --no-pager -l > services/sshd-status.txt
find services -maxdepth 1 -type f | sort
```

### 3. Journal Review
Querying the system journal for specific unit logs:
```bash
journalctl -u sshd --no-pager | tail -n 30
journalctl -b --no-pager | tail -n 30
```

### 4. Custom Service Creation & Validation
Deploying a diagnostic `systemd` unit:
```bash
# 1. Create script
cat > systemd/phase06-demo.sh <<'EOF'
#!/usr/bin/env bash
echo "phase06-demo $(date)" >> /tmp/phase06-demo.log
EOF

# 2. Define service unit
cat > systemd/phase06-demo.service <<'EOF'
[Unit]
Description=Phase 06 Demo Service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/phase06-demo.sh

[Install]
WantedBy=multi-user.target
EOF

# 3. Deploy and validate
sudo cp systemd/phase06-demo.sh /usr/local/bin/
sudo cp systemd/phase06-demo.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now phase06-demo.service
systemctl status phase06-demo.service --no-pager -l
```

---

## 📸 Planned Screenshot Inventory
Evidence stored in `assets/screenshots/phase-06/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P06-01** | Running services baseline | `srv-admin` |
| **P06-02** | `sshd` status and state validation | `srv-admin` |
| **P06-03** | `journalctl` review (boot/unit) | `srv-admin` |
| **P06-04** | Custom service status and enablement | `srv-admin` |
| **P06-05** | Custom service execution log (`/tmp`) | `srv-admin` |
| **P06-06..08** | Final replicated workspace validation | `srv-web/db/storage` |

---

## 🔗 Related Files
* `runbooks/f06-running-systems-service-management.md` — Detailed step-by-step guide.
* `phases/05-software-and-scripting/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 06 is **Not Started** ⚪.

The lab is currently awaiting the execution of the system and service management workflow to establish a manageable operational baseline across all nodes.
