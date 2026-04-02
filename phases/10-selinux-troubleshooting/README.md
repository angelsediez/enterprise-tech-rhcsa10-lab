# 🔐 Phase 10 - SELinux and Troubleshooting

## 🎯 Objective
Establish and validate a controlled Phase 10 baseline for **SELinux awareness** and practical **troubleshooting** across the guest set.

This phase focused on documenting SELinux status inspection, context visibility, controlled labeling validation, and basic diagnostic workflows using standard RHEL tools. The goal was to strengthen confidence in diagnosing access issues through proper context management rather than disabling security enforcement.

This phase used a **full reference workflow on `srv-admin`** and then applied a **shorter validation pattern** on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* **SELinux Baseline:** Inspection of enforcement mode and detailed `sestatus`.
* **Context Visibility:** Reviewing file and process labels using the `-Z` flag.
* **Controlled Label Validation:** Demonstration of manual context change and correction using `restorecon`.
* **Basic Troubleshooting:** Review of AVC-related evidence using `ausearch` and `journalctl`.
* **Evidence Persistence:** Structured capture of security contexts and diagnostic outputs.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | SELinux and Troubleshooting |
| **Primary Guest** | `srv-admin` (Full Reference) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f10-selinux-troubleshooting` |
| **Status** | ✅ Complete |

---

## 🛠️ Execution Strategy
The phase follows the established **Master & Replicate** model:

1. **Full Reference Workflow:** Execute the complete SELinux inspection and troubleshooting workflow on `srv-admin`.
2. **Validation Pattern:** Apply a shorter, inspection-focused validation block on the secondary guests.
3. **Evidence:** Capture screenshots of SELinux state, contexts, relabeling results, and troubleshooting outputs.
4. **Closure:** Confirm that the validated Phase 10 baseline exists across all four guests.

---

## 📐 Workspace Design
Phase 10 operations were isolated within a dedicated directory structure:

```text
~/lab/f10-selinux-troubleshooting/
├── selinux/          # State and context evidence
├── troubleshooting/  # AVC and journal evidence
├── validation/       # Relabel verification evidence
└── tmp/              # Temporary testing area
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-admin` | Reference Node | ✅ Complete |
| `srv-web` | Web Service | ✅ Complete |
| `srv-db` | Database Node | ✅ Complete |
| `srv-storage` | Storage Node | ✅ Complete |

### **Phase 10 Completed Milestones**
* [x] Phase 10 workspace created on `srv-admin`.
* [x] SELinux mode and status validated and exported.
* [x] Context visibility evidence captured on `srv-admin`.
* [x] Controlled `restorecon` workflow completed and verified.
* [x] AVC and journal diagnostic checks captured on `srv-admin`.
* [x] Validation pattern replicated on `srv-web`, `srv-db`, and `srv-storage`.
* [x] Screenshot evidence captured for all planned validation points.

---

## 🧪 Completed srv-admin Validation Areas

### 1. SELinux Baseline
Validated with `getenforce` and `sestatus`. Observed baseline:
* **Mode:** Enforcing
* **Status:** enabled
* **Policy:** targeted

### 2. Context Visibility
Confirmed root directory and system contexts using:
```bash
ls -Zd /
ps -eZ | head -n 20
```

### 3. Controlled Label Validation
Demostración de corrección de contextos con restorecon después de un cambio manual con chcon.

### 4. Basic Troubleshooting
Búsqueda de denegaciones AVC en registros de auditoría y journal.

---

## 📸 Screenshot Inventory
Evidence stored in assets/screenshots/phase-10/:

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

---

## 🏁 Current Outcome
Phase 10 is complete ✅. El laboratorio cuenta ahora con una línea base de **SELinux y Troubleshooting** validada en los cuatro nodos, asegurando que el entorno opera bajo políticas de cumplimiento de Red Hat y que somos capaces de diagnosticar accesos mediante contextos de seguridad de forma profesional.
