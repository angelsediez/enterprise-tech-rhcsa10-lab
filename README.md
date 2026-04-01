<div align="center">

# 🖥️ EnterpriseTech RHEL 10 Lab

### A production-style Red Hat Enterprise Linux 10.1 homelab built with KVM, QEMU, and libvirt

![Platform](https://img.shields.io/badge/Platform-RHEL%2010.1-red)
![Host](https://img.shields.io/badge/Host-Fedora%2043-blue)
![Virtualization](https://img.shields.io/badge/Virtualization-KVM%20%2B%20QEMU%20%2B%20libvirt-6f42c1)
![Status](https://img.shields.io/badge/Status-Active%20Development-brightgreen)
![Docs](https://img.shields.io/badge/Documentation-Phase--based-orange)

</div>

---

## 📌 Overview
This repository documents a **multi-VM Red Hat Enterprise Linux 10.1 homelab** designed to simulate a compact enterprise environment. It explores real-world Linux systems administration in a structured, repeatable, and well-documented way.

The lab is organized into **execution phases**, supported by professional runbooks, validation checklists, and technical evidence.

Phase 02 established a validated four-node RHEL 10.1 baseline.
Phase 03 established a validated shell, files, and local documentation workspace.
Phase 04 established a validated identity, SSH, and permissions baseline.
Phase 05 established a validated software inspection and scripting baseline.
Phase 06 established a validated running systems and service management baseline.
Phase 07 established a validated local storage and filesystem baseline.
Phase 08 established a validated networking and firewall baseline.
Phase 09 established a validated NFS and autofs baseline across the lab, preparing the environment for the next SELinux and troubleshooting phase.

---

## 🏗️ Lab Nodes
The infrastructure consists of four core virtual machines:

| Hostname | Role | Description |
| :--- | :--- | :--- |
| `srv-admin` | **Administration** | Management node and reference for full workflows. |
| `srv-web` | **Web Service** | Application node for web service simulation. |
| `srv-db` | **Database** | Backend node for structured data services. |
| `srv-storage` | **Storage** | Shared storage and NFS server node. |

---

## 🚀 Progress by Phase

- [x] **Phase 00** — Bootstrap and repository setup ✅
- [x] **Phase 01** — Virtualization host preparation ✅
- [x] **Phase 02** — Four-node RHEL 10.1 guest deployment baseline ✅
- [x] **Phase 03** — Shell, files, and local documentation baseline ✅
- [x] **Phase 04** — Identity, SSH, and permissions baseline ✅
- [x] **Phase 05** — Software and scripting baseline ✅
- [x] **Phase 06** — Running systems and service management baseline ✅
- [x] **Phase 07** — Local storage and filesystems baseline ✅
- [x] **Phase 08** — Networking and firewall baseline ✅
- [x] **Phase 09** — NFS and autofs baseline ✅
- [ ] **Phase 10** — SELinux and troubleshooting ⚪
- [ ] **Phase 11** — Final integrated validation ⚪

---

## 🖼️ Snapshot Gallery

### 🖥️ Host & Deployment (Phases 01-02)
**Host Identity & RHEL 10.1 Anaconda Summary**
![Deployment](assets/screenshots/phase-02/P02-26-anaconda-installation-summary-srv-storage.png)

### ⚙️ Services & Storage (Phases 06-07)
**srv-admin custom services & persistent mounts**
![Storage Baseline](assets/screenshots/phase-07/P07-05-persistent-mount-validation-srv-admin.png)

### 🌐 Networking & Firewall (Phase 08)
**srv-admin controlled firewall rule & node replication**
![Firewall Rule](assets/screenshots/phase-08/P08-04-controlled-firewall-rule-srv-admin.png)
![Replicated Workspace](assets/screenshots/phase-08/P08-08-final-workspace-srv-storage.png)

### 📂 NFS and autofs (Phase 09)
**srv-storage NFS server baseline and export validation**
![Phase 09 NFS Server](assets/screenshots/phase-09/P09-02-export-configuration-srv-storage.png)

**Replicated client validation through autofs**
![Phase 09 Client Validation](assets/screenshots/phase-09/P09-07-client-validation-srv-db.png)

---

## ✅ Active Development

> [!IMPORTANT]
> **Status:** Active Development  
> **Current Baseline:** RHEL 10.1 Deployment + All Phases up to Phase 09 NFS/autofs Complete.  
> **Next Milestone:** Phase 10 — SELinux and Troubleshooting

---

## 🧪 Current Lab Baseline Summary

**Phase 02:** Four-node RHEL 10.1 guest set (`srv-admin`, `srv-web`, `srv-db`, `srv-storage`) with UEFI/OVMF.
**Phase 03:** Shell environment, redirection, regex filtering, and archive handling.
**Phase 04:** Local identity, `sudo` access, and SSH service hardening.
**Phase 05:** RPM/DNF inspection and executable Bash scripting baseline.
**Phase 06:** `systemd` service management, `journalctl` logs, and custom unit creation.
**Phase 07:** GPT partitioning (`vdb`), XFS filesystem creation, and `/etc/fstab` persistence.
**Phase 08:** `ip/nmcli` addressing, routing, listener inspection, and `firewalld` zone/service management.
**Phase 09:** NFS export management on `srv-storage`, manual client-side NFS validation, and on-demand automount behavior through `autofs` on `srv-admin`, `srv-web`, and `srv-db`.

---

## 🔗 Key Files (All Phases)

### 📘 Runbooks
* `runbooks/rhel10-install.md` — Main installation guide.
* `runbooks/f03-shell-files-docs.md` | `runbooks/f04-identity-ssh-permissions.md`
* `runbooks/f05-software-and-scripting.md` | `runbooks/f06-running-systems-service-management.md`
* `runbooks/f07-local-storage-filesystems.md` | `runbooks/f08-networking-firewall.md`
* `runbooks/f09-nfs-autofs.md`

### 📋 Phase Reports
* `phases/01-virtualization-host/README.md` | `phases/02-rhel10-install/README.md`
* `phases/03-shell-files-docs/README.md` | `phases/04-identity-ssh-permissions/README.md`
* `phases/05-software-and-scripting/README.md` | `phases/06-running-systems-service-management/README.md`
* `phases/07-local-storage-filesystems/README.md` | `phases/08-networking-firewall/README.md`
* `phases/09-nfs-autofs/README.md`

### ✅ Validation Checklists
* `validation/03-shell-files-docs-checklist.md` | `validation/04-identity-ssh-permissions-checklist.md`
* `validation/05-software-and-scripting-checklist.md` | `validation/06-running-systems-service-management-checklist.md`
* `validation/07-local-storage-filesystems-checklist.md` | `validation/08-networking-firewall-checklist.md`
* `validation/09-nfs-autofs-checklist.md`

---

## 👤 Author
**Angel Diez**
