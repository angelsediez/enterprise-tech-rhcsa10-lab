# Phase 01 - Virtualization Host

## Objective

Prepare the Fedora 43 host to support the multi-VM lab using KVM, QEMU, libvirt, and virt-manager.

## Host Platform

- Host OS: Fedora Linux 43 (KDE Plasma Desktop Edition)
- Host role: local virtualization platform
- Guest target: RHEL 10.1 virtual machines
- ISO planned for guests: `rhel-10.1-x86_64-dvd.iso`

## Work Completed

- Verified CPU virtualization support
- Verified `/dev/kvm` availability
- Installed virtualization packages
- Enabled and validated libvirt sockets/services
- Added the local user to `libvirt` and `kvm`
- Created and validated the `lab-int` virtual network
- Created and validated the storage pool for lab images
- Confirmed system connection through `qemu:///system`
- Validated host readiness for guest deployment

## Design Decisions

- Fedora 43 is used as the host platform
- RHEL 10.1 will be used only inside guest VMs
- `qemu:///system` is used for system-wide virtualization management
- `lab-int` is used as the internal lab network
- KVM + QEMU + libvirt + virt-manager are the primary virtualization tools

## Validation Summary

- `virt-host-validate` completed successfully
- libvirt sockets are active
- virtualization packages are installed
- user access groups are correct
- storage pool is active
- `lab-int` network is defined and active

## Screenshots

Store screenshots in:
- `assets/screenshots/phase-01/`

Suggested captures:
- host release info
- virtualization capability
- installed packages
- libvirt sockets active
- group membership
- `virt-host-validate`
- storage pool
- `lab-int` network
- virt-manager connection

## Related Files

- `runbooks/kvm-libvirt-host-setup.md`
- `notes/host-hardware.md`
- `phases/01-virtualization-host/lab-int.xml`

## Outcome

Phase 01 completed successfully. The host is ready to begin guest creation and RHEL 10.1 installation in the next phase.
