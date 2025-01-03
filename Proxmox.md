![Image](https://www.proxmox.com/images/proxmox/Proxmox_logo_standard_hex_400px.png)

## 🛠️ Configuration 

```bash
cd /etc/apt/sources.list.d
mv pve-enterprise.list pve-enterprise.list.original
echo 'deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription' > pve-community.list
# Change debian version according to yours (here bullseye)
mv pve-enterprise.list.original /root

# Updating system
apt update
apt -y dist-upgrade
```

Recupération d'une image cloud Ubuntu:


```bash
cd /var/lib/vz/template/iso

# Download the image
wget https://cloud-images.ubuntu.com/lunar/current/lunar-server-cloudimg-amd64.img

ID_VM=1000

# Create a new virtual machine
qm create ${ID_VM} --memory 2048 --core 2 --name ubuntu-cloud --net0 virtio,bridge=vmbr0

# Import the downloaded Ubuntu disk to local-lvm storage
qm importdisk ${ID_VM} lunar-server-cloudimg-amd64.img local-lvm

# Attach the new disk to the vm as a scsi drive on the scsi controller
qm set ${ID_VM} --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-${ID_VM}-disk-0

# Add cloud init drive
qm set ${ID_VM} --ide1 local-lvm:cloudinit

# Make the cloud init drive bootable and restrict BIOS to boot from disk only
qm set ${ID_VM} --boot c --bootdisk scsi0

# Add serial console
qm set ${ID_VM} --serial0 socket --vga serial0

# Configure Cloud-Init Drive parameters (GUI) and Transform VM to Template
qm template ${ID-VM}

```

### Passing a Physical Drive through to a VM in ProxMox

For Windows Install VirtIO drivers in guest machine beforehand:
https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers

```bash
ls -n /dev/disk/by-id/
ata-SAMSUNG_HD103UJ_S13PJDWS665103 -> ../../sdb

# Attach disk to VM
/sbin/qm set <vm_id> -virtio2 /dev/disk/by-id/ata-SAMSUNG_HD103UJ_S13PJDWS665103

# NB: Disable backup of disk if used as file storage for example
```
