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
* Text editing with `vi`.
* Hard links and Symbolic links (Inode validation).
* Archive creation (`tar`) and compression (`gzip`, `bzip2`).
* Local documentation lookup (`man`, `info`, `rpm`, `--help`).
* Post-reboot persistence validation on `srv-admin`.

---

## 💻 Prerequisites
Before proceeding, confirm the following baseline conditions:

| Requirement | Status | Verification Command |
| :--- | :--- | :--- |
| **Guest Baseline** | Phase 02 Complete | `virsh list --all` |
| **Network** | `lab-int` Active | `virsh net-info lab-int` |
| **Accessibility** | Guest login available | Console login via `virt-viewer` or direct access |
| **Workspace Ready** | No previous F03 data | Check inside guests: `ls -d ~/lab/f03-*` |

---

## 📍 Primary Workspace Structure
The Phase 03 workspace is organized as follows:

```text
~/lab/f03-shell-files-docs/
├── archive/    # Tarballs, gzip, bzip2, and extraction tests
├── links/      # Hard and symbolic link verification
├── streams/    # Redirection logs (stdout/stderr)
├── text/       # Sample data and edited text files
└── tmp/        # Temporary extraction and testing area
```

---

## 🚀 Step-by-Step Execution: srv-admin (Full Workflow)

### 1. Workspace & Context Initialization
```bash
mkdir -p ~/lab/f03-shell-files-docs/{text,streams,archive,links,tmp}
cd ~/lab/f03-shell-files-docs
pwd && whoami && hostnamectl --static
```

### 2. Stream Management (Stdout/Stderr)
```bash
hostnamectl --static > streams/hostname.txt
pwd >> streams/hostname.txt
ls /no/such/path 2> streams/error.log
cat streams/hostname.txt && cat streams/error.log
```

### 3. Text Processing & Regex Filtering
```bash
printf "alpha\nbeta\nalpha-01\nweb01\ndb01\nstorage01\n" > text/sample.txt
grep -n 'alpha' text/sample.txt
grep -E '^(alpha|web|db|storage)[-]?[0-9]*$' text/sample.txt > text/regex-hit.txt
cat text/regex-hit.txt
grep -En '^(root|jdoe):' /etc/passwd > text/passwd-match.txt
cat text/passwd-match.txt
```

### 4. File Operations & Inode Linking
```bash
# File Ops
mkdir -p text/src && touch text/src/file1 text/src/file2
cp text/src/file1 text/src/file1.copy
mv text/src/file2 text/src/file2.moved
rm -f text/src/file1.copy
ls -l text/src

# Link Validation
ln text/src/file1 links/file1.hard
ln -s ../text/src/file1 links/file1.soft
ls -li text/src/file1 links/file1.hard links/file1.soft
```

### 5. Text Editing Workflow
Use `vi text/notes.txt` to save the following content:
```text
F03 workspace on srv-admin
shell, files, local docs completed
links and archives validated
```
Then verify: `cat text/notes.txt`

### 6. Archiving & Compression
```bash
tar -cvf archive/text-src.tar text/src
gzip -k archive/text-src.tar
bzip2 -k archive/text-src.tar
mkdir -p tmp/extract-gz tmp/extract-bz2
tar -xzf archive/text-src.tar.gz -C tmp/extract-gz
tar -xjf archive/text-src.tar.bz2 -C tmp/extract-bz2
find archive -maxdepth 1 -type f | sort
find tmp -maxdepth 3 -type f | sort
```

### 7. Local Documentation Lookup
```bash
ls --help | head
man ls | head -n 20
man -k tar
info tar | head -n 20 || true
ls /usr/share/doc | head
rpm -qd gzip | head
```

### 8. Post-Reboot Persistence Validation
Perform a system reboot to ensure the workspace is persistent:
```bash
sudo systemctl reboot
```
After logging back in:
```bash
hostnamectl --static
find ~/lab/f03-shell-files-docs -maxdepth 3 -type f | sort
ls -li ~/lab/f03-shell-files-docs/text/src/file1 \
      ~/lab/f03-shell-files-docs/links/file1.hard \
      ~/lab/f03-shell-files-docs/links/file1.soft
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)
Run this block on each secondary guest to confirm pattern reuse:

```bash
mkdir -p ~/lab/f03-shell-files-docs/{text,archive,links,streams}
cd ~/lab/f03-shell-files-docs
hostnamectl --static > streams/hostname.txt
printf "node=%s\nphase=F03\n" "$(hostnamectl --static)" > text/node.txt
cp text/node.txt text/node.copy
mv text/node.copy text/node.moved
ln text/node.txt links/node.hard
ln -s ../text/node.txt links/node.soft
tar -cvf archive/node.tar text && gzip -k archive/node.tar
man hostnamectl | head -n 15
cat text/node.txt
find ~/lab/f03-shell-files-docs -maxdepth 3 -type f | sort
ls -li text/node.txt links/node.hard links/node.soft
```

---

## 🧪 Validation Checklist

### **srv-admin (Detailed)**
- [ ] `streams/error.log` captured a 2> redirection.
- [ ] `text/regex-hit.txt` contains filtered data (no "beta").
- [ ] `text/notes.txt` created and content verified.
- [ ] Hard link shares the same Inode as source.
- [ ] `text-src.tar.gz` and `text-src.tar.bz2` created and extracted.
- [ ] **Post-Reboot:** Workspace persists after `sudo reboot`.

### **Secondary Guests**
- [ ] `node.txt` reflects the correct hostname.
- [ ] `node.moved` exists in `text/`.
- [ ] Hard link shares Inode with `node.txt`.
- [ ] Tarball `node.tar.gz` exists in `archive/`.

---

## 📸 Screenshots
Store evidence in `assets/screenshots/phase-03/`:

| ID | Description | Guest |
| :--- | :--- | :--- |
| **P03-01** | Local documentation lookup | `srv-admin` |
| **P03-02** | Redirection and error logging | `srv-admin` |
| **P03-03** | Grep and Regex results | `srv-admin` |
| **P03-04** | File operations | `srv-admin` |
| **P03-04b** | Hard link and symlink validation | `srv-admin` |
| **P03-05** | Archive and Compression results | `srv-admin` |
| **P03-06** | Edited text validation | `srv-admin` |
| **P03-07** | Final workspace tree | `srv-admin` |
| **P03-08** | Final replicated workspace | `srv-web` |
| **P03-09** | Final replicated workspace | `srv-db` |
| **P03-10** | Final replicated workspace | `srv-storage` |
| **P03-11** | Post-reboot persistence | `srv-admin` |

---

## 🏁 Outcome
Successful execution leaves a validated Phase 03 workspace on all guests, with documented evidence of shell redirection, text processing, file handling, archiving, link validation, and local documentation lookup.
