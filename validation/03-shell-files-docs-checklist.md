# ✅ Phase 03 Validation Checklist - Shell, Files, and Local Documentation

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 03 across the four planned RHEL 10.1 guests.

It is intended to confirm that the shell, file, archive, link, and local documentation workflows were executed successfully and documented with evidence.

---

## 💻 Guest Coverage

| Guest | Status |
| :--- | :--- |
| `srv-admin` | ☐ Pending / ☐ Complete |
| `srv-web` | ☐ Pending / ☐ Complete |
| `srv-db` | ☐ Pending / ☐ Complete |
| `srv-storage` | ☐ Pending / ☐ Complete |

---

## 🧪 Detailed Validation - `srv-admin`

### Workspace and Shell Context
- [ ] Phase 03 workspace created under `~/lab/f03-shell-files-docs`
- [ ] Working directory validated with `pwd`
- [ ] Active user validated with `whoami`
- [ ] Hostname validated with `hostnamectl --static`

### Stdout / Stderr Redirection
- [ ] `streams/hostname.txt` created successfully
- [ ] `streams/error.log` created successfully
- [ ] stdout redirection validated
- [ ] stderr redirection validated

### Text Creation and Regex Filtering
- [ ] `text/sample.txt` created successfully
- [ ] `text/regex-hit.txt` created successfully
- [ ] `text/passwd-match.txt` created successfully
- [ ] `grep -n` output validated
- [ ] extended regex filtering validated
- [ ] `/etc/passwd` regex match validated

### File and Directory Operations
- [ ] `text/src/` created successfully
- [ ] `file1` created successfully
- [ ] `file2` created successfully
- [ ] copy operation validated
- [ ] move/rename operation validated
- [ ] file removal validated

### Hard Link and Symlink Validation
- [ ] hard link created successfully
- [ ] symbolic link created successfully
- [ ] inode validation performed with `ls -li`
- [ ] hard link shares inode with source file
- [ ] symbolic link points to the correct target

### Text Editing
- [ ] `text/notes.txt` created and saved
- [ ] edited text content validated with `cat`

### Archive and Compression Workflow
- [ ] `archive/text-src.tar` created successfully
- [ ] `archive/text-src.tar.gz` created successfully
- [ ] `archive/text-src.tar.bz2` created successfully
- [ ] gzip extraction validated
- [ ] bzip2 extraction validated
- [ ] archive contents validated with `tar -tf`

### Local Documentation Lookup
- [ ] `ls --help` consulted
- [ ] `man ls` consulted
- [ ] `man -k tar` consulted
- [ ] `info tar` attempted or validated
- [ ] `/usr/share/doc` consulted
- [ ] `rpm -qd gzip` consulted

### Final Validation
- [ ] final workspace listing completed
- [ ] final inode validation completed
- [ ] archive listing validation completed

### Post-Reboot Persistence
- [ ] `srv-admin` rebooted successfully
- [ ] workspace still present after reboot
- [ ] files still present after reboot
- [ ] hard link and symlink still present after reboot
- [ ] persistence screenshot captured

---

## 🔁 Short Validation - `srv-web`
- [ ] workspace created
- [ ] `streams/hostname.txt` created
- [ ] `text/node.txt` created
- [ ] `text/node.moved` created
- [ ] hard link created
- [ ] symbolic link created
- [ ] `archive/node.tar` created
- [ ] `archive/node.tar.gz` created
- [ ] `man hostnamectl` consulted
- [ ] final workspace listing completed
- [ ] inode validation completed
- [ ] screenshot captured as `P03-08-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [ ] workspace created
- [ ] `streams/hostname.txt` created
- [ ] `text/node.txt` created
- [ ] `text/node.moved` created
- [ ] hard link created
- [ ] symbolic link created
- [ ] `archive/node.tar` created
- [ ] `archive/node.tar.gz` created
- [ ] `man hostnamectl` consulted
- [ ] final workspace listing completed
- [ ] inode validation completed
- [ ] screenshot captured as `P03-09-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [ ] workspace created
- [ ] `streams/hostname.txt` created
- [ ] `text/node.txt` created
- [ ] `text/node.moved` created
- [ ] hard link created
- [ ] symbolic link created
- [ ] `archive/node.tar` created
- [ ] `archive/node.tar.gz` created
- [ ] `man hostnamectl` consulted
- [ ] final workspace listing completed
- [ ] inode validation completed
- [ ] screenshot captured as `P03-10-final-workspace-srv-storage.png`

---

## 📸 Screenshot Inventory
- [ ] `P03-01-local-docs-srv-admin.png`
- [ ] `P03-02-redirection-stderr-srv-admin.png`
- [ ] `P03-03-grep-regex-srv-admin.png`
- [ ] `P03-04a-file-ops-srv-admin.png`
- [ ] `P03-04b-links-srv-admin.png`
- [ ] `P03-05-archives-srv-admin.png`
- [ ] `P03-06-edited-text-srv-admin.png`
- [ ] `P03-07-final-workspace-srv-admin.png`
- [ ] `P03-08-final-workspace-srv-web.png`
- [ ] `P03-09-final-workspace-srv-db.png`
- [ ] `P03-10-final-workspace-srv-storage.png`
- [ ] `P03-11-post-reboot-persistence-srv-admin.png`

---

## 🏁 Phase 03 Exit Criteria
Phase 03 is considered complete when:

- [ ] `srv-admin` full workflow completed and validated
- [ ] `srv-admin` post-reboot persistence confirmed
- [ ] `srv-web` short validation completed
- [ ] `srv-db` short validation completed
- [ ] `srv-storage` short validation completed
- [ ] all expected screenshots captured
- [ ] `phases/03-shell-files-docs/README.md` updated
- [ ] `runbooks/f03-shell-files-docs.md` updated
- [ ] this checklist reviewed and completed
