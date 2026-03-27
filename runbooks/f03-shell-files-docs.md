# 🛠️ Phase 03 Runbook: Shell, Files, and Local Documentation

## 🎯 Objective
Execute and validate a practical Phase 03 workflow focused on core Linux administration tasks. This runbook establishes a consistent environment for shell navigation, stream management, text processing, and local documentation lookup.

This runbook uses `srv-admin` as the **full reference workflow** and applies a **shorter validation pattern** to `srv-web`, `srv-db`, and `srv-storage`.

---

## 🔍 Scope

### **Included in this phase:**
* Guest-side shell environment validation.
* Local workspace creation and structure.
* Stdout/Stderr redirection and logging.
* Text creation and filtering using `grep` and Regular Expressions.
* Standard file operations (Create, Copy, Move, Remove).
* Hard links and Symbolic links (Inode validation).
* Archive creation (`tar`) and compression (`gzip`, `bzip2`).
* Local documentation lookup (`man`, `info`, `rpm`).
* Post-reboot persistence validation on `srv-admin`.

### **Excluded (Later Phases):**
* SSH hardening and advanced permissions.
* SELinux policy tuning.
* Network and Firewall service exposure.
* NFS/autofs implementation.

---

## 💻 Prerequisites
Before proceeding, confirm the following status on the **Fedora 43 host**:

| Requirement | Status | Verification Command |
| :--- | :--- | :--- |
| **Guest Baseline** | Phase 02 Complete | `virsh list --all` |
| **Network** | `lab-int` Active | `virsh net-info lab-int` |
| **Accessibility** | User login available | `ssh <user>@srv-admin` |
| **Workspace Ready** | No previous F03 data | `ls -d ~/lab/f03-*` |

---

## 📍 Primary Workspace Structure
All work in this phase is performed under the following directory tree:

```text
~/lab/f03-shell-files-docs/
├── archive/    # Packaging and compression tests
├── docs/       # Manual page exports and help samples
├── links/      # Hard and symbolic link verification
├── streams/    # Redirection logs (stdout/stderr)
├── text/       # Sample data and regex targets
└── tmp/        # Temporary playground
```

---

## 🚀 Step-by-Step Execution: srv-admin (Full Workflow)

### 1. Workspace & Context Initialization
```bash
mkdir -p ~/lab/f03-shell-files-docs/{docs,text,streams,archive,links,tmp}
cd ~/lab/f03-shell-files-docs
pwd && whoami && hostnamectl --static
```
* **Purpose:** Creates a clean environment and confirms the guest identity.
* **Validation:** Run `find ~/lab/f03-shell-files-docs -maxdepth 1 -type d`.

### 2. Stream Management (Stdout/Stderr)
```bash
hostnamectl --static > streams/hostname.txt
pwd >> streams/hostname.txt
ls /no/such/path 2> streams/error.log
cat streams/hostname.txt && cat streams/error.log
```
* **Purpose:** Validates output redirection and separate error logging.
* **Validation:** Confirm `error.log` contains the "No such file" message.

### 3. Text Processing & Regex Filtering
```bash
printf "alpha\nbeta\nalpha-01\nweb01\ndb01\nstorage01\n" > text/sample.txt
grep -E '^(alpha|web|db|storage)[-]?[0-9]*$' text/sample.txt > text/regex-hit.txt
grep -En '^(root|jdoe):' /etc/passwd > text/passwd-match.txt
```
* **Purpose:** Creates controlled data and validates extended regex patterns.
* **Validation:** `cat text/regex-hit.txt` should show all lines except "beta".

### 4. File Operations & Inode Linking
```bash
# File Ops
mkdir -p text/src && touch text/src/file1 text/src/file2
cp text/src/file1 text/src/file1.copy
mv text/src/file2 text/src/file2.moved
rm -f text/src/file1.copy

# Links
ln text/src/file1 links/file1.hard
ln -s ../text/src/file1 links/file1.soft
```
* **Purpose:** Validates standard file lifecycle and link/inode behavior.
* **Validation:** `ls -li text/src/file1 links/file1.hard` (Must share same Inode).

### 5. Archiving & Compression
```bash
tar -cvf archive/text-src.tar text/src
gzip -k archive/text-src.tar
bzip2 -k archive/text-src.tar
tar -xzf archive/text-src.tar.gz -C archive/tmp/ # Test extraction
```
* **Purpose:** Validates packaging and different compression algorithms.

### 6. Local Documentation Lookup
```bash
man ls | head -n 20
man -k tar
rpm -qd gzip | head
```
* **Purpose:** Confirms access to internal system help and manual pages.

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
Run this block on each secondary guest to confirm pattern reuse:

```bash
mkdir -p ~/lab/f03-shell-files-docs/{text,archive,links,streams}
cd ~/lab/f03-shell-files-docs
hostnamectl --static > streams/hostname.txt
printf "node=%s\nphase=F03\n" "$(hostnamectl --static)" > text/node.txt
ln text/node.txt links/node.hard
ln -s ../text/node.txt links/node.soft
tar -cvf archive/node.tar text && gzip -k archive/node.tar
find ~/lab/f03-shell-files-docs -maxdepth 3 -type f | sort
```

---

## 🧪 Validation Checklist

### **srv-admin (Detailed)**
- [ ] Workspace structure confirmed (`find`).
- [ ] `streams/error.log` captured a 2> redirection.
- [ ] `text/regex-hit.txt` contains filtered data.
- [ ] Hard link shares the same Inode as source.
- [ ] Archive `text-src.tar.gz` is valid and readable.
- [ ] **Post-Reboot:** Workspace persists after `sudo reboot`.

### **Secondary Guests**
- [ ] `node.txt` reflects the correct hostname.
- [ ] Soft link is functional (not broken).
- [ ] Tarball `node.tar.gz` exists in `archive/`.

---

## 🧯 Troubleshooting
* **Broken Symlink:** If `links/file1.soft` is red/blinking, the relative path `../text/src/file1` is incorrect. Re-link from the `links/` directory.
* **Man pages missing:** RHEL 10.1 minimal might lack some pages. Use `dnf install man-pages` if required, or fallback to `<command> --help`.
* **Redirection failed:** Ensure you have write permissions in the `streams/` directory.

---

## 📸 Screenshots
Store evidence in `assets/screenshots/phase-03/`:

| ID | Description | Guest |
| :--- | :--- | :--- |
| **P03-01** | Local documentation lookup | `srv-admin` |
| **P03-02** | Redirection and error logging | `srv-admin` |
| **P03-03** | Grep and Regex results | `srv-admin` |
| **P03-04** | File operations and Link Inodes | `srv-admin` |
| **P03-05** | Archive and Compression results | `srv-admin` |
| **P03-07** | Final workspace tree | `srv-admin` |
| **P03-08..10** | Final workspace (Short pattern) | `srv-web/db/storage` |
| **P03-11** | Post-reboot persistence | `srv-admin` |

---

## 🏁 Outcome
Successful execution leaves:
1.  A validated **Phase 03 workspace** on all guests.
2.  Evidence of mastery over shell redirection, regex, and file handling.
3.  Confirmation of system persistence and documentation lookup capability.

