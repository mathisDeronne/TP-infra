# TP Infra 
# **Mathis DERONNE, Océane ROUSSELOT**
## Voici le rendu du tp infra et un peu de contexte *(si tu en a pas besoin, tu peux te passer de la lecture)* 
Le site que l'on va faire sera un site de streaming/téléchargement de films nommé "SupraFlix". 

**Pourquoi SupraFlix ?** 

Parce que la Supra est une voiture de sport japonaise très connue qu'on aime beaucoup, et Flix en rapport avec NETFLIX (et le nom sonnait bien XD).

Pour notre site on va récuprérer l'API du site nommé "[Zone-Téléchargement](https://www.zone-telechargement.al)".



# Création du site
- Pour commencer, on loue un serveur OVH.

- Ensuite, on s'y connecte depuis notre pc en ssh.

- Pour commencer, on créé un deuxième utilisateur, on lui donne un mot de passe et les droits sudo pour pouvoir travailler a deux en même temps sur le serveur.

- Ensuite, on change le hostname par le nom du site "SupraFlix"
```
[mathis@SupraFlix ~]$ hostnamectl set-hostname SupraFlix.stream
```

Une fois tout cela fait, on peut commencer les installations :

- On commence par installer apache.
```
[mathis@SupraFlix ~]$ sudo dnf install apache
```
- On le lance.
```
[mathis@SupraFlix ~]$ sudo systemctl start httpd
```
- on vérifie qu'il est bien activé.
```
[mathis@SupraFlix ~]$ sudo systemctl is-enabled httpd
[sudo] password for mathis:
enabled
```
- On installe Fail2Ban.
```
[mathis@SupraFlix ~]$ sudo dnf install fail2ban
```
- On le lance et on vérifie qu'il est lancé.
```
[mathis@SupraFlix ~]$ sudo systemctl start fail2ban
[mathis@SupraFlix ~]$ sudo systemctl is-enabled fail2ban
enabled
```
- on install Jellyfin, on mets à jour le système 
```
[oceane@supraflix ~]$ sudo apt-get update && sudo apt-get upgrade
```
- on ajoute le référenciel Jellyfin au système
```
[oceane@supraflix ~]$ wget -O - https://repo.jellyfin.org/debian/jellyfin_team.gpg.key | sudo apt-key add -
sudo sh -c "echo 'deb [arch=$( dpkg --print-architecture )] https://repo.jellyfin.org/debian $( lsb_release -c -s ) main' >> /etc/apt/sources.list.d/jellyfin.list"
```
- on mets à jour le système et on install Jellyfin
```
[oceane@supraflix] sudo apt-get update && sudo apt-get install jellyfin
```
- on démarre le service Jellyfin 
```
[oceane@supraflix ~]$ sudo systemctl start jellyfin
``` 
- On vérifie que le service Jellyfin fonctionne correctement
```
[oceane@supraflix ~]$  http://adresse_IP_du_serveur:8096
```
- installation d'un firewall sur rocky linux
```
[oceane@supraflix ~]$
sudo dnf install firewalld
```
- on démarre le service firewalld
``` 
[oceane@supraflix ~]$ 
sudo systemctl start firewalld
```
- on vérifie l'état du service
```
[oceane@supraflix ~]$
sudo systemctl status firewalld
``` 
- on installe et on démarre Docker sur rocky linux 
``` 
[oceane@supraflix ~]$
sudo systemctl start docker

``` 
- on vérifie que Docker fonctionne 
````
[oceane@supraflix ~]$ 
docker version
Client: Docker Engine  - Community
Version:           23.0.2
API version:        1.42
Go version:         go1.19.7
Git commit:        569dd73
Built:             Mon Mar 27 16:19:13  2023
OS/Arch:           linux/amd64
Context:           default
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.nect: permission denied
``` 

```` 
- on démarre automatiquement le système Docker 
 ```
 [oceane@supraflix ~]$
 sudo systemctl enable docker
 Created symlink /etc/systemd/system/multi-user.target.wants/docker.service -> /usr/lib/system/docker.service.

 ``` 

 - 

 ```
 [oceane@supraflix ~]$ sudo !!
 sudo docker pull jellyfin/jellyfin
 using default tag: latest
 latest: pulling from jellygin/jellyfin
 8740c948ffd4: Pull complete
 e318c113e9bf: Pull complete
 28f984869f48: Pull complete
 9f168e5cffa: Pull complete 
Digest: sha256:97ae358b2f9091c6f745c47d9342cc615c73e3d672d8ac383385548afc836e28
Status: Downloaded newer image for jellyfin/jellyfin: latest
docker.io/jellyfin/jellyfin:latest
``` 

```
- 
``` 
[oceane@supraflix ~]$ sudo !!
sudo docker volume create jellyfin-config 
[sudo] passeword for oceane:
jellyfin-config
```

``` 
- 
``` 
[oceane@supraflix ~]$ sudo !!
sudo docker volume create jellyfin-cache
jellyfin cache
```

