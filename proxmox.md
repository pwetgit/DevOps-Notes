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
https://cloud-images.ubuntu.com/lunar/current/lunar-server-cloudimg-amd64.img

```bash
qm importdisk 8000 lunar-server-cloudimg-amd64.img local-lvm
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