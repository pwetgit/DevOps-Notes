![Image](https://www.proxmox.com/images/proxmox/Proxmox_logo_standard_hex_400px.png)

# Proxmox Useful Commands

---

## 🛠️ Configuration

### 🔄 Switch to Community Repository
```bash
cd /etc/apt/sources.list.d
mv pve-enterprise.list pve-enterprise.list.original
echo 'deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription' > pve-community.list
# Change debian version according to yours (here bullseye)
mv pve-enterprise.list.original /root
```

### 🔄 Update the System
```bash
apt update
apt -y dist-upgrade
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
qm importdisk ${ID_VM} ${RELEASE}-server-cloudimg-amd64.img local-lvm
```

### 🔗 Attach the Disk to the VM
```bash
# Attach the new disk to the VM as a SCSI drive on the SCSI controller
qm set ${ID_VM} --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-${ID_VM}-disk-0
```

### ☁️ Add a Cloud-Init Drive
```bash
# Add cloud-init drive
qm set ${ID_VM} --ide1 local-lvm:cloudinit
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