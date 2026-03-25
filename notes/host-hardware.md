# Host Hardware Notes

## Host Identity

- Hostname: `john-fa`
- Host OS: `Fedora Linux 43 (KDE Plasma Desktop Edition)`
- Release: `Fedora release 43 (Forty Three)`
- Kernel: `6.19.8-200.fc43.x86_64`
- Primary purpose: local virtualization host for the EnterpriseTech RHEL 10 lab

---

## CPU and Virtualization

- Architecture: `x86_64`
- Vendor: `AuthenticAMD`
- CPU model: `AMD Ryzen Threadripper 1950X 16-Core Processor`
- Hardware virtualization: `AMD-V`
- KVM device: `/dev/kvm`
- Current virtualization status: available and accessible

---

## User Access

- Primary local user: `john`
- Required groups confirmed:
  - `kvm`
  - `libvirt`

Example validation:
- `id "$USER"` shows membership in both groups

---

## Virtualization Stack

Installed and validated components:

- `qemu-kvm`
- `libvirt-daemon-kvm`
- `libvirt-daemon-config-network`
- `libvirt-client`
- `virt-install`
- `virt-manager`
- `virt-viewer`
- `edk2-ovmf`

Management model:
- libvirt system connection: `qemu:///system`

---

## Libvirt Services / Sockets

Validated libvirt components:

- `virtqemud.socket`
- `virtnetworkd.socket`
- `virtproxyd.socket`

Expected state:
- active
- enabled

---

## Storage Configuration

### Dedicated lab storage pool

- Pool name: `enterprise-tech-images`
- Pool type: `dir`
- Pool state: active
- Autostart: enabled
- Capacity: `474.34 GiB`
- Allocation: `80.00 GiB`
- Available: `394.34 GiB`

Purpose:
- store guest disks and related VM image files for the lab

---

## Lab Network

### Internal libvirt network

- Network name: `lab-int`
- Bridge: `virbr10`
- Network state: active
- Persistent: yes
- Autostart: yes
- Gateway IP: `192.168.150.1/24`
- DHCP range: `192.168.150.100` to `192.168.150.199`

Notes:
- `virbr10` may appear as `DOWN` before guests are attached
- the network is still valid if libvirt reports it as active and persistent

---

## Firmware / Guest Boot Support

- UEFI support package installed: `edk2-ovmf`
- Intended guest firmware model: OVMF / UEFI

---

## Planned Guest Platform

- Guest operating system target: `Red Hat Enterprise Linux 10.1`
- ISO file: `rhel-10.1-x86_64-dvd.iso`

Planned guest roles:
- `srv-admin`
- `srv-web`
- `srv-db`
- `srv-storage`

---

## Validation Summary

The following host-side requirements have already been validated:

- Fedora 43 host identity confirmed
- hardware virtualization confirmed
- `/dev/kvm` exists
- KVM/libvirt packages installed
- libvirt sockets active
- user group membership validated
- `virt-host-validate` executed
- storage pool created and active
- `lab-int` network created and active
- virt-manager opens successfully

---

## Notes

This file documents the real host baseline used to build the lab.

It should be updated only when:
- the host OS changes
- the kernel changes in a way that affects virtualization
- the virtualization stack changes
- the lab network changes
- the storage pool design changes
