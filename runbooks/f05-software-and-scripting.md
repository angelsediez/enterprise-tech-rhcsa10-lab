# 🛠️ Phase 05 Runbook: Software and Scripting

## 🎯 Objective
Execute and validate a practical Phase 05 workflow focused on software inspection and basic scripting across the guest set.

This runbook establishes a baseline for:
- installed package inspection
- repository state inspection
- evidence capture into persistent files
- simple Bash script creation
- script execution with controlled permissions
- output logging and workspace validation

This runbook uses `srv-admin` as the **full reference workflow** and then applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Software Inspection:** Querying installed packages and repository state.
* **Evidence Capture:** Redirecting package and repository output into persistent files.
* **Script Development:** Creating functional Bash scripts with proper shebangs.
* **Execution Control:** Validating executable permissions (`750`) for scripts.
* **Log Management:** Capturing script output into log files.
* **Workspace Validation:** Final tree-state confirmation across all guests.

### **Excluded (Later Phases):**
* Advanced package lifecycle workflows.
* Complex software deployment stacks.
* Cross-guest automation.
* Complex scripting logic (functions, arrays, loops, advanced regex).

---

## 💻 Prerequisites
Before proceeding, confirm the following baseline conditions:

| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phase 04 complete | Review `phases/04-identity-ssh-permissions/README.md` |
| **Guest Availability** | All four guests operational | `virsh list --all` |
| **Access Control** | Working user has local access | Console or terminal login confirmed |
| **DNF Visibility** | `rpm` and `dnf` commands available | `rpm -q bash` and `dnf repolist` |

> [!NOTE]
> In this lab environment, `dnf repolist` may return `No repositories available` if the guest is not subscribed and no local repository is configured. This is still valid Phase 05 evidence because it documents the current repository state.

---

## 📍 Primary Workspace
All operations are centralized within the following structure:

```text
~/lab/f05-software-and-scripting/
├── packages/    # Stored package query results
├── repos/       # Repository state exports
├── scripts/     # Functional Bash scripts
├── logs/        # Script execution logs
└── tmp/         # Temporary testing area
```

---

## 🚀 Step-by-Step Execution: srv-admin (Full Workflow)

### 1. Workspace & Context Initialization
```bash
mkdir -p ~/lab/f05-software-and-scripting/{packages,repos,scripts,logs,tmp}
cd ~/lab/f05-software-and-scripting
pwd
whoami
hostnamectl --static
find ~/lab/f05-software-and-scripting -maxdepth 2 -type d | sort
```
**Purpose:** Creates a clean, isolated Phase 05 workspace and confirms guest identity.

### 2. Software & Repository Baseline
```bash
rpm -q bash
rpm -q openssh-server
dnf repolist
dnf list installed | head -n 20
```
**Purpose:** Confirms package query visibility and captures the current repository state.

### 3. Evidence Collection
```bash
rpm -q bash > packages/bash.txt
rpm -q openssh-server > packages/openssh-server.txt
dnf repolist > repos/repolist.txt
dnf list installed > packages/installed.txt
find packages repos -maxdepth 1 -type f | sort
cat packages/bash.txt
cat packages/openssh-server.txt
```
**Purpose:** Stores the current software and repository baseline in persistent files.

### 4. Basic Information Script (Creation & Run)
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
**Purpose:** Validates script creation, execution, and executable permissions.

### 5. Package-Check Script & Logging
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
**Purpose:** Validates script output capture into a persistent log file.

### 6. Final Workspace Validation
```bash
find ~/lab/f05-software-and-scripting -maxdepth 3 -type f | sort
ls -l ~/lab/f05-software-and-scripting/scripts
ls -l ~/lab/f05-software-and-scripting/logs
cat ~/lab/f05-software-and-scripting/logs/package-check.log
```
**Purpose:** Confirms final workspace state, stored evidence, and executable script files.

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
Run this block on each secondary guest to ensure consistency:

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
**Purpose:** Confirms that the validated Phase 05 pattern is reusable on the secondary guests without repeating the full reference workflow.

---

## 🧪 Validation Targets

### **srv-admin (Detailed Checklist)**
- [x] Workspace tree confirmed via `find`.
- [x] `packages/bash.txt` contains valid RPM output.
- [x] `packages/openssh-server.txt` contains valid RPM output.
- [x] `repos/repolist.txt` captures repository state.
- [x] `packages/installed.txt` created successfully.
- [x] `scripts/phase05-info.sh` has executable permissions.
- [x] `scripts/package-check.sh` has executable permissions.
- [x] `logs/package-check.log` captured script output successfully.
- [x] Final workspace validation completed.

### **Secondary Guests**
- [x] `packages/bash.txt` created successfully.
- [x] `repos/repolist.txt` captures repository state.
- [x] `scripts/node-info.sh` created and marked executable.
- [x] `logs/node-info.log` created successfully.
- [x] Final workspace listing completed on `srv-web`.
- [x] Final workspace listing completed on `srv-db`.
- [x] Final workspace listing completed on `srv-storage`.

---

## 🧯 Short Troubleshooting
1. **`dnf repolist` shows no repositories available:** This can happen when the guest is not subscribed and no local repository is configured. For this phase, capture the state as evidence and continue with the scripting workflow.
2. **Permission denied when executing scripts:** Confirm `chmod 750` was applied and verify the script path with `ls -l scripts/`.
3. **Shebang problems:** Ensure the first line is exactly `#!/usr/bin/env bash`.
4. **Empty log file:** Run the script directly first, then re-run it with output redirection into `logs/`.

---

## 📸 Expected Screenshots
Store evidence in `assets/screenshots/phase-05/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P05-01** | Package baseline verification | `srv-admin` |
| **P05-02** | Repository state evidence | `srv-admin` |
| **P05-03** | Script execution validation | `srv-admin` |
| **P05-04** | Script log validation | `srv-admin` |
| **P05-05** | Final replicated workspace tree | `srv-web` |
| **P05-06** | Final replicated workspace tree | `srv-db` |
| **P05-07** | Final replicated workspace tree | `srv-storage` |

---

## 🏁 Outcome
Successful execution leaves a validated Phase 05 workspace on all guests, providing evidence of software inspection capability, repository state capture, basic Bash scripting, executable permission handling, and persistent script log generation across the lab.
