![Portainer](https://icon.icepanel.io/Technology/svg/Portainer.svg)

## 🚀 Portainer Setup

### 📦 Create Portainer Volume
```bash
sudo docker volume create portainer_data
```

### 🏃‍♂️ Run Portainer Container
```bash
sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

### 🌐 External Templates
To use external templates, follow these steps:

1. Login to your Portainer setup.
2. Go to **Settings**.
3. Enable **Use external templates**.
4. Add the URL: `https://raw.githubusercontent.com/Qballjos/portainer_templates/master/Template/template.json`.
5. Go to **App Templates** and hit **Refresh** at the top.

### 🔗 Default Template URL
```plaintext
https://raw.githubusercontent.com/portainer/templates/master/templates-2.0.json
```
