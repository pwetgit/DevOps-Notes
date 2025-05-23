![Image](https://www.vectorlogo.zone/logos/ansible/ansible-ar21.svg)

## 🧑🏻‍💻 Useful commands
```bash
# Affiche la config ansible
ansible-config view

# Affiche les variables
ansible-inventory all -i mon_inventaire.inv --graph --vars
ansible-inventory all -i mon_inventaire.inv --list

# Affiche les plugins
ansible-doc -l

# Affiche la documentation d’un module
ansible-doc -<nom_module>

# Affiche les information sur l'utilisation du module à l'intérieur d'un playbook
ansible-doc -s -<nom_module> 

# Simulation de l'application du playbook
ansible-playbook --check playbook.yml

# Créer la structure d’un role automatiquement
ansible-galaxy init fcc.outillage

# Recherche des rôles crontab
ansible-galaxy search crontab

# récupérer les facts
ansible localhost -m setup | grep distribution

# Lister les tags presents
ansible-playbook --list-tags playbook.yml

# Crypter les fichiers sensibles
ansible-vault encrypt secure.yml
```

https://www.redhat.com/sysadmin/faster-ansible-playbook-execution
