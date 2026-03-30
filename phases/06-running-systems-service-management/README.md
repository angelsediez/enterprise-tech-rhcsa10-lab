# ⚙️ Phase 06 - Running Systems and Service Management

## 🎯 Objective
Establish and validate a controlled Phase 06 baseline for running systems and service management across the guest set.

This phase focuses on documenting active service inspection, `systemd` unit management, log analysis via `journalctl`, and the lifecycle of a controlled custom service unit.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Service Baseline:** Inspecting currently running services and critical unit states.
* **Systemd Evidence:** Querying `systemctl` for status, enablement, and activity state.
* **Journal Review:** Log inspection and filtering with `journalctl`.
* **Custom Service:** Developing, deploying, and validating a simple `systemd` service unit.
* **Service Persistence:** Verifying boot-time enablement and post-execution active state.
* **Evidence:** Capturing screenshots and command validation results.

### **Excluded (Later Phases):**
* Complex multi-service application stacks.
* Network-facing service exposure and Firewall/SELinux tuning for services.
* Advanced `systemd` features (dependency graphs, timers, slices).
* Service orchestration across multiple guests.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Running Systems and Service Management |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation Pattern) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f06-running-systems-service-management` |
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1. **Full Reference Workflow:** Execute complete service-management tests and custom unit deployment on `srv-admin`.
2. **Validation Pattern:** Apply a condensed validation block on `srv-web`, `srv-db`, and `srv-storage`.
3. **Evidence:** Capture screenshots of unit states, journals, and custom service logs.
4. **Closure:** Confirm that the validated Phase 06 baseline exists across all four guests.

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
| `srv-admin` | Reference Node | ✅ Completed & Validated |
| `srv-web` | Web Service | ✅ Validation Pattern Completed |
| `srv-db` | Database Node | ✅ Validation Pattern Completed |
| `srv-storage` | Storage Node | ✅ Validation Pattern Completed |

### **Phase 06 Achievements**
* [x] Phase 06 workspace created on `srv-admin`.
* [x] Running service baseline validated and exported.
* [x] `sshd` service state and enablement verified.
* [x] `journalctl` evidence captured for service and boot logs.
* [x] Custom `systemd` service created, started, enabled, and validated.
* [x] Validation pattern replicated on `srv-web`, `srv-db`, and `srv-storage`.
* [x] Screenshot evidence captured for all planned points.

---

## 🧪 srv-admin Validation Areas

### 1. Running Service Baseline
```bash
systemctl list-units --type=service --state=running | head -n 20
systemctl status sshd --no-pager -l
systemctl is-active sshd
systemctl is-enabled sshd
```

### 2. Service Evidence Storage
```bash
systemctl list-units --type=service --state=running > services/running-services.txt
systemctl status sshd --no-pager -l > services/sshd-status.txt
systemctl is-active sshd > services/sshd-active.txt
systemctl is-enabled sshd > services/sshd-enabled.txt
find services -maxdepth 1 -type f | sort
```

### 3. Journal Review
```bash
journalctl -u sshd --no-pager | tail -n 30
journalctl -b --no-pager | tail -n 30
```

### 4. Custom Service Creation & Validation
```bash
# Script creation
cat > systemd/phase06-demo.sh <<'EOF'
#!/usr/bin/env bash
echo "phase06-demo $(date)" >> /tmp/phase06-demo.log
EOF

chmod 750 systemd/phase06-demo.sh
sudo cp systemd/phase06-demo.sh /usr/local/bin/phase06-demo.sh
sudo chmod 755 /usr/local/bin/phase06-demo.sh

# Unit definition
cat > systemd/phase06-demo.service <<'EOF'
[Unit]
Description=Phase 06 Demo Service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/phase06-demo.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
EOF

# Deployment and validation
sudo cp systemd/phase06-demo.service /etc/systemd/system/phase06-demo.service
sudo systemctl daemon-reload
sudo systemctl start phase06-demo.service
sudo systemctl enable phase06-demo.service
sudo systemctl status phase06-demo.service --no-pager -l
systemctl is-active phase06-demo.service
systemctl is-enabled phase06-demo.service
cat /tmp/phase06-demo.log
```

### 5. Final Workspace Validation
```bash
find ~/lab/f06-running-systems-service-management -maxdepth 3 -type f | sort
ls -l systemd/
cat services/sshd-active.txt
cat services/sshd-enabled.txt
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
The following condensed validation block was executed on each secondary guest:

```bash
mkdir -p ~/lab/f06-running-systems-service-management/{services,systemd,logs}
cd ~/lab/f06-running-systems-service-management
systemctl status sshd --no-pager -l | head -n 20 > services/sshd-status.txt
systemctl is-active sshd > services/sshd-active.txt
systemctl is-enabled sshd > services/sshd-enabled.txt
journalctl -u sshd --no-pager | tail -n 20 > logs/sshd-journal-tail.txt
find ~/lab/f06-running-systems-service-management -maxdepth 3 -type f | sort
cat services/sshd-active.txt
cat services/sshd-enabled.txt
cat logs/sshd-journal-tail.txt | tail -n 10
```

---

## 📸 Screenshot Inventory
Evidence is stored in `assets/screenshots/phase-06/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P06-01** | Running services baseline | `srv-admin` |
| **P06-02** | `sshd` status and state validation | `srv-admin` |
| **P06-03** | `journalctl` review (boot/unit) | `srv-admin` |
| **P06-04** | Custom service status and enablement | `srv-admin` |
| **P06-05** | Custom service execution log (`/tmp`) | `srv-admin` |
| **P06-06** | Final replicated workspace validation | `srv-web` |
| **P06-07** | Final replicated workspace validation | `srv-db` |
| **P06-08** | Final replicated workspace validation | `srv-storage` |

---

## 🔗 Related Files
* `runbooks/f06-running-systems-service-management.md` — Detailed step-by-step guide.
* `phases/05-software-and-scripting/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 06 is **complete** ✅.

A full running systems and service-management workflow was executed and validated on `srv-admin`, including running service inspection, `sshd` validation, `journalctl` review, and the lifecycle of a controlled custom `systemd` unit. 

A shorter validation pattern was then successfully reused on `srv-web`, `srv-db`, and `srv-storage`.

The lab now has a documented and validated Phase 06 service-management baseline across all four guests, preparing the environment for **Phase 07 — Local Storage and Filesystems**.
