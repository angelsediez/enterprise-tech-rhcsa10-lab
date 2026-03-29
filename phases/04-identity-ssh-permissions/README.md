# 🔐 Phase 04 - Identity, SSH, and Permissions

## 🎯 Objective
Establish and validate a controlled Phase 04 baseline for local identity management, SSH service inspection, and filesystem access control across the guest set.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Identity Management:** Validating local users, group memberships, and identity evidence files.
* **Privileged Access:** Verifying functional `sudo` capability for the administrative user.
* **SSH Baseline:** Inspecting `sshd` service state, listener visibility, and active configuration lines.
* **Permissions & Ownership:** Validating `chmod` and `chgrp` behavior on controlled workspace files.
* **Evidence:** Capturing multi-point validation results via screenshots and stored command output.

### **Excluded (Later Phases):**
* Advanced SSH hardening (Key-only auth, custom ports).
* SSH key distribution between nodes.
* Centralized identity integration.
* ACL-focused workflows beyond basic permissions.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Identity, SSH, and Permissions |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation Pattern) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f04-identity-ssh-permissions` |
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase follows the "Master & Replicate" model established in previous milestones:

1. **Full Reference Workflow:** Execute the complete identity, `sudo`, SSH, and permissions workflow on `srv-admin`.
2. **Validation Pattern:** Apply a condensed version of the same checks on `srv-web`, `srv-db`, and `srv-storage`.
3. **Evidence:** Capture screenshots of command results and final workspace state.
4. **Closure:** Confirm that the validated Phase 04 pattern now exists across all four guests.

---

## 📐 Workspace Design
Phase 04 operations are isolated within a dedicated directory structure:

```text
~/lab/f04-identity-ssh-permissions/
├── identity/       # Stored identity evidence
├── ssh/            # SSH inspection workspace
├── permissions/    # Controlled chmod/chgrp test files
└── tmp/            # Temporary tests
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

### **Phase 04 Achievements**
* [x] Phase 04 workspace created across all guests.
* [x] Identity baseline validated on `srv-admin`.
* [x] Identity evidence files created on `srv-admin`.
* [x] `sudo` access confirmed for the administrative account.
* [x] SSH service status and baseline configuration verified.
* [x] Ownership and permission workflows documented on `srv-admin`.
* [x] Validation pattern replicated on `srv-web`, `srv-db`, and `srv-storage`.
* [x] Screenshot evidence captured for all planned validation points.

---

## 🧪 srv-admin Validation Areas

### 1. Identity Baseline
```bash
whoami && id && groups
getent passwd root
getent passwd jdoe
getent group root
getent group wheel
```

### 2. Identity Evidence
```bash
whoami > identity/whoami.txt
id > identity/id.txt
groups > identity/groups.txt
getent passwd jdoe > identity/passwd-jdoe.txt
getent group wheel > identity/group-wheel.txt
find identity -maxdepth 1 -type f | sort
```

### 3. sudo Validation
```bash
sudo -l
sudo id
sudo ls -ld /root
```

### 4. SSH Service Baseline
```bash
systemctl status sshd --no-pager -l
ss -tulpn | grep ssh || true
sudo grep -Ev '^\s*#|^\s*$' /etc/ssh/sshd_config
ls -l /etc/ssh
```

### 5. Ownership and Permissions
```bash
mkdir -p permissions/labdir
touch permissions/labdir/file-a permissions/labdir/file-b
echo "phase04 srv-admin file-a" > permissions/labdir/file-a
echo "phase04 srv-admin file-b" > permissions/labdir/file-b
chgrp wheel permissions/labdir/file-a
chmod 640 permissions/labdir/file-a
chmod 600 permissions/labdir/file-b
chmod 750 permissions/labdir
ls -ld permissions/labdir
ls -l permissions/labdir
stat permissions/labdir/file-a
stat permissions/labdir/file-b
```

### 6. Final Workspace Validation
```bash
find ~/lab/f04-identity-ssh-permissions -maxdepth 3 -type f | sort
ls -ld ~/lab/f04-identity-ssh-permissions/permissions/labdir
ls -l ~/lab/f04-identity-ssh-permissions/permissions/labdir
cat ~/lab/f04-identity-ssh-permissions/identity/id.txt
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
The following condensed validation block was executed on each secondary guest:

```bash
mkdir -p ~/lab/f04-identity-ssh-permissions/{identity,ssh,permissions}
cd ~/lab/f04-identity-ssh-permissions
whoami > identity/whoami.txt
id > identity/id.txt
groups > identity/groups.txt
sudo -l
systemctl status sshd --no-pager -l | head -n 20
mkdir -p permissions/labdir
touch permissions/labdir/node-file
echo "phase04 $(hostnamectl --static)" > permissions/labdir/node-file
chmod 640 permissions/labdir/node-file
ls -l permissions/labdir/node-file
find ~/lab/f04-identity-ssh-permissions -maxdepth 3 -type f | sort
```

---

## 📸 Screenshot Inventory
All evidence is stored in `assets/screenshots/phase-04/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P04-01** | Identity baseline validation | `srv-admin` |
| **P04-02** | `sudo` access validation | `srv-admin` |
| **P04-03** | SSH service baseline | `srv-admin` |
| **P04-04** | Permissions and ownership validation | `srv-admin` |
| **P04-05** | Final workspace validation | `srv-admin` |
| **P04-06** | Final replicated workspace | `srv-web` |
| **P04-07** | Final replicated workspace | `srv-db` |
| **P04-08** | Final replicated workspace | `srv-storage` |

---

## 🔗 Related Files
* `runbooks/f04-identity-ssh-permissions.md` — Detailed step-by-step guide.
* `validation/04-identity-ssh-permissions-checklist.md` — Phase 04 validation checklist.
* `phases/03-shell-files-docs/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 04 is **complete** ✅.

A full identity, sudo, SSH baseline, and permissions workflow was executed and validated on `srv-admin`. A shorter validation pattern was then successfully reused on `srv-web`, `srv-db`, and `srv-storage`.

The lab now has a documented and validated Phase 04 identity, SSH, and permissions baseline across all four guests, providing a clean foundation for the next software and scripting phase.
