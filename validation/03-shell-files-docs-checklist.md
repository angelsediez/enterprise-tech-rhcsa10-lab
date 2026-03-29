# ✅ Phase 03 Validation Checklist - Shell, Files, and Local Documentation

## 🎯 Purpose
This checklist tracks the completion and validation status of Phase 03 across the four planned RHEL 10.1 guests.

It is intended to confirm that the shell, file, archive, link, and local documentation workflows were executed successfully and documented with evidence.

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
- [x] Phase 03 workspace created under `~/lab/f03-shell-files-docs`
- [x] Working directory validated with `pwd`
- [x] Active user validated with `whoami`
- [x] Hostname validated with `hostnamectl --static`

### Stdout / Stderr Redirection
- [x] `streams/hostname.txt` created successfully
- [x] `streams/error.log` created successfully
- [x] stdout redirection validated
- [x] stderr redirection validated

### Text Creation and Regex Filtering
- [x] `text/sample.txt` created successfully
- [x] `text/regex-hit.txt` created successfully
- [x] `text/passwd-match.txt` created successfully
- [x] `grep -n` output validated
- [x] extended regex filtering validated
- [x] `/etc/passwd` regex match validated

### File and Directory Operations
- [x] `text/src/` created successfully
- [x] `file1` created successfully
- [x] `file2` created successfully
- [x] copy operation validated
- [x] move/rename operation validated
- [x] file removal validated

### Hard Link and Symlink Validation
- [x] hard link created successfully
- [x] symbolic link created successfully
- [x] inode validation performed with `ls -li`
- [x] hard link shares inode with source file
- [x] symbolic link points to the correct target

### Text Editing
- [x] `text/notes.txt` created and saved
- [x] edited text content validated with `cat`

### Archive and Compression Workflow
- [x] `archive/text-src.tar` created successfully
- [x] `archive/text-src.tar.gz` created successfully
- [x] `archive/text-src.tar.bz2` created successfully
- [x] gzip extraction validated
- [x] bzip2 extraction validated
- [x] archive contents validated with `tar -tf`

### Local Documentation Lookup
- [x] `ls --help` consulted
- [x] `man ls` consulted
- [x] `man -k tar` consulted
- [x] `info tar` attempted or validated
- [x] `/usr/share/doc` consulted
- [x] `rpm -qd gzip` consulted

### Final Validation
- [x] final workspace listing completed
- [x] final inode validation completed
- [x] archive listing validation completed

### Post-Reboot Persistence
- [x] `srv-admin` rebooted successfully
- [x] workspace still present after reboot
- [x] files still present after reboot
- [x] hard link and symlink still present after reboot
- [x] persistence screenshot captured

---

## 🔁 Short Validation - `srv-web`
- [x] workspace created
- [x] `streams/hostname.txt` created
- [x] `text/node.txt` created
- [x] `text/node.moved` created
- [x] hard link created
- [x] symbolic link created
- [x] `archive/node.tar` created
- [x] `archive/node.tar.gz` created
- [x] `man hostnamectl` consulted
- [x] final workspace listing completed
- [x] inode validation completed
- [x] screenshot captured as `P03-08-final-workspace-srv-web.png`

---

## 🔁 Short Validation - `srv-db`
- [x] workspace created
- [x] `streams/hostname.txt` created
- [x] `text/node.txt` created
- [x] `text/node.moved` created
- [x] hard link created
- [x] symbolic link created
- [x] `archive/node.tar` created
- [x] `archive/node.tar.gz` created
- [x] `man hostnamectl` consulted
- [x] final workspace listing completed
- [x] inode validation completed
- [x] screenshot captured as `P03-09-final-workspace-srv-db.png`

---

## 🔁 Short Validation - `srv-storage`
- [x] workspace created
- [x] `streams/hostname.txt` created
- [x] `text/node.txt` created
- [x] `text/node.moved` created
- [x] hard link created
- [x] symbolic link created
- [x] `archive/node.tar` created
- [x] `archive/node.tar.gz` created
- [x] `man hostnamectl` consulted
- [x] final workspace listing completed
- [x] inode validation completed
- [x] screenshot captured as `P03-10-final-workspace-srv-storage.png`

---

## 📸 Screenshot Inventory
- [x] `P03-01-local-docs-srv-admin.png`
- [x] `P03-02-redirection-stderr-srv-admin.png`
- [x] `P03-03-grep-regex-srv-admin.png`
- [x] `P03-04a-file-ops-srv-admin.png`
- [x] `P03-04b-links-srv-admin.png`
- [x] `P03-05-archives-srv-admin.png`
- [x] `P03-06-edited-text-srv-admin.png`
- [x] `P03-07-final-workspace-srv-admin.png`
- [x] `P03-08-final-workspace-srv-web.png`
- [x] `P03-09-final-workspace-srv-db.png`
- [x] `P03-10-final-workspace-srv-storage.png`
- [x] `P03-11-post-reboot-persistence-srv-admin.png`

---

## 🏁 Phase 03 Exit Criteria
Phase 03 is considered complete when:

- [x] `srv-admin` full workflow completed and validated
- [x] `srv-admin` post-reboot persistence confirmed
- [x] `srv-web` short validation completed
- [x] `srv-db` short validation completed
- [x] `srv-storage` short validation completed
- [x] all expected screenshots captured
- [x] `phases/03-shell-files-docs/README.md` updated
- [x] `runbooks/f03-shell-files-docs.md` updated
- [x] this checklist reviewed and completed
