# KVM / Libvirt Host Setup

## Objective
Prepare the **Fedora 43** host to support the EnterpriseTech RHEL 10 lab using KVM, QEMU, libvirt, and virt-manager.

This runbook documents the host-side setup required before creating any guest virtual machines.

---

## Scope
This runbook covers:
* Host validation
* CPU virtualization checks
* `/dev/kvm` availability
* Package installation
* Libvirt service/socket validation
* Local user group access
* Storage pool preparation
* Internal lab network definition
* Host readiness verification

> [!NOTE]
> This runbook does **not** cover guest installation. Guest creation and RHEL 10.1 installation belong to the next phase.

---

## Host Information
* **Host OS:** Fedora Linux 43 (KDE Plasma Desktop Edition)
* **Host Purpose:** Local virtualization platform
* **Guest OS Target:** RHEL 10.1
* **Guest ISO:** `rhel-10.1-x86_64-dvd.iso`

---

## Prerequisites
Before starting, confirm:
* Virtualization is enabled in firmware (Intel VT-x / AMD-V).
* The host has enough RAM and disk space for multiple VMs.
* The RHEL 10.1 ISO is already downloaded and stored locally.
* The current user has `sudo` privileges.

---

## Implementation Steps

### Step 1 - Confirm Host Identity
```bash
cat /etc/os-release
cat /etc/redhat-release
uname -r
```
* **Why:** Confirms the exact host OS and kernel before installing components.
* **Action:** Reads release metadata and shows the running kernel.
* **Verification:** Output should clearly show **Fedora Linux 43**.

### Step 2 - Confirm Hardware Virtualization Support
```bash
lscpu | egrep 'Architecture|Vendor ID|Model name|Virtualization|VT-x|AMD-V'
ls -l /dev/kvm
```
* **Why:** KVM requires hardware virtualization support and access to the `/dev/kvm` device.
* **Verification:** You should see virtualization capability in `lscpu` output and a valid `/dev/kvm` device.

### Step 3 - Check Available Memory and Storage
```bash
free -h
df -h
lsblk
```
* **Why:** A multi-VM lab needs enough resources before images are created.
* **Verification:** Confirm enough free RAM and storage for the planned VMs.

### Step 4 - Install Virtualization Packages
```bash
sudo dnf install -y qemu-kvm libvirt-daemon-kvm libvirt-daemon-config-network libvirt-client virt-install virt-manager virt-viewer edk2-ovmf
```
* **Why:** Provides hypervisor, management layer, networking, GUI tools, and UEFI firmware.
* **Verification:**
```bash
rpm -q qemu-kvm libvirt-daemon-kvm libvirt-daemon-config-network libvirt-client virt-install virt-manager virt-viewer edk2-ovmf
```

### Step 5 - Enable and Validate Libvirt Sockets
```bash
sudo systemctl enable --now virtqemud.socket virtnetworkd.socket virtproxyd.socket
systemctl status virtqemud.socket virtnetworkd.socket virtproxyd.socket --no-pager
systemctl is-enabled virtqemud.socket virtnetworkd.socket virtproxyd.socket
```
* **Why:** These sockets support system-level virtualization management.
* **Verification:** Sockets should show as **active** and **enabled**.

### Step 6 - Add Local User to Virtualization Groups
```bash
sudo usermod -aG libvirt,kvm "$USER"
id "$USER"
```
* **Why:** Allows managing resources without using `sudo` for every command.
* **Note:** You must **log out and log back in** for changes to take effect.
* **Verification:** `id "$USER"` should include both `libvirt` and `kvm`.

### Step 7 - Validate Libvirt System Connection
```bash
virsh -c qemu:///system uri
virt-host-validate
```
* **Why:** Confirms host-to-libvirt communication and core requirements.
* **Verification:** `virsh` should return `qemu:///system`. `virt-host-validate` should pass.

### Step 8 - Prepare a Storage Pool Directory
```bash
sudo mkdir -p /var/lib/libvirt/images/enterprise-tech
sudo virsh -c qemu:///system pool-define-as enterprise-tech-images dir --target /var/lib/libvirt/images/enterprise-tech
sudo virsh -c qemu:///system pool-start enterprise-tech-images
sudo virsh -c qemu:///system pool-autostart enterprise-tech-images
virsh -c qemu:///system pool-list --all
virsh -c qemu:///system pool-info enterprise-tech-images
```
* **Why:** Organizes lab disk images in a reproducible way.
* **Verification:** Pool should appear as **active** and **autostart** enabled.

### Step 9 - Define the Internal Lab Network
1. Create the definition file: `phases/01-virtualization-host/lab-int.xml`
```xml
<network>
  <name>lab-int</name>
  <bridge name='virbr10' stp='on' delay='0'/>
  <ip address='192.168.110.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.110.100' end='192.168.110.200'/>
    </dhcp>
  </ip>
</network>
```
2. Apply the configuration:
```bash
sudo virsh -c qemu:///system net-define phases/01-virtualization-host/lab-int.xml
sudo virsh -c qemu:///system net-start lab-int
sudo virsh -c qemu:///system net-autostart lab-int
virsh -c qemu:///system net-list --all
ip -brief addr show virbr10
```

### Step 10 - Confirm UEFI Support
```bash
ls /usr/share/OVMF/OVMF_CODE.fd /usr/share/OVMF/OVMF_VARS.fd
virsh -c qemu:///system domcapabilities | grep -A8 '<loader'
```
* **Why:** RHEL 10 guests benefit from UEFI support via OVMF.

### Step 11 - Confirm Virt-Manager Access
```bash
virt-manager --connect qemu:///system
```
* **Verification:** The GUI should open and connect successfully to the system hypervisor.

---

## Validation Checklist
- [ ] Fedora 43 host identity documented.
- [ ] Hardware virtualization available (`/dev/kvm` exists).
- [ ] Virtualization packages installed.
- [ ] Libvirt sockets active and enabled.
- [ ] Local user in `libvirt` and `kvm` groups.
- [ ] `virt-host-validate` passes.
- [ ] Storage pool `enterprise-tech-images` active and autostarted.
- [ ] Network `lab-int` active and autostarted.
- [ ] OVMF firmware files present.
- [ ] `virt-manager` connects successfully.

---

## Screenshots to Capture
*Store in: `assets/screenshots/phase-01/`*
1. Fedora release and kernel info.
2. CPU virtualization support results.
3. Active libvirt sockets status.
4. Storage pool and lab-int network status.
5. Virt-manager connected session.

---

## Related Files
* `phases/01-virtualization-host/lab-int.xml`
* `notes/host-hardware.md`
