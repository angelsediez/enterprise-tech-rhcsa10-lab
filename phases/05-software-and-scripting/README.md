# 📦 Phase 05 - Software and Scripting

## 🎯 Objective
Establish and validate a controlled Phase 05 baseline for software inspection and basic scripting across the guest set. 

This phase focuses on documenting how installed software is inspected, how repository state is queried, and how simple Bash scripts are created, executed, and validated to ensure environment consistency.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Software Baseline:** Inspecting installed packages and repository state.
* **Package Evidence:** Storing command output related to installed software and repository visibility.
* **Scripting Workspace:** Creating a dedicated Phase 05 workspace inside the guest.
* **Script Execution:** Developing and running simple Bash scripts.
* **Permissions:** Applying executable permissions to scripts.
* **Logging:** Capturing script output into persistent log files.
* **Evidence:** Capturing screenshots and command validation results.

### **Excluded (Later Phases):**
* Advanced package lifecycle workflows.
* Service deployment and daemon-specific configuration.
* Complex shell scripting (loops, conditionals, functions, arrays).
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
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase follows the established "Master & Replicate" model:

1. **Full Reference Workflow:** Execute the complete software and scripting workflow on `srv-admin`.
2. **Validation Pattern:** Apply a shorter validation block on `srv-web`, `srv-db`, and `srv-storage`.
3. **Evidence:** Capture screenshots of package inspection, repository state, script execution, and stored logs.
4. **Closure:** Confirm that the validated Phase 05 pattern exists across all four guests.

---

## 📐 Workspace Design
Phase 05 operations are isolated within a dedicated directory structure:

```text
~/lab/f05-software-and-scripting/
├── packages/    # Package query evidence
├── repos/       # Repository state evidence
├── scripts/     # Bash scripts created during the phase
├── logs/        # Script execution logs
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

### **Phase 05 Achievements**
* [x] Phase 05 workspace created on `srv-admin`.
* [x] Installed package baseline validated and stored.
* [x] Repository state captured on `srv-admin`.
* [x] Simple Bash scripts created and executed with correct permissions.
* [x] Script output captured into persistent log files.
* [x] Validation pattern replicated on `srv-web`, `srv-db`, and `srv-storage`.
* [x] Screenshot evidence captured for all planned validation points.

> [!NOTE]
> In this lab environment, `dnf repolist` may return `No repositories available` when the guest is not subscribed and no local repository is configured. This is still valid Phase 05 evidence because it documents the current repository state.

---

## 🧪 srv-admin Validation Areas

### 1. Software Baseline
```bash
rpm -q bash
rpm -q openssh-server
dnf repolist
dnf list installed | head -n 20
```

### 2. Evidence Storage
```bash
rpm -q bash > packages/bash.txt
rpm -q openssh-server > packages/openssh-server.txt
dnf repolist > repos/repolist.txt
dnf list installed > packages/installed.txt
find packages repos -maxdepth 1 -type f | sort
cat packages/bash.txt
cat packages/openssh-server.txt
```

### 3. Script Creation and Execution
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

### 5. Final Workspace Validation
```bash
find ~/lab/f05-software-and-scripting -maxdepth 3 -type f | sort
ls -l ~/lab/f05-software-and-scripting/scripts
ls -l ~/lab/f05-software-and-scripting/logs
cat ~/lab/f05-software-and-scripting/logs/package-check.log
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
The following condensed validation block was executed on each secondary guest:

```bash
mkdir -p ~/lab/f05-software-and-scripting/{packages,repos,scripts,logs}
cd ~/lab/f05-software-and-scripting
rpm -q bash > packages/bash.txt
dnf repolist > repos/repolist.txt
cat > scripts/node-info.sh <<'EOF'
#!/usr/bin/env bash
echo "host=$(hostnamectl --static)"
echo "user=$(whoami)"
EOF
chmod 750 scripts/node-info.sh
./scripts/node-info.sh > logs/node-info.log
cat logs/node-info.log
find ~/lab/f05-software-and-scripting -maxdepth 3 -type f | sort
```

---

## 📸 Screenshot Inventory
Evidence is stored in `assets/screenshots/phase-05/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P05-01** | Installed package baseline | `srv-admin` |
| **P05-02** | Repository state baseline | `srv-admin` |
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
Phase 05 is complete ✅.

A full software inspection and scripting workflow was executed and validated on `srv-admin`. A shorter validation pattern was then successfully reused on `srv-web`, `srv-db`, and `srv-storage`.

The lab now has a documented and validated Phase 05 software and scripting baseline across all four guests, including package inspection evidence, repository state capture, basic Bash scripting, executable permission handling, and script log generation.
