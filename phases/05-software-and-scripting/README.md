# 📦 Phase 05 - Software and Scripting

## 🎯 Objective
Establish and validate a controlled Phase 05 baseline for software management and basic scripting across the guest set. This phase focuses on documenting how software is inspected, how repository information is queried, and how simple Bash scripts are created, executed, and validated to ensure environment consistency.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Software Baseline:** Inspecting installed packages and repository configurations.
* **Package Evidence:** Storing command output related to installed software and enabled repositories.
* **Scripting Workspace:** Creating a dedicated Phase 05 workspace inside the guest.
* **Script Execution:** Developing and running simple Bash scripts.
* **Permissions:** Applying executable permissions to scripts via numeric and symbolic modes.
* **Logging:** Capturing script output into persistent log files for auditing.
* **Evidence:** Capturing screenshots and command validation results.

### **Excluded (Later Phases):**
* Advanced package lifecycle workflows (Installation/Removal).
* Service deployment and daemon-specific configuration.
* Complex shell scripting (Loops, conditionals, or functions).
* CI/CD-style automation.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Software and Scripting |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation Pattern) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f05-software-and-scripting` |
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1. **Full Reference Workflow:** Execute the complete software and scripting workflow on `srv-admin`.
2. **Validation Pattern:** Apply a shorter validation block on `srv-web`, `srv-db`, and `srv-storage`.
3. **Evidence:** Capture screenshots of package inspection, repository queries, script execution, and stored logs.
4. **Closure:** Confirm that the validated Phase 05 pattern exists across all four guests.

---

## 📐 Workspace Design
Phase 05 operations are isolated within a dedicated directory structure:

```text
~/lab/f05-software-and-scripting/
├── packages/    # Package query evidence
├── repos/       # Repository query evidence
├── scripts/     # Bash scripts created during the phase
├── logs/        # Script execution logs
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

### **Phase 05 Planned Milestones**
* [ ] Phase 05 workspace created on `srv-admin`.
* [ ] Installed package baseline validated and stored.
* [ ] Repository baseline validated on `srv-admin`.
* [ ] Simple Bash scripts created and executed with correct permissions.
* [ ] Script output captured into persistent log files.
* [ ] Validation pattern replicated on secondary guests.
* [ ] Screenshot evidence captured for planned validation points.

---

## 🧪 Planned srv-admin Validation Areas

### 1. Software Baseline
Performing initial inspection of the guest environment:
```bash
rpm -q bash
rpm -q openssh-server
dnf repolist
dnf list installed | head -n 20
```

### 2. Evidence Storage
Capturing the baseline state into the workspace:
```bash
rpm -q bash > packages/bash.txt
dnf repolist > repos/repolist.txt
dnf list installed > packages/installed.txt
find packages repos -maxdepth 1 -type f | sort
```

### 3. Script Creation and Execution
Creating a diagnostic script to validate environment context:
```bash
cat > scripts/phase05-info.sh <<'EOF'
#!/usr/bin/env bash
echo "host=$(hostnamectl --static)"
echo "user=$(whoami)"
echo "pwd=$(pwd)"
EOF

chmod 750 scripts/phase05-info.sh
./scripts/phase05-info.sh
```

### 4. Script Output Logging
Automating package checks and storing results:
```bash
cat > scripts/package-check.sh <<'EOF'
#!/usr/bin/env bash
rpm -q bash
rpm -q openssh-server
EOF

chmod 750 scripts/package-check.sh
./scripts/package-check.sh > logs/package-check.log
cat logs/package-check.log
```

---

## 📸 Planned Screenshot Inventory
Evidence will be stored in `assets/screenshots/phase-05/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P05-01** | Installed package baseline | `srv-admin` |
| **P05-02** | Repository baseline | `srv-admin` |
| **P05-03** | Script execution validation | `srv-admin` |
| **P05-04** | Script log validation | `srv-admin` |
| **P05-05** | Replicated workspace validation | `srv-web` |
| **P05-06** | Replicated workspace validation | `srv-db` |
| **P05-07** | Replicated workspace validation | `srv-storage` |

---

## 🔗 Related Files
* `runbooks/f05-software-and-scripting.md` — Detailed step-by-step guide.
* `phases/04-identity-ssh-permissions/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 05 is **Not Started** ⚪.

The lab is currently awaiting the execution of software inspection and basic scripting workflows to establish a manageable automation baseline across all nodes.

