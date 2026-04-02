# ✅ Phase 10 Validation Checklist: SELinux and Troubleshooting

## 🎯 Validation Goal
Confirm that Phase 10 established a working SELinux inspection and troubleshooting baseline on `srv-admin`, and that the shorter SELinux validation pattern was successfully replicated on `srv-web`, `srv-db`, and `srv-storage`.

---

## 🖥️ Reference Validation — `srv-admin`

### SELinux Baseline
- [x] `getenforce` executed successfully
- [x] `getenforce` returned `Enforcing`
- [x] `sestatus` executed successfully
- [x] `sestatus` confirms SELinux is `enabled`
- [x] `sestatus` confirms current mode is `enforcing`
- [x] `sestatus` confirms loaded policy is `targeted`

### Context Visibility
- [x] Root context reviewed with `ls -Zd /`
- [x] Common directory contexts reviewed with `ls -Zd /var /home /tmp`
- [x] Process contexts reviewed with `ps -eZ | head -n 20`

### SELinux Evidence Export
- [x] `selinux/getenforce.txt` created
- [x] `selinux/sestatus.txt` created
- [x] `selinux/root-context.txt` created
- [x] `selinux/common-contexts.txt` created
- [x] `selinux/process-contexts.txt` created

### Controlled Relabel Validation
- [x] Demo directory created under `tmp/demo`
- [x] Demo file `testfile` created
- [x] Initial file context reviewed
- [x] Manual context change applied with `chcon`
- [x] Temporary context changed to `httpd_sys_content_t`
- [x] Default context restored successfully with `restorecon`
- [x] Final corrected context reviewed successfully

### Relabel Evidence Export
- [x] `validation/demo-contexts-final.txt` created
- [x] `validation/restorecon-output.txt` created

### Troubleshooting Validation
- [x] `ausearch -m AVC` executed successfully
- [x] AVC search result documented even when no matches were present
- [x] `journalctl -t setroubleshoot --no-pager | tail -n 20` executed successfully
- [x] SELinux-related journal review executed successfully
- [x] Basic troubleshooting evidence captured without disabling SELinux

### Troubleshooting Evidence Export
- [x] `troubleshooting/ausearch-avc.txt` created
- [x] `troubleshooting/setroubleshoot-tail.txt` created
- [x] `troubleshooting/journal-selinux-tail.txt` created

---

## 🔁 Replicated Validation — `srv-web`
- [x] Phase 10 workspace created
- [x] `selinux/getenforce.txt` created
- [x] `selinux/sestatus.txt` created
- [x] `selinux/common-contexts.txt` created
- [x] `selinux/process-contexts.txt` created
- [x] `troubleshooting/ausearch-avc.txt` created
- [x] Final file listing validated
- [x] SELinux state reviewed successfully

---

## 🔁 Replicated Validation — `srv-db`
- [x] Phase 10 workspace created
- [x] `selinux/getenforce.txt` created
- [x] `selinux/sestatus.txt` created
- [x] `selinux/common-contexts.txt` created
- [x] `selinux/process-contexts.txt` created
- [x] `troubleshooting/ausearch-avc.txt` created
- [x] Final file listing validated
- [x] SELinux state reviewed successfully

---

## 🔁 Replicated Validation — `srv-storage`
- [x] Phase 10 workspace created
- [x] `selinux/getenforce.txt` created
- [x] `selinux/sestatus.txt` created
- [x] `selinux/common-contexts.txt` created
- [x] `selinux/process-contexts.txt` created
- [x] `troubleshooting/ausearch-avc.txt` created
- [x] Final file listing validated
- [x] SELinux state reviewed successfully

---

## 📸 Screenshot Validation
- [x] `P10-01-selinux-baseline-srv-admin.png`
- [x] `P10-02-context-visibility-srv-admin.png`
- [x] `P10-03-controlled-relabel-srv-admin.png`
- [x] `P10-04-troubleshooting-evidence-srv-admin.png`
- [x] `P10-05-final-workspace-srv-web.png`
- [x] `P10-06-final-workspace-srv-db.png`
- [x] `P10-07-final-workspace-srv-storage.png`

---

## 🏁 Final Phase 10 Outcome
- [x] `srv-admin` validated as the reference SELinux workflow node
- [x] SELinux baseline inspection completed successfully
- [x] Context visibility validated for files and processes
- [x] Controlled relabel validation completed with `chcon` and `restorecon`
- [x] Basic troubleshooting evidence captured successfully
- [x] Replicated short validation completed on `srv-web`, `srv-db`, and `srv-storage`
- [x] Screenshot evidence captured and documented
- [x] Phase 10 evidence captured and documented
- [x] Lab ready for **Phase 11: Final integrated validation**
