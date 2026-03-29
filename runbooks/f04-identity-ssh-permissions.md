# 🛠️ Phase 04 Runbook: Identity, SSH, and Permissions

## 🎯 Objective
Execute and validate a practical Phase 04 workflow focused on core identity security, service baselines, and filesystem access control. This runbook ensures that the administrative context and remote access services are correctly configured across the guest set.

This runbook uses `srv-admin` as the **full reference workflow** and applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **Identity Inspection:** Validating local user and group states.
* **Privilege Escalation:** Confirming functional `sudo` capability.
* **Service Baseline:** Reviewing `sshd` status, port bindings, and config.
* **Access Control:** Managing ownership and permissions with `chmod`/`chgrp`.
* **Behavioral Testing:** Validating directory traversal and file read/write modes.

### **Excluded (Later Phases):**
* Advanced SSH hardening (Key-only auth, custom ports).
* Distributed Identity (Centralized authentication).
* SELinux policy customization and Firewalld exposure rules.

---

## 💻 Prerequisites
Before proceeding, confirm the following status on the **Fedora 43 host**:

| Requirement | Status | Verification Command |
| :--- | :--- | :--- |
| **Guest Baseline** | Phase 03 Complete | `ls -d ~/lab/f03-*` (Inside guests) |
| **Guests Up** | All VMs Operational | `virsh list --all` |
| **Access** | Local Console available | Login via `virt-viewer` or terminal |

---

## 📍 Primary Workspace
All work in this phase is performed under the following tree:

```text
~/lab/f04-identity-ssh-permissions/
├── identity/       # User and group evidence
├── permissions/    # Ownership and mode testing ground
├── ssh/            # Service status and daemon config exports
└── tmp/            # Temporary access and link tests
```

---

## 🚀 Step-by-Step Execution: srv-admin (Full Workflow)

### 1. Workspace & Context Initialization
```bash
mkdir -p ~/lab/f04-identity-ssh-permissions/{identity,ssh,permissions,tmp}
cd ~/lab/f04-identity-ssh-permissions
pwd && whoami && hostnamectl --static
```
* **Validation:** `find ~/lab/f04-identity-ssh-permissions -maxdepth 1 -type d`.

### 2. Identity Baseline & Evidence
```bash
# Inspection
id && groups
getent passwd $(whoami)
getent group wheel

# Save Evidence
whoami > identity/whoami.txt
id > identity/id.txt
groups > identity/groups.txt
getent passwd $(whoami) > identity/passwd-user.txt
getent group wheel > identity/group-wheel.txt
```

### 3. Sudo Validation
Confirm privileged execution is active and allowed:
```bash
sudo -l
sudo id
sudo ls -ld /root
```

### 4. SSH Service Baseline
Reviewing the default security posture of the remote access daemon:
```bash
systemctl status sshd --no-pager -l
ss -tulpn | grep ssh || true
sudo grep -Ev '^\s*#|^\s*$' /etc/ssh/sshd_config
```

### 5. Ownership & Permission Management
```bash
# Create test files
mkdir -p permissions/labdir
touch permissions/labdir/file-a permissions/labdir/file-b
echo "phase04 srv-admin file-a" > permissions/labdir/file-a
echo "phase04 srv-admin file-b" > permissions/labdir/file-b

# Group Ownership (using 'wheel' as a safe baseline)
chgrp wheel permissions/labdir/file-a

# Permission Modes
chmod 640 permissions/labdir/file-a
chmod 600 permissions/labdir/file-b
chmod 750 permissions/labdir
```
* **Verification:** `ls -ld permissions/labdir` and `stat permissions/labdir/file-a`.

### 6. Post-Reboot Persistence (Final Check)
```bash
sudo systemctl reboot
# After reconnecting:
ls -l ~/lab/f04-identity-ssh-permissions/permissions/labdir
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
Run this block on each secondary guest to ensure consistency:

```bash
mkdir -p ~/lab/f04-identity-ssh-permissions/{identity,ssh,permissions}
cd ~/lab/f04-identity-ssh-permissions
whoami > identity/whoami.txt && id > identity/id.txt
sudo -l
systemctl status sshd --no-pager -l | head -n 15
mkdir -p permissions/labdir
echo "phase04 $(hostnamectl --static)" > permissions/labdir/node-file
chmod 640 permissions/labdir/node-file
find ~/lab/f04-identity-ssh-permissions -maxdepth 3 -type f | sort
```

---

## 🧪 Validation Checklist

### **srv-admin (Detailed)**
- [ ] Identity evidence files (`whoami`, `id`, `groups`) created.
- [ ] `sudo -l` confirms the user has administrative privileges.
- [ ] SSH listener confirmed on `0.0.0.0:22` or `[::]:22`.
- [ ] File-a belongs to group `wheel`.
- [ ] Numeric modes (640/600/750) validated via `stat`.

### **Secondary Guests**
- [ ] Identity files reflect correct Guest hostname.
- [ ] `node-file` permissions set to `-rw-r-----`.
- [ ] SSH service is active and running.

---

## 🧯 Short Troubleshooting
1.  **`sudo -l` fails:** Verify the user is in the `wheel` group using `id`. If missing, add with `usermod -aG wheel <user>`.
2.  **`chgrp wheel` fails:** Check if the group exists with `getent group wheel`. On RHEL, this group is standard for admins.
3.  **Broken directory traversal:** If you can't enter `labdir`, check that the directory has the `x` (execute) bit for the owner/group.

---

## 📸 Expected Screenshots
Evidence stored in `assets/screenshots/phase-04/`:

| ID | Description | Guest |
| :--- | :--- | :--- |
| **P04-01** | Identity baseline results | `srv-admin` |
| **P04-02** | Sudo validation and privileged `id` | `srv-admin` |
| **P04-03** | SSH service status and config grep | `srv-admin` |
| **P04-04** | Permissions and Ownership stat output | `srv-admin` |
| **P04-05** | Final workspace tree listing | `srv-admin` |
| **P04-06..08** | Replicated validation workspaces | `srv-web/db/storage` |

---

## 🏁 Outcome
Successful completion of this runbook leaves a validated Phase 04 identity, SSH, and permissions workspace on all guests. The environment is now secured for more advanced network and service configurations in later phases.
