# Activation du Wake-On-Lan

### Prérequis

Activation de l'option dans le BIOS:
Activer 'Power On by PCI' ou 'Resume on LAN'et redémarrer

### Sur la machine cliente (exemple: serveur Proxmox)
#### Installer ethtool
```bash
apt install ethtool
```
#### Vérification de l'interface réseau eno1
```bash
ethtool eno1 | grep -i wake
```
```bash
Supports Wake-on: pumbg
Wake-on: d
```

#### Activation du Wake-On-Lan d->g
```bash
vim /etc/network/interfaces
```
```bash
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

#### Ajouter ces 2 lignes à la fin de la section iface vmbr0
```bash
post-up /usr/sbin/ethtool -s eno1 wol g
post-down /usr/sbin/ethtool -s eno1 wol g
```
#### Redémarrer la machine et vérifier qu'on a bien le 'g'
```bash
ethtool eno1 | grep -i wake
```
```bash
Supports Wake-on: pumbg
Wake-on: g
```

#### Recupérer l'adresse MAC de la machine 
```bash
cat /sys/class/net/eno1/address	# pour l'interface en eno1
```

### Reveil à Distance
```bash
apt install wakeonlan
```
#### Eteindre la machine et envoyer le magic packet *tadaaaaa*
```bash
wakeonlan XX:XX:XX:XX:XX:XX
```
