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