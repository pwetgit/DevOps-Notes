```bash 
# Download Ubuntu (replace with the url of the one you chose from above)
wget https://cloud-images.ubuntu.com/lunar/current/lunar-server-cloudimg-amd64.img

# Install libguestfs-tools on Proxmox server.
apt-get install libguestfs-tools

# Install qemu-guest-agent on Ubuntu image.
virt-customize -a lunar-server-cloudimg-amd64.img --install qemu-guest-agent

# Enable password authentication in the template. Obviously, not recommended for except for testing.
virt-customize -a lunar-server-cloudimg-amd64.img --run-command "sed -i 's/.*PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config"


# Create a new virtual machine
qm create 1000 --memory 2048 --core 2 --name cloud-ubuntu-lunar --net0 virtio,bridge=vmbr0

# Import the downloaded Ubuntu disk to local-lvm storage
qm importdisk 1000 lunar-server-cloudimg-amd64.img local-lvm

# Attach the new disk to the vm as a scsi drive on the scsi controller
qm set 1000 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-1000-disk-0

# Resize the new disk
qm resize 1000 scsi0 +28.5G

# Add cloud init drive
qm set 1000 --ide1 local-lvm:cloudinit

# Make the cloud init drive bootable and restrict BIOS to boot from disk only
qm set 1000 --boot c --bootdisk scsi0

# Add serial console
qm set 1000 --serial0 socket --vga serial0

# DO NOT START YOUR VM
# Now, configure hardware and cloud init, then create a template and clone. If you want to expand your hard drive you can on this base image before creating a template or after you clone a new machine. 
# I prefer to expand the hard drive after I clone a new machine based on need.

# Create template
qm template 1000

# Clone template
qm clone 1000 201 --name ubuntu01 --full

```


```bash
/var/lib/vz/snippets/qemu-guest-agent.yml
#cloud-config
runcmd:
  - apt update
  - apt install -y qemu-guest-agent
  - systemctl start qemu-guest-agent
```
