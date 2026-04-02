# 🛠️ Phase 10 Runbook: SELinux and Troubleshooting

## 🎯 Objective
Execute and validate a practical Phase 10 workflow focused on SELinux awareness and basic troubleshooting across the guest set.

This runbook documents:
- SELinux enforcement and status inspection
- file and process context visibility
- controlled label change and correction with `restorecon`
- basic troubleshooting using `ausearch` and `journalctl`
- evidence capture across the lab guests

This runbook uses **`srv-admin` as the full reference workflow** and then applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included**
- SELinux baseline inspection with `getenforce` and `sestatus`
- file and process context review with `ls -Z` and `ps -eZ`
- controlled relabel validation using `chcon` and `restorecon`
- basic troubleshooting using `ausearch` and `journalctl`
- evidence capture and final workspace validation

### **Excluded**
- custom SELinux policy authoring
- module generation with `audit2allow`
- complex SELinux Boolean tuning
- permanent production policy redesign

---

## 💻 Prerequisites

Before using this runbook, confirm:

| Requirement | Expected State | Verification |
| :--- | :--- | :--- |
| **Lab Nodes** | All four guests operational | `virsh list --all` |
| **Phase Baseline** | Phase 09 complete | Review Phase 09 report |
| **Access Control** | Working user has sudo access | `sudo -v` |
| **SELinux** | SELinux available on guest | `getenforce` |

> [!IMPORTANT]
> This phase validates SELinux inspection and troubleshooting fundamentals. Do **not** disable SELinux as part of the normal workflow.

---

## 📍 Primary Workspace

All work in this phase is organized under:
`~/lab/f10-selinux-troubleshooting`

**Workspace Layout**
```text
~/lab/f10-selinux-troubleshooting/
├── selinux/          # SELinux state and context evidence
├── troubleshooting/  # AVC and journal evidence
├── validation/       # Controlled relabel validation evidence
└── tmp/              # Temporary demo files
```

---

## 🚀 Full Workflow on srv-admin
### 1) Create Workspace and Validate Context
```bash
mkdir -p ~/lab/f10-selinux-troubleshooting/{selinux,troubleshooting,validation,tmp}
cd ~/lab/f10-selinux-troubleshooting
pwd && whoami && hostnamectl --static
find ~/lab/f10-selinux-troubleshooting -maxdepth 2 -type d | sort
```

### 2) SELinux Baseline
```bash
getenforce
sestatus
```

### 3) Context Visibility
```bash
ls -Zd /
ls -Zd /var /home /tmp
ps -eZ | head -n 20
```

### 4) Save SELinux Baseline Evidence
```bash
getenforce > selinux/getenforce.txt
sestatus > selinux/sestatus.txt
ls -Zd / > selinux/root-context.txt
ls -Zd /var /home /tmp > selinux/common-contexts.txt
ps -eZ | head -n 20 > selinux/process-contexts.txt
find selinux -maxdepth 1 -type f | sort
```

### 5) Controlled Relabel Validation
```bash
mkdir -p ~/lab/f10-selinux-troubleshooting/tmp/demo
touch ~/lab/f10-selinux-troubleshooting/tmp/demo/testfile
ls -lZ ~/lab/f10-selinux-troubleshooting/tmp/demo

sudo chcon -t httpd_sys_content_t ~/lab/f10-selinux-troubleshooting/tmp/demo/testfile
ls -lZ ~/lab/f10-selinux-troubleshooting/tmp/demo

sudo restorecon -Rv ~/lab/f10-selinux-troubleshooting/tmp/demo
ls -lZ ~/lab/f10-selinux-troubleshooting/tmp/demo
```

### 6) Save Relabel Evidence
```bash
ls -lZ ~/lab/f10-selinux-troubleshooting/tmp/demo > validation/demo-contexts-final.txt
sudo restorecon -Rv ~/lab/f10-selinux-troubleshooting/tmp/demo > validation/restorecon-output.txt
```

### 7) Basic Troubleshooting Review
```bash
sudo ausearch -m AVC || true
sudo journalctl --no-pager | grep -i selinux | tail -n 20 || true
```

---

## 🔁 Short Validation Pattern (Secondary Guests)
```bash
mkdir -p ~/lab/f10-selinux-troubleshooting/{selinux,troubleshooting,validation}
cd ~/lab/f10-selinux-troubleshooting
getenforce > selinux/getenforce.txt
sestatus > selinux/sestatus.txt
ls -Zd / /var /home /tmp > selinux/common-contexts.txt
ps -eZ | head -n 20 > selinux/process-contexts.txt
sudo ausearch -m AVC > troubleshooting/ausearch-avc.txt 2>/dev/null || true
find ~/lab/f10-selinux-troubleshooting -maxdepth 3 -type f | sort
```

---

## 🧪 Validation Targets
### **srv-admin**
- [x] Workspace created successfully
- [x] SELinux baseline (Enforcing) validated
- [x] Context visibility (File/Process) validated
- [x] Controlled relabel (chcon/restorecon) validated
- [x] Troubleshooting evidence captured

### **srv-web, srv-db, srv-storage**
- [x] Short validation pattern completed
- [x] SELinux state and contexts captured
- [x] Workspace validated

---

## 📸 Expected Screenshots
Store in: `assets/screenshots/phase-10/`

| ID | Description | Target |
| :--- | :--- | :--- |
| **P10-01** | SELinux baseline (Enforce/Status) | `srv-admin` |
| **P10-02** | Context visibility (Files/Processes) | `srv-admin` |
| **P10-03** | Controlled relabel validation | `srv-admin` |
| **P10-04** | Troubleshooting diagnostic evidence | `srv-admin` |
| **P10-05..07**| Final replicated SELinux workspaces | `srv-web/db/storage` |

---

## 🏁 Outcome
Successful execution confirms a validated SELinux and troubleshooting baseline across the guest set. The environment is now prepared for the final lab closure.
