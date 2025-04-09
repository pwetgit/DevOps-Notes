# Installation de Home Assistant sur Proxmox

## Obtenir l'image VM

1. Naviguez vers la page d'installation sur le site de Home Assistant : [Installation Alternative](https://www.home-assistant.io/installation/alternative)
2. Faites un clic droit sur le lien KVM/Proxmox et copiez l'adresse.
3. Dans votre console Proxmox, utilisez la commande suivante pour télécharger le fichier :

```bash
wget <ADDRESS>
```

4. Décompressez l'image compressée :

```bash
unxz </path/to/file.qcow2.xz>
```

---

## Créer la VM

### Général

- Sélectionnez le nom et l'ID de votre VM.
- Cochez l'option "Démarrer au démarrage".

### OS

- Sélectionnez "Ne pas utiliser de média".

### Système

- Changez "machine" en `q35`.
- Changez le BIOS en `OVMF (UEFI)`.
- Sélectionnez le stockage EFI (généralement `local-lvm`).
- Décochez "Pré-enregistrer les clés".

### Disques

- Supprimez le disque SCSI et tout autre disque.

### CPU

- Configurez un minimum de 2 cœurs.

### Mémoire

- Configurez un minimum de 4096 MB.

### Réseau

- Laissez les paramètres par défaut sauf si vous avez des exigences spécifiques (statique, VLAN, etc.).

---

Confirmez et terminez. **Ne démarrez pas encore la VM.**

---

## Ajouter l'image à la VM

1. Dans la console de votre nœud, utilisez la commande suivante pour importer l'image de l'hôte vers la VM :

```bash
qm importdisk <VM ID> </path/to/file.qcow2> <EFI location>
```

**Exemple :**

```bash
qm importdisk 202 /home/user/haos_ova-15.0.qcow2 local-lvm
```

2. Fermez la console du nœud et sélectionnez votre VM Home Assistant.
3. Allez dans l'onglet "Matériel".
4. Sélectionnez le "Disque inutilisé" et cliquez sur le bouton "Modifier".
5. Cochez la case "Discard" si vous utilisez un SSD, puis cliquez sur "Ajouter".
6. Allez dans l'onglet "Options".
7. Sélectionnez "Ordre de démarrage" et cliquez sur "Modifier".
8. Cochez le nouveau disque créé (probablement `scsi0`) et décochez tout le reste.

---

## Finaliser

1. Démarrez la VM.
2. Vérifiez le shell de la VM. Si elle a démarré correctement, un lien pour accéder à l'interface Web devrait s'afficher.
3. Naviguez vers :

```
<VM IP>:8123
```

**Félicitations !** Tout devrait maintenant être opérationnel.