# ✅ Phase 04 Validation Checklist - Identity, SSH, and Permissions

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 04 across the four planned RHEL 10.1 guests.

It confirms that identity inspection, `sudo` validation, SSH baseline review, and permission testing were executed successfully and documented with evidence.

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
- [x] Phase 04 workspace created under `~/lab/f04-identity-ssh-permissions`
- [x] Working directory validated with `pwd`
- [x] Active user validated with `whoami`
- [x] Hostname validated with `hostnamectl --static`

### Identity Baseline
- [x] `whoami` executed successfully
- [x] `id` executed successfully
- [x] `groups` executed successfully
- [x] `getent passwd root` executed successfully
- [x] `getent passwd jdoe` executed successfully
- [x] `getent group root` executed successfully
- [x] `getent group wheel` executed successfully

### Identity Evidence Files
- [x] `identity/whoami.txt` created successfully
- [x] `identity/id.txt` created successfully
- [x] `identity/groups.txt` created successfully
- [x] `identity/passwd-jdoe.txt` created successfully
- [x] `identity/group-wheel.txt` created successfully

### `sudo` Validation
- [x] `sudo -l` executed successfully
- [x] `sudo id` executed successfully
- [x] `sudo ls -ld /root` executed successfully

### SSH Baseline
- [x] `systemctl status sshd --no-pager -l` executed successfully
- [x] `ss -tulpn | grep ssh` executed successfully
- [x] effective `sshd_config` lines reviewed
- [x] `/etc/ssh` contents inspected

### Permissions Test Workspace
- [x] `permissions/labdir` created successfully
- [x] `permissions/labdir/file-a` created successfully
- [x] `permissions/labdir/file-b` created successfully
- [x] test content written to `file-a`
- [x] test content written to `file-b`

### Ownership and Group Ownership
- [x] group ownership changed on `file-a`
- [x] `file-a` validated with group `wheel`

### Permission Mode Validation
- [x] `file-a` set to mode `640`
- [x] `file-b` set to mode `600`
- [x] `permissions/labdir` set to mode `750`
- [x] file mode validation confirmed with `ls -l`
- [x] file mode validation confirmed with `stat`
- [x] directory mode validation confirmed with `ls -ld`
- [x] directory mode validation confirmed with `stat`

### Final Workspace Validation
- [x] final workspace listing completed
- [x] final permissions listing completed
- [x] stored identity evidence reviewed

---

## 🔁 Short Validation - `srv-web`
- [x] workspace created
- [x] `identity/whoami.txt` created
- [x] `identity/id.txt` created
- [x] `identity/groups.txt` created
- [x] `sudo -l` executed successfully
- [x] `sshd` status reviewed
- [x] `permissions/labdir/node-file` created
- [x] `node-file` populated with host-specific content
- [x] `node-file` set to mode `640`
- [x] final workspace listing completed
- [x] screenshot captured as `P04-06-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [x] workspace created
- [x] `identity/whoami.txt` created
- [x] `identity/id.txt` created
- [x] `identity/groups.txt` created
- [x] `sudo -l` executed successfully
- [x] `sshd` status reviewed
- [x] `permissions/labdir/node-file` created
- [x] `node-file` populated with host-specific content
- [x] `node-file` set to mode `640`
- [x] final workspace listing completed
- [x] screenshot captured as `P04-07-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [x] workspace created
- [x] `identity/whoami.txt` created
- [x] `identity/id.txt` created
- [x] `identity/groups.txt` created
- [x] `sudo -l` executed successfully
- [x] `sshd` status reviewed
- [x] `permissions/labdir/node-file` created
- [x] `node-file` populated with host-specific content
- [x] `node-file` set to mode `640`
- [x] final workspace listing completed
- [x] screenshot captured as `P04-08-final-workspace-srv-storage.png`

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

- [x] `srv-admin` full workflow completed and validated
- [x] `srv-web` short validation completed
- [x] `srv-db` short validation completed
- [x] `srv-storage` short validation completed
- [x] all expected screenshots captured
- [ ] `phases/04-identity-ssh-permissions/README.md` updated
- [ ] `runbooks/f04-identity-ssh-permissions.md` updated
- [x] this checklist reviewed and completed
