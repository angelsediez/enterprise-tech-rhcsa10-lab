# ✅ Phase 09 Validation Checklist: NFS and autofs

## 🎯 Validation Goal
Confirm that Phase 09 established a working shared-storage baseline using NFS on `srv-storage` and validated both manual and automatic client-side access from `srv-admin`, `srv-web`, and `srv-db`.

---

## 🖥️ Server Validation — `srv-storage`

### NFS Server Baseline
- [x] `nfs-utils` installed
- [x] `rpcbind` service enabled
- [x] `rpcbind` service active
- [x] `nfs-server` service enabled
- [x] `nfs-server` service active

### Export Directory
- [x] `/srv/nfs/phase09-share` exists
- [x] `/srv/nfs/phase09-share/README.txt` exists
- [x] Export directory ownership validated
- [x] Export directory permissions validated

### Export Configuration
- [x] `/etc/exports.d/phase09.exports` created
- [x] Export points to `/srv/nfs/phase09-share`
- [x] Export restricted to `192.168.150.0/24`
- [x] `exportfs -rav` executed successfully
- [x] `exportfs -v` confirms export visibility
- [x] `showmount -e localhost` lists the export correctly

### Firewall and Listener Validation
- [x] `firewalld` allows `nfs`
- [x] `firewalld` allows `mountd`
- [x] `firewalld` allows `rpc-bind`
- [x] Runtime firewall rules persisted successfully
- [x] Listener validation confirms activity on ports `2049` and `111`

### Server Evidence
- [x] `server/nfs-utils.txt` created
- [x] `server/rpcbind-status.txt` created
- [x] `server/nfs-server-status.txt` created
- [x] `server/nfs-server-enabled.txt` created
- [x] `server/nfs-server-active.txt` created
- [x] `server/phase09-exports.txt` created
- [x] `server/exportfs-v.txt` created
- [x] `server/showmount-localhost.txt` created
- [x] `server/firewall-services.txt` created

---

## 🧪 Reference Client Validation — `srv-admin`

### Manual NFS Access
- [x] `showmount -e 192.168.150.115` lists the export
- [x] `/mnt/phase09-nfs` created
- [x] Manual NFS mount completed successfully
- [x] Exported `README.txt` readable from client
- [x] `mount | grep phase09-nfs` confirms active manual mount

### autofs Validation
- [x] `/etc/auto.master.d/phase09.autofs` created
- [x] `/etc/auto.phase09` created
- [x] `autofs` enabled
- [x] `autofs` active
- [x] Access to `/mnt/phase09-auto` triggered automount successfully
- [x] Exported `README.txt` readable through `autofs`
- [x] `mount | grep phase09-auto` confirms active automount

### Reference Client Evidence
- [x] `clients/showmount-srv-storage.txt` created
- [x] `clients/mount-phase09-nfs.txt` created
- [x] `clients/ls-phase09-nfs.txt` created
- [x] `clients/readme-phase09-nfs.txt` created
- [x] `autofs/autofs-package.txt` created
- [x] `autofs/autofs-status.txt` created
- [x] `autofs/auto-master-phase09.txt` created
- [x] `autofs/auto-phase09.txt` created
- [x] `autofs/mount-phase09-auto.txt` created

---

## 🔁 Replicated Client Validation — `srv-web`
- [x] Phase 09 workspace created
- [x] Manual export discovery validated
- [x] Manual NFS mount validated
- [x] Exported `README.txt` readable
- [x] `autofs` configuration created
- [x] `autofs` trigger validated on `/mnt/phase09-auto`
- [x] Final evidence files created under `clients/` and `autofs/`

---

## 🔁 Replicated Client Validation — `srv-db`
- [x] Phase 09 workspace created
- [x] Manual export discovery validated
- [x] Manual NFS mount validated
- [x] Exported `README.txt` readable
- [x] `autofs` configuration created
- [x] `autofs` trigger validated on `/mnt/phase09-auto`
- [x] Final evidence files created under `clients/` and `autofs/`

---

## 📸 Screenshot Validation
- [x] `P09-01-nfs-server-baseline-srv-storage.png`
- [x] `P09-02-export-configuration-srv-storage.png`
- [x] `P09-03-nfs-service-firewall-srv-storage.png`
- [x] `P09-04-client-mount-validation-srv-admin.png`
- [x] `P09-05-autofs-validation-srv-admin.png`
- [x] `P09-06-client-validation-srv-web.png`
- [x] `P09-07-client-validation-srv-db.png`

---

## 🏁 Final Phase 09 Outcome
- [x] `srv-storage` validated as the reference NFS server
- [x] Shared export available to the lab network
- [x] Manual client NFS access validated
- [x] `autofs` on-demand access validated
- [x] Reference and replicated client workflows completed
- [x] Phase 09 evidence captured and documented
- [x] Lab ready for **Phase 10: SELinux and troubleshooting**
