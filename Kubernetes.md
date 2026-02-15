# ☸️ K3s / Kubernetes – Commandes essentielles

## 🔎 Cluster & Nodes
# Voir les nodes
kubectl get nodes -o wide

# Infos cluster
kubectl cluster-info

# Version client / server
kubectl version --short

# Etat des composants
kubectl get componentstatuses


## 📦 Pods
# Lister les pods
kubectl get pods

# Tous les namespaces
kubectl get pods -A

# Détails d’un pod
kubectl describe pod <pod>

# Logs d’un pod
kubectl logs <pod>

# Logs en temps réel
kubectl logs -f <pod>

# Exec dans un pod
kubectl exec -it <pod> -- /bin/sh

# Supprimer un pod
kubectl delete pod <pod>


## 🧱 Deployments
# Lister
kubectl get deployments

# Créer depuis un fichier
kubectl apply -f deployment.yaml

# Modifier replicas
kubectl scale deployment <name> --replicas=3

# Rollout status
kubectl rollout status deployment <name>

# Historique rollout
kubectl rollout history deployment <name>

# Rollback
kubectl rollout undo deployment <name>


## 🌐 Services
# Lister services
kubectl get svc

# Détails
kubectl describe svc <name>

# Exposer un deployment
kubectl expose deployment <name> --type=NodePort --port=80


## 📂 Namespaces
# Lister
kubectl get ns

# Changer namespace courant
kubectl config set-context --current --namespace=<ns>

# Créer
kubectl create ns <name>


## ⚙️ ConfigMaps & Secrets
# Lister
kubectl get configmaps
kubectl get secrets

# Créer configmap
kubectl create configmap <name> --from-file=config.env

# Créer secret
kubectl create secret generic <name> --from-literal=password=xxx


## 💾 Volumes
# Persistent Volumes
kubectl get pv

# Persistent Volume Claims
kubectl get pvc


## 📜 Debug & Troubleshooting
# Events
kubectl get events --sort-by=.metadata.creationTimestamp

# Tout voir
kubectl get all -A

# YAML complet
kubectl get pod <pod> -o yaml

# Edit live
kubectl edit deployment <name>


## 🚀 K3s spécifique
# Status service
sudo systemctl status k3s

# Logs k3s
sudo journalctl -u k3s -f

# Token node
sudo cat /var/lib/rancher/k3s/server/node-token

# Désinstaller k3s
/usr/local/bin/k3s-uninstall.sh


## 🧹 Nettoyage
# Supprimer via manifest
kubectl delete -f file.yaml

# Supprimer namespace (tout dedans)
kubectl delete ns <name>
