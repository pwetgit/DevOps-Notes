# WINDOWS 11 INSTALLATION 

```bash
# Connect to proxmox console and cd to iso directory
cd /var/lib/vz/template/iso

# Download Windows 11 ISO 
wget https://software.download.prss.microsoft.com/dbazure/Win11_22H2_English_x64v2.iso?t=0d7284c5-ed69-4eaf-a57c-e941efe4e0e7&e=1691495446&h=4521fdbfe85faee7cdaae6cc1c691e2d1cea8f983d011adb9b38b3507aa9b1c4


## VirtIO drivers Installation :
# https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers

# Download VirtIO Drivers
wget https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso

# Add CD/DVD Drive with virto drivers


## Install windows 11 without Microsoft account :
Hit Shift + F10
Type OOBE\BYPASSNRO to disable the Internet connection requirement.


## Qemu-Guest-Agent Installation :
# https://pve.proxmox.com/wiki/Qemu-guest-agent
The guest agent installer is in the directory guest-agent
```


Enable Remote desktop



## Proxmox GPU Passthrough to Windows 10/11
```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt video=vesafb:off video=efifb:off"

update-grub

reboot


vi /etc/modules
# Then place the following into the file:

vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```