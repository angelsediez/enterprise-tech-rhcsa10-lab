# 🐚 Phase 03 - Shell, Files, and Local Documentation

## 🎯 Objective
Establish a validated Phase 03 workspace inside the guest systems to practice and document essential Linux administration tasks, including shell navigation, stream redirection, text processing, file operations, and local documentation lookup.

This phase begins with a **full workflow on `srv-admin`** and then reuses a shorter validation pattern on the remaining guests.

---

## 🔍 Scope

### **Included in this phase:**
* **Workspace Setup:** Creating a dedicated Phase 03 root inside the guests.
* **Stream Management:** Redirecting `stdout` and `stderr` into persistent files.
* **Text Processing:** Using `grep` and Regular Expressions (Regex) for data filtering.
* **File Operations:** Managing directories, hard links, and symbolic links.
* **Archiving:** Workflows for `.tar`, `.tar.gz`, and `.tar.bz2` creation/extraction.
* **System Docs:** Consulting `man`, `info`, `--help`, and `/usr/share/doc`.
* **Persistence:** Validating workspace integrity after guest reboots.

### **Excluded (Later Phases):**
* SSH hardening and advanced identity/permissions.
* SELinux policy work and Firewalld rules.
* NFS/autofs implementation.

---

## 💻 Phase Context

| Component | Details |
| :--- | :--- |
| **Phase Focus** | Shell, Files, and Local Documentation |
| **Primary Guest** | `srv-admin` (Full Workflow) |
| **Secondary Guests** | `srv-web`, `srv-db`, `srv-storage` (Validation Pattern) |
| **Guest Platform** | Red Hat Enterprise Linux 10.1 |
| **Workspace Root** | `~/lab/f03-shell-files-docs` |

---

## 🛠️ Execution Strategy
The phase follows a "Master & Replicate" model:

1. **Full Workflow:** Execute all shell and file operations on `srv-admin`.
2. **Persistence Check:** Confirm the workspace remains intact after a system reboot.
3. **Validation Pattern:** Reuse a condensed version of the commands on `srv-web`, `srv-db`, and `srv-storage`.
4. **Evidence:** Capture screenshots of command results and directory structures.
5. **Closure:** Confirm that the validated Phase 03 pattern now exists across all four guests.

---

## 📐 Workspace Design
The Phase 03 environment is structured to be self-contained and repeatable:

```text
~/lab/f03-shell-files-docs/
├── archive/    # Compressed files and tarballs
├── links/      # Hard and symbolic link tests
├── streams/    # Stdout and Stderr redirection logs
├── text/       # Sample data and Regex targets
└── tmp/        # Temporary operation tests
```

---

## ✅ Work Completed

### **Guest Status**
| Guest | Role | Status |
| :--- | :--- | :--- |
| `srv-admin` | Reference Node | ✅ Completed & Validated |
| `srv-web` | Web Service | ✅ Validation Pattern Completed |
| `srv-db` | Database Node | ✅ Validation Pattern Completed |
| `srv-storage` | Storage Node | ✅ Validation Pattern Completed |

### **Hitos Alcanzados (srv-admin)**
* [x] Phase 03 workspace structure created.
* [x] Stream redirection (stdout/stderr) validated.
* [x] Grep + Regex filtering confirmed.
* [x] Hard/Soft links cross-referenced by Inode.
* [x] Tarball compression (Gzip/Bzip2) and extraction verified.
* [x] Local documentation lookup (man/rpm) documented.
* [x] Post-reboot persistence confirmed.

### **Validation Pattern Reuse**
* [x] Short validation pattern executed on `srv-web`.
* [x] Short validation pattern executed on `srv-db`.
* [x] Short validation pattern executed on `srv-storage`.
* [x] Final replicated workspace evidence captured for all secondary guests.

---

## 🧪 srv-admin Command Flow

### 1. Workspace & Context
```bash
mkdir -p ~/lab/f03-shell-files-docs/{text,streams,archive,links,tmp}
cd ~/lab/f03-shell-files-docs
pwd && whoami && hostnamectl --static
```

