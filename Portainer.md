sudo docker volume create portainer_data
sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

https://raw.githubusercontent.com/portainer/templates/master/templates-2.0.json

Login to your portainer setup go to settings
Enable Use external templates
Add the url: https://raw.githubusercontent.com/Qballjos/portainer_templates/master/Template/template.json then go to app templates and hit refresh at the top.
