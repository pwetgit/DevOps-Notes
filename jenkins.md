![Image](https://www.vectorlogo.zone/logos/jenkins/jenkins-ar21.svg)

## 🧑🏻‍💻 Useful commands
```bash
# Lancement d'un container Jenkins avec volume "jenkins_home"
docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

# Installation de Maven
apt-get update && apt-get install maven

```