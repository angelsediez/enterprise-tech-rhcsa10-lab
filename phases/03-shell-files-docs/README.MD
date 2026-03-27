
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

1.  **Full Workflow:** Execute all shell and file operations on `srv-admin`.
2.  **Persistence Check:** Confirm the workspace remains intact after a system reboot.
3.  **Validation Pattern:** Reuse a condensed version of the commands on secondary guests.
4.  **Evidence:** Capture screenshots of command results and directory structures.

---

## 📐 Workspace Design
The Phase 03 environment is structured to be self-contained and repeatable:

```text
~/lab/f03-shell-files-docs/
├── archive/    # Compressed files and tarballs
├── docs/       # Man page exports and help samples
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
| `srv-web` | Web Service | 🚧 In Progress |
| `srv-db` | Database Node | ⚪ Pending |
| `srv-storage`| Storage Node | ⚪ Pending |

### **Hitos Alcanzados (srv-admin)**
* [x] Phase 03 workspace structure created.
* [x] Stream redirection (stdout/stderr) validated.
* [x] Grep + Regex filtering confirmed.
* [x] Hard/Soft links cross-referenced by Inode.
* [x] Tarball compression (Gzip/Bzip2) and extraction verified.
* [x] Local documentation lookup (man/rpm) documented.
* [x] Post-reboot persistence confirmed.

---

## 🧪 srv-admin Command Flow

### 1. Workspace & Context
```bash
mkdir -p ~/lab/f03-shell-files-docs/{docs,text,streams,archive,links,tmp}
cd ~/lab/f03-shell-files-docs
pwd && whoami && hostnamectl --static
```

### 2. Stream Redirection
```bash
hostnamectl --static > streams/hostname.txt
pwd >> streams/hostname.txt
ls /no/such/path 2> streams/error.log
```

### 3. Text Processing & Regex
```bash
printf "alpha\nbeta\nalpha-01\nweb01\ndb01\nstorage01\n" > text/sample.txt
grep -E '^(alpha|web|db|storage)[-]?[0-9]*$' text/sample.txt > text/regex-hit.txt
grep -En '^(root|jdoe):' /etc/passwd > text/passwd-match.txt
```

### 4. Link & Archive Validation
```bash
# Links
ln text/src/file1 links/file1.hard
ln -s ../text/src/file1 links/file1.soft
ls -li text/src/file1 links/file1.hard links/file1.soft

# Archives
tar -cvf archive/text-src.tar text/src
gzip -k archive/text-src.tar
bzip2 -k archive/text-src.tar
```

---

## 📸 Screenshots
Evidence stored in `assets/screenshots/phase-03/`:

**srv-admin Core Evidence:**
* `P03-01-local-docs-srv-admin.png`
* `P03-02-redirection-stderr-srv-admin.png`
* `P03-03-grep-regex-srv-admin.png`
* `P03-04a-file-ops-srv-admin.png`
* `P03-05-archives-srv-admin.png`
* `P03-11-post-reboot-persistence-srv-admin.png`

**Secondary Guests:**
* *Pending validation...*

---

## 🏁 Current Outcome
Phase 03 is **In Progress** 🚧.

A complete shell/files/local-docs workflow has been successfully executed and validated on `srv-admin`, including post-reboot persistence. The deployment pattern is ready to be applied to the remaining guests.
