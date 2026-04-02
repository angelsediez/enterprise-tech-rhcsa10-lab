# 🔐 Phase 10 - SELinux and Troubleshooting

## 🎯 Objective
Establish and validate a controlled Phase 10 baseline for **SELinux awareness** and practical **troubleshooting** across the guest set.

This phase focuses on documenting SELinux status inspection, context visibility, controlled labeling validation, and basic diagnostic workflows using standard RHEL tools. The goal is to strengthen confidence in diagnosing access issues through proper context management rather than disabling security enforcement.

This phase begins with a **full reference workflow on `srv-admin`** and follows with a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **SELinux Baseline:** Inspection of enforcement modes (`Enforcing`/`Permissive`) and detailed `sestatus`.
* **Context Visibility:** Reviewing file and process labels using the `-Z` flag (`ls`, `ps`).
* **Controlled Label Validation:** Demonstration of context inheritance and correction using `restorecon`.
* **Basic Troubleshooting:** Identification of Access Vector Cache (AVC) denials using `ausearch` and `journalctl`.
* **Evidence Persistence:** Structured capture of security contexts and diagnostic logs.

### **Excluded (Later Phases):**
* Advanced custom policy development from scratch.
* Complex module authoring with `audit2allow`.
* System-wide relabeling of non-standard service ports.
* Deep Boolean tuning for complex multi-tier applications.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | SELinux and Troubleshooting |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f10-selinux-troubleshooting` |
| **Status** | ⚪ Not Started |

---

## 🛠️ Execution Strategy
The phase follows the established **Master & Replicate** model:

1.  **Full Reference Workflow:** Execute the complete SELinux inspection and troubleshooting workflow on `srv-admin`.
2.  **Validation Pattern:** Apply a shorter, inspection-focused validation block on the secondary guests.
3.  **Evidence:** Capture screenshots of security states, contexts, and relabeling results.
4.  **Closure:** Confirm that the validated Phase 10 baseline exists across all four guests.

---

## 📐 Workspace Design
Phase 10 operations are isolated within a dedicated directory structure:

```text
~/lab/f10-selinux-troubleshooting/
├── selinux/          # State, booleans, and context evidence
├── troubleshooting/   # AVC denials and diagnostic logs
├── validation/       # Relabel and fix validation results
└── tmp/              # Temporary testing area (demo labels)
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

### **Phase 10 Planned Milestones**
* [ ] Phase 10 workspace created on `srv-admin`.
* [ ] SELinux mode and status validated and exported.
* [ ] Context visibility evidence (File/Process) captured.
* [ ] Controlled `restorecon` workflow completed and verified.
* [ ] Audit log diagnostic checks (AVC) captured on `srv-admin`.
* [ ] Validation pattern replicated on secondary guests.
* [ ] Screenshot evidence captured for all planned points.

---

## 🧪 Planned srv-admin Validation Areas

### 1. SELinux Baseline
```bash
getenforce
sestatus
```

### 2. Context Visibility
```bash
ls -Z /
ps -eZ | head -n 15
```

### 3. Controlled Label Validation
```bash
mkdir -p ~/lab/f10-selinux-troubleshooting/tmp/demo
touch ~/lab/f10-selinux-troubleshooting/tmp/demo/testfile
ls -Z ~/lab/f10-selinux-troubleshooting/tmp/demo/testfile
# Test relabeling
sudo restorecon -Rv ~/lab/f10-selinux-troubleshooting/tmp/demo
```

### 4. Basic Troubleshooting
```bash
sudo ausearch -m AVC -ts recent || true
sudo journalctl -t setroubleshoot --no-pager | tail -n 20 || true
```

---

## 📸 Planned Screenshot Inventory
Evidence stored in `assets/screenshots/phase-10/`:

| ID | Description | Target |
| :--- | :--- | :--- |
| **P10-01** | SELinux baseline (Enforce/Status) | `srv-admin` |
| **P10-02** | Context visibility (Files/Processes) | `srv-admin` |
| **P10-03** | Controlled relabel validation | `srv-admin` |
| **P10-04** | Troubleshooting diagnostic evidence | `srv-admin` |
| **P10-05..07**| Final replicated SELinux workspaces | `srv-web/db/storage` |

---

## 🔗 Related Files
* `runbooks/f10-selinux-troubleshooting.md` — Detailed step-by-step guide.
* `phases/09-nfs-autofs/README.md` — Previous phase report.
* `notes/guest-inventory.md` — Lab VM hardware tracker.

---

## 🏁 Current Outcome
Phase 10 is **Not Started** ⚪.
