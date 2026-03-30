# ✅ Phase 05 Validation Checklist - Software and Scripting

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 05 across the four planned RHEL 10.1 guests.

It confirms that software inspection, repository state capture, basic Bash scripting, script execution, and log generation were executed successfully and documented with evidence.

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
- [x] Phase 05 workspace created under `~/lab/f05-software-and-scripting`
- [x] Working directory validated with `pwd`
- [x] Active user validated with `whoami`
- [x] Hostname validated with `hostnamectl --static`

### Software Baseline
- [x] `rpm -q bash` executed successfully
- [x] `rpm -q openssh-server` executed successfully
- [x] `dnf repolist` executed successfully
- [x] `dnf list installed | head -n 20` executed successfully

### Stored Software Evidence
- [x] `packages/bash.txt` created successfully
- [x] `packages/openssh-server.txt` created successfully
- [x] `packages/installed.txt` created successfully
- [x] `repos/repolist.txt` created successfully
- [x] package and repository evidence files listed successfully

### Script Creation and Execution
- [x] `scripts/phase05-info.sh` created successfully
- [x] `scripts/phase05-info.sh` marked executable with `chmod 750`
- [x] `scripts/phase05-info.sh` executed successfully

### Script Logging
- [x] `scripts/package-check.sh` created successfully
- [x] `scripts/package-check.sh` marked executable with `chmod 750`
- [x] `logs/package-check.log` created successfully
- [x] package-check script output redirected into the log successfully
- [x] `cat logs/package-check.log` validated successfully

### Final Workspace Validation
- [x] final workspace file listing completed
- [x] script permissions reviewed successfully
- [x] log directory contents reviewed successfully
- [x] final package-check log output reviewed successfully

---

## 🔁 Short Validation - `srv-web`
- [x] workspace created
- [x] `packages/bash.txt` created
- [x] `repos/repolist.txt` created
- [x] `scripts/node-info.sh` created
- [x] `scripts/node-info.sh` marked executable
- [x] `logs/node-info.log` created
- [x] `cat logs/node-info.log` executed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P05-05-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [x] workspace created
- [x] `packages/bash.txt` created
- [x] `repos/repolist.txt` created
- [x] `scripts/node-info.sh` created
- [x] `scripts/node-info.sh` marked executable
- [x] `logs/node-info.log` created
- [x] `cat logs/node-info.log` executed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P05-06-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [x] workspace created
- [x] `packages/bash.txt` created
- [x] `repos/repolist.txt` created
- [x] `scripts/node-info.sh` created
- [x] `scripts/node-info.sh` marked executable
- [x] `logs/node-info.log` created
- [x] `cat logs/node-info.log` executed successfully
- [x] final workspace listing completed
- [x] screenshot captured as `P05-07-final-workspace-srv-storage.png`

---

## 📸 Screenshot Inventory
- [x] `P05-01-package-baseline-srv-admin.png`
- [x] `P05-02-repositories-srv-admin.png`
- [x] `P05-03-script-execution-srv-admin.png`
- [x] `P05-04-script-log-validation-srv-admin.png`
- [x] `P05-05-final-workspace-srv-web.png`
- [x] `P05-06-final-workspace-srv-db.png`
- [x] `P05-07-final-workspace-srv-storage.png`

---

## 🏁 Phase 05 Exit Criteria
Phase 05 is considered complete when:

- [x] `srv-admin` full workflow completed and validated
- [x] `srv-web` short validation completed
- [x] `srv-db` short validation completed
- [x] `srv-storage` short validation completed
- [x] all expected screenshots captured
- [x] `phases/05-software-and-scripting/README.md` updated
- [x] `runbooks/f05-software-and-scripting.md` updated
- [x] this checklist reviewed and completed

---

## 📝 Notes
- [x] Repository state was captured even if `dnf repolist` returned `No repositories available`
- [x] Phase 05 was documented as a software inspection and scripting baseline, not as a full package installation/removal phase
