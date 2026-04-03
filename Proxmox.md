![Image](https://www.proxmox.com/images/proxmox/Proxmox_logo_standard_hex_400px.png)

# Proxmox Useful Commands

---

## 🛠️ Configuration

### 🔄 Use post-install ProxmoxVE script
```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/post-pve-install.sh)"
```

### 🔄 Update the System
```bash
apt update && apt -y dist-upgrade
```

### Storage
(optionnal)

Remove backup from local storage
Add container and disk images to local storage
Remove local-lvm

```bash
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
```
### Terraform
https://registry.terraform.io/providers/Terraform-for-Proxmox/proxmox/latest/docs

```bash
# Create Terraform Role
pveum role add TerraformUser -privs "Datastore.AllocateSpace Datastore.Audit Pool.Allocate Sys.Audit Sys.Console Sys.Modify VM.Allocate VM.Audit VM.Clone VM.Config.CDROM VM.Config.Cloudinit VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Migrate VM.PowerMgmt SDN.Use"

# Create Terraform User
pveum user add terraform@pve --password <password>
pveum aclmod / -user terraform@pve -role TerraformUser

# Create token for user and save secret value
pveum user token add terraform@pve terraform-token --privsep 0
```

### Monitoring
```bash
# Create Monitoring User
pveum user add monitoring@pve --password <password>
pveum aclmod / -user monitoring@pve -role PVEAuditor

# Create token for user and save secret value
pveum user token add monitoring@pve monitoring-token --privsep 0
```

---

## ☁️ Cloud Image Setup

### 📥 Download an Ubuntu Cloud Image
```bash
cd /var/lib/vz/template/iso

RELEASE=noble

# Download the image
wget https://cloud-images.ubuntu.com/${RELEASE}/current/${RELEASE}-server-cloudimg-amd64.img
```

### 🖥️ Create a New Virtual Machine
```bash
ID_VM=1000
STORAGE=local-lvm

# Create a new virtual machine
qm create ${ID_VM} --memory 2048 --core 2 --name ubuntu-${RELEASE}-ci --net0 virtio,bridge=vmbr0
```

### 💾 Import the Ubuntu Disk
```bash
# Import the downloaded Ubuntu disk to local-lvm storage
qm importdisk ${ID_VM} ${RELEASE}-server-cloudimg-amd64.img ${STORAGE}
```

### 🔗 Attach the Disk to the VM
```bash
# Attach the new disk to the VM as a SCSI drive on the SCSI controller
qm set ${ID_VM} --scsihw virtio-scsi-pci --scsi0 ${STORAGE}:${ID_VM}/vm-${ID_VM}-disk-0.raw
```

### ☁️ Add a Cloud-Init Drive
```bash
# Add cloud-init drive
qm set ${ID_VM} --ide1 ${STORAGE}:cloudinit
```

### 🚀 Configure Boot Settings
```bash
# Make the cloud-init drive bootable and restrict BIOS to boot from disk only
qm set ${ID_VM} --boot c --bootdisk scsi0
```

### 🖥️ Add a Serial Console
```bash
# Add serial console
qm set ${ID_VM} --serial0 socket --vga serial0
```

### 🖼️ Convert VM to Template
```bash
# CHange Disk size from and Configure Cloud-Init Drive parameters (GUI) and transform VM to template
qm template ${ID_VM}
```

---

## 🖴 Passing a Physical Drive to a VM

### 📋 List Available Drives
```bash
ls -n /dev/disk/by-id/
# Example output:
# ata-SAMSUNG_HD103UJ_S13PJDWS665103 -> ../../sdb
```

### 🔗 Attach a Physical Drive to a VM
```bash
# Attach disk to VM
/sbin/qm set <vm_id> -virtio2 /dev/disk/by-id/ata-SAMSUNG_HD103UJ_S13PJDWS665103

# NB: Disable backup of disk if used as file storage for example
```

---

## 🌐 Change Proxmox IP Address
```bash
# Edit the network configuration file
nano /etc/network/interfaces
nano /etc/hosts

# Restart the network service
systemctl restart networking
```

---

## 📥 Install VirtIO Drivers for Windows
For Windows VMs, install VirtIO drivers in the guest machine beforehand:
[Proxmox VirtIO Drivers](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers)