# 🔐 Phase 04 - Identity, SSH, and Permissions

## 🎯 Objective
Establish and validate a controlled Phase 04 baseline for local identity management, service security, and filesystem access control. This phase ensures that the lab environment has a secure and predictable foundation for user operations and remote access.

This phase begins with a **full workflow on `srv-admin`** and then reuses a **shorter validation pattern** on the remaining guests.

---

## 🔍 Scope

### **Included in this phase:**
* **Identity Management:** Validating local users, group memberships, and UID/GID consistency.
* **Privileged Access:** Verifying `sudo` functionality and policy for the administrative user.
* **SSH Baseline:** Inspecting the `sshd` service state, listening ports, and basic configuration hardening.
* **Permissions & Ownership:** Validating `chmod`, `chown`, and `chgrp` behavior on controlled workspace files.
* **Link Security:** Reviewing permission behavior on symbolic and hard links created in Phase 03.
* **Evidence:** Capturing multi-point validation results via screenshots.

### **Excluded (Later Phases):**
* Advanced SSH hardening (Key-only auth, custom ports).
* SSH Key-Pair distribution between nodes (Automation prep).
* Centralized Identity (IPA/Active Directory integration).
* ACL-focused workflows (Advanced Access Control Lists).

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Identity, SSH, and Permissions |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f04-identity-ssh-permissions` |
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows the "Master & Replicate" model established in previous milestones:

1.  **Full Reference Workflow:** Execute the complete suite of identity and permission tests on `srv-admin`.
2.  **Service Hardening Review:** Perform a baseline inspection of the SSH daemon and its default security posture.
3.  **Validation Pattern:** Apply a condensed version of these checks on `srv-web`, `srv-db`, and `srv-storage` to ensure environment parity.
4.  **Evidence:** Capture screenshots of command results and directory structures.
5.  **Closure:** Confirm that the validated Phase 04 pattern now exists across all four guests.

---

## 📐 Workspace Design
Phase 04 operations are isolated within a dedicated directory structure to prevent interference with system-critical files:

```text
~/lab/f04-identity-ssh-permissions/
├── identity/       # Logs of user/group lookups
├── ssh/            # Service status and config exports
├── permissions/    # Controlled chmod/chown test files
└── tmp/            # Temporary link and access tests
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

### **Hitos de la Fase 04**
* [ ] Phase 04 workspace created across all guests.
* [ ] Identity baseline (ID/Groups) validated on `srv-admin`.
* [ ] `sudo` access confirmed for the administrative account.
* [ ] SSH service status and port binding (`22/tcp`) verified.
* [ ] Ownership and Permission workflows (Standard/Numeric) documented.
* [ ] Validation pattern replicated on secondary guests.

---

## 🧪 Planned srv-admin Validation Areas

### 1. Identity Baseline
Checking the current user context and group environment:
```bash
whoami && id
getent passwd $(whoami)
getent group | grep $(whoami)
```

### 2. Sudo Validation
Confirming the ability to perform privileged operations:
```bash
sudo -l
sudo hostnamectl # Sample privileged command
```

### 3. SSH Service Baseline
Reviewing the current state of remote access security:
```bash
systemctl status sshd
ss -tulpn | grep :22
sudo sshd -T | grep -E 'permitrootlogin|passwordauthentication'
```

### 4. Ownership and Permissions
Testing filesystem security controls:
```bash
ls -l permissions/
chmod 640 permissions/test-file
chown :users permissions/test-file
ls -li permissions/test-file
```

---

## 📸 Screenshot Inventory
All evidence will be stored in `assets/screenshots/phase-04/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P04-01** | Identity baseline (id/getent) | `srv-admin` |
| **P04-02** | Sudo access validation | `srv-admin` |
| **P04-03** | SSH service status and port binding | `srv-admin` |
| **P04-04** | Permissions and Ownership validation | `srv-admin` |
| **P04-05** | Final workspace validation | `srv-admin` |
| **P04-06..08**| Replicated validation pattern | `srv-web/db/storage` |

---

## 🔗 Related Files
* `runbooks/f04-identity-ssh-permissions.md` — Detailed step-by-step guide.
* `phases/03-shell-files-docs/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware and status tracker.

---

## 🏁 Current Outcome
Phase 04 is **Not Started** ⚪.

The lab infrastructure is currently awaiting the execution of identity hardening and permission validation. The baseline established in Phase 03 is ready to be secured.
