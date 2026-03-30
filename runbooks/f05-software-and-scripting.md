# 🛠️ Phase 05 Runbook: Software and Scripting

## 🎯 Objective
Execute and validate a practical Phase 05 workflow focused on software management and basic automation. This runbook establishes a baseline for package inspection, repository visibility, and the creation of reproducible Bash scripts across the guest set.

This runbook uses `srv-admin` as the **full reference workflow** and applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Software Inspection:** Querying installed packages and repository status.
* **Evidence Capture:** Redirecting package metadata into persistent text files.
* **Script Development:** Creating functional Bash scripts with proper shebangs.
* **Execution Control:** Validating numeric permissions (`750`) for script execution.
* **Log Management:** Capturing real-time script output into log files.
* **Workspace Validation:** Final tree-state confirmation across all guests.

### **Excluded (Later Phases):**
* Advanced package lifecycle (installation/removal of complex stacks).
* Cross-guest automation (Ansible/SSH loops).
* Complex scripting logic (functions, arrays, or advanced regex).

---

## 💻 Prerequisites
Before proceeding, confirm the following baseline conditions:

| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Phase Baseline** | Phase 04 complete | Review `phases/04-identity-ssh-permissions/README.md` |
| **Guest Availability**| All four guests operational | `virsh list --all` |
| **Access Control** | Working user has sudo/local access| Console or SSH login confirmed |
| **Network** | `lab-int` connectivity | `ping -c 2 8.8.8.8` (or internal gateway) |

---

## 📍 Primary Workspace
All operations are centralized within the following structure:

```text
~/lab/f05-software-and-scripting/
├── packages/    # Stored package query results
├── repos/       # Repository metadata exports
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
pwd && whoami && hostnamectl --static
```
* **Purpose:** Ensures a clean, isolated environment for software evidence.

### 2. Software & Repository Baseline
```bash
rpm -q bash openssh-server
dnf repolist
dnf list installed | head -n 20
```
* **Purpose:** Confirms visibility of the package manager and core utilities.

### 3. Evidence Collection
```bash
rpm -q bash > packages/bash.txt
rpm -q openssh-server > packages/openssh-server.txt
dnf repolist > repos/repolist.txt
dnf list installed > packages/installed.txt
find packages repos -maxdepth 1 -type f | sort
```
* **Purpose:** Permanently stores the current software state for audit.

### 4. Basic Information Script (Creation & Run)
```bash
cat > scripts/phase05-info.sh <<'EOF'
#!/usr/bin/env bash
echo "--- Guest Info Report ---"
echo "host=$(hostnamectl --static)"
echo "user=$(whoami)"
echo "pwd=$(pwd)"
echo "date=$(date)"
EOF

chmod 750 scripts/phase05-info.sh
./scripts/phase05-info.sh
```
* **Purpose:** Validates script permissions and environment context variable expansion.

### 5. Package-Check Script & Logging
```bash
cat > scripts/package-check.sh <<'EOF'
#!/usr/bin/env bash
echo "Checking core packages..."
rpm -q bash
rpm -q openssh-server
EOF

chmod 750 scripts/package-check.sh
./scripts/package-check.sh > logs/package-check.log
cat logs/package-check.log
```
* **Purpose:** Validates redirection of script-generated output into log files.

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
find ~/lab/f05-software-and-scripting -maxdepth 3 -type f | sort
```

---

## 🧪 Validation Targets

### **srv-admin (Detailed Checklist)**
- [ ] Workspace tree confirmed via `find`.
- [ ] `packages/bash.txt` contains valid RPM metadata.
- [ ] `repos/repolist.txt` shows active DNF repositories.
- [ ] `scripts/phase05-info.sh` has `rwxr-x---` permissions.
- [ ] `logs/package-check.log` captured script output successfully.

### **Secondary Guests**
- [ ] `node-info.log` correctly identifies the specific guest hostname.
- [ ] Script executable bit is present.
- [ ] Software evidence files exist in `packages/` and `repos/`.

---

## 🧯 Short Troubleshooting
1.  **DNF Repolist Fails:** Check connectivity or ensure `/etc/yum.repos.d/` contains valid `.repo` files.
2.  **Permission Denied:** Ensure you used `chmod 750` (or `u+x`) and that the partition allows execution (no `noexec` mount option).
3.  **Shebang Errors:** Ensure the first line is exactly `#!/usr/bin/env bash` or `#!/bin/bash` without trailing spaces.

---

## 📸 Expected Screenshots
Store evidence in `assets/screenshots/phase-05/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P05-01** | Package baseline verification | `srv-admin` |
| **P05-02** | Repository list evidence | `srv-admin` |
| **P05-03** | Script execution (stdout) | `srv-admin` |
| **P05-04** | Persistent log file verification | `srv-admin` |
| **P05-05..07** | Final replicated workspace tree | `srv-web/db/storage` |

---

## 🏁 Outcome
Successful execution leaves a validated Phase 05 workspace on all guests, providing evidence of software inspection capability and a functional scripting baseline.
