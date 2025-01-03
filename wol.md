# Activation du Wake-On-Lan pour Proxmox

Aller l'option dans le BIOS: 
Activer 'Power On by PCI' ou 'Resume on LAN'et redémarrer

```bash
# Installer ethtool si pas déja présent
apt install ethtool -y

ethtool eno1 | grep -i wake
Supports Wake-on: pumbg
Wake-on: d
```

```bash
vim /etc/network/interfaces

auto lo
iface lo inet loopback

iface eno1 inet manual

auto vmbr0
iface vmbr0 inet static
	address x.x.x.x/24
	gateway x.x.x.x
	bridge-ports eno1
	bridge-stp off
	bridge-fd 0

iface wlp3s0 inet manual
```

Ajouter ces 2 lignes à la fin de la section iface vmbr0 :
```bash
post-up /usr/sbin/ethtool -s eno1 wol g
post-down /usr/sbin/ethtool -s eno1 wol g

# redémarrer la machine et vérifier qu'on a bien le 'g'
ethtool eno1 | grep -i wake
Supports Wake-on: pumbg
Wake-on: g
```

Eteindre la machine et envoyer le magic packet  *tadaaaaa*