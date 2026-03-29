# ✅ Phase 04 Validation Checklist - Identity, SSH, and Permissions

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 04 across the four planned RHEL 10.1 guests. 

It ensures that identity management, privileged access (`sudo`), SSH service baselines, and filesystem permissions have been inspected and documented according to the lab's operational standards.

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

### Workspace and Shell Context
- [x] Phase 04 workspace created under `~/lab/f04-identity-ssh-permissions/`
- [x] Directory structure confirmed (`identity/`, `ssh/`, `permissions/`, `tmp/`)
- [x] Working directory and user context validated

### Identity Baseline
- [x] `whoami`, `id`, and `groups` output reviewed
- [x] `getent` for `root` and `jdoe` validated
- [x] `getent` for administrative groups (`root`, `wheel`) validated
- [x] Identity evidence saved to `.txt` files in `identity/`
- [x] Evidence files content verified with `cat`

### Privileged Access (sudo)
- [x] `sudo -l` returns the expected privilege set
- [x] `sudo id` runs successfully (returning root context)
- [x] `sudo ls -ld /root` confirms privileged access to restricted paths

### SSH Service Baseline
- [x] `sshd` service status is `active (running)`
- [x] Port binding confirmed (typically `:22`) with `ss`
- [x] Baseline configuration grep performed on `sshd_config`
- [x] Directory listing of `/etc/ssh` completed

### Ownership and Permissions Management
- [x] Test directory `permissions/labdir` created
- [x] Test files `file-a` and `file-b` created with sample content
- [x] Group ownership of `file-a` changed to `wheel`
- [x] Numeric mode `640` applied to `file-a`
- [x] Numeric mode `600` applied to `file-b`
- [x] Directory mode `750` applied to `labdir`
- [x] Final modes validated with `stat` or `ls -l`

### Final Workspace Integrity
- [x] Final recursive listing of Phase 04 workspace completed
- [x] All evidence files are readable and formatted correctly

---

## 🔁 Short Validation - `srv-web`
- [x] Phase 04 workspace and subdirectories created
- [x] Identity evidence files (`whoami`, `id`, `groups`) saved
- [x] `sudo -l` capability confirmed
- [x] `sshd` status head (20 lines) reviewed
- [x] `node-file` created and `640` permissions applied
- [x] Final workspace listing completed
- [x] Screenshot captured as `P04-06-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [x] Phase 04 workspace and subdirectories created
- [x] Identity evidence files saved
- [x] `sudo -l` capability confirmed
- [x] `sshd` status reviewed
- [x] `node-file` created and `640` permissions applied
- [x] Final workspace listing completed
- [x] Screenshot captured as `P04-07-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [x] Phase 04 workspace and subdirectories created
- [x] Identity evidence files saved
- [x] `sudo -l` capability confirmed
- [x] `sshd` status reviewed
- [x] `node-file` created and `640` permissions applied
- [x] Final workspace listing completed
- [x] Screenshot captured as `P04-08-final-workspace-srv-storage.png`

---

## 📸 Screenshot Inventory
- [x] `P04-01-identity-baseline-srv-admin.png`
- [x] `P04-02-sudo-validation-srv-admin.png`
- [x] `P04-03-ssh-baseline-srv-admin.png`
- [x] `P04-04-permissions-validation-srv-admin.png`
- [x] `P04-05-final-workspace-srv-admin.png`
- [x] `P04-06-final-workspace-srv-web.png`
- [x] `P04-07-final-workspace-srv-db.png`
- [x] `P04-08-final-workspace-srv-storage.png`

---

## 🏁 Phase 04 Exit Criteria
Phase 04 is considered complete when:

- [x] Administrative identity and groups are validated across the guest set
- [x] Sudo access is confirmed as functional for all nodes
- [x] SSH service baseline is documented for all nodes
- [x] Standard ownership and numeric permission changes are validated
- [x] Workspace evidence is complete and screenshots are captured
- [x] `phases/04-identity-ssh-permissions/README.md` updated with completion status
- [x] `runbooks/f04-identity-ssh-permissions.md` updated with completion status
- [x] `validation/04-identity-ssh-permissions-checklist.md` reviewed and completed
- [x] `README.md` updated to reflect Phase 04 completion