### 2. Stream Redirection
```bash
hostnamectl --static > streams/hostname.txt
pwd >> streams/hostname.txt
ls /no/such/path 2> streams/error.log
cat streams/hostname.txt
cat streams/error.log
```

### 3. Text Processing & Regex
```bash
printf "alpha\nbeta\nalpha-01\nweb01\ndb01\nstorage01\n" > text/sample.txt
grep -n 'alpha' text/sample.txt
grep -E '^(alpha|web|db|storage)[-]?[0-9]*$' text/sample.txt > text/regex-hit.txt
cat text/regex-hit.txt
grep -En '^(root|jdoe):' /etc/passwd > text/passwd-match.txt
cat text/passwd-match.txt
```

### 4. File Operations
```bash
mkdir -p text/src
touch text/src/file1 text/src/file2
cp text/src/file1 text/src/file1.copy
mv text/src/file2 text/src/file2.moved
rm -f text/src/file1.copy
ls -l text/src
```

### 5. Link Validation
```bash
ln text/src/file1 links/file1.hard
ln -s ../text/src/file1 links/file1.soft
ls -li text/src/file1 links/file1.hard links/file1.soft
```

### 6. Text Editing
Create and save `text/notes.txt` with `vi`, then verify it:
```bash
cat text/notes.txt
```
*Expected content:*
```text
F03 workspace on srv-admin
shell, files, local docs completed
links and archives validated
```

### 7. Archive Validation
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

### 8. Local Documentation
```bash
ls --help | head
man ls | head -n 20
man -k tar
info tar | head -n 20 || true
ls /usr/share/doc | head
rpm -qd gzip | head
```

### 9. Post-Reboot Persistence
After reboot:
```bash
hostnamectl --static
find ~/lab/f03-shell-files-docs -maxdepth 3 -type f | sort
ls -li ~/lab/f03-shell-files-docs/text/src/file1 \
      ~/lab/f03-shell-files-docs/links/file1.hard \
      ~/lab/f03-shell-files-docs/links/file1.soft
```

---

## 🔁 Short Validation Pattern (srv-web, srv-db, srv-storage)

The following shortened validation block was executed on each secondary guest:

```bash
mkdir -p ~/lab/f03-shell-files-docs/{text,archive,links,streams}
cd ~/lab/f03-shell-files-docs
hostnamectl --static > streams/hostname.txt
printf "node=%s\nphase=F03\n" "$(hostnamectl --static)" > text/node.txt
cp text/node.txt text/node.copy
mv text/node.copy text/node.moved
ln text/node.txt links/node.hard
ln -s ../text/node.txt links/node.soft
tar -cvf archive/node.tar text
gzip -k archive/node.tar
man hostnamectl | head -n 15
cat text/node.txt
find ~/lab/f03-shell-files-docs -maxdepth 3 -type f | sort
ls -li text/node.txt links/node.hard links/node.soft
```

This pattern was completed on:
* `srv-web`
* `srv-db`
* `srv-storage`

---

## 📸 Screenshots
Evidence stored in `assets/screenshots/phase-03/`:

**srv-admin Core Evidence:**
* `P03-01-local-docs-srv-admin.png`
* `P03-02-redirection-stderr-srv-admin.png`
* `P03-03-grep-regex-srv-admin.png`
* `P03-04a-file-ops-srv-admin.png`
* `P03-04b-links-srv-admin.png`
* `P03-05-archives-srv-admin.png`
* `P03-06-edited-text-srv-admin.png`
* `P03-07-final-workspace-srv-admin.png`
* `P03-11-post-reboot-persistence-srv-admin.png`

**Secondary Guest Evidence:**
* `P03-08-final-workspace-srv-web.png`
* `P03-09-final-workspace-srv-db.png`
* `P03-10-final-workspace-srv-storage.png`

---

## 🏁 Current Outcome
Phase 03 is **complete** ✅.

A full shell/files/local-docs workflow was executed and validated on `srv-admin`, including post-reboot persistence. A shorter validation pattern was then successfully reused on `srv-web`, `srv-db`, and `srv-storage`.

The lab now has a documented and validated Phase 03 workspace across all four guests, providing a clean baseline for the next identity, SSH, and permissions phase.
