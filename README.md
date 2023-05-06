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

- on met à jour les informations du système
```
[oceane@supraflix ~]$ dnf update  
```
- on ajoute le référenciel Jellyfin au système
```
[oceane@supraflix ~]$ dnf install https://repo.jellyfin.org/releases/server/el/8/x86_64/jellyfin-server-10.7.0-1.el8.x86_64.rpm
```
Cette commande va permettre d'installer la dernière version de Jellyfin qui est disponible pour rocky linux. 

- on démarre le service Jellyfin 
```
[oceane@supraflix ~]$ sudo systemctl start jellyfin
[oceane@supraflix ~]$ sudo systemctl enable jellyfin
``` 
- On vérifie que le service Jellyfin fonctionne correctement
```
[oceane@supraflix ~]$  http://adresse_IP_du_serveur:8096
```
(spoileur alerte: ça ne fonctionne pas)
- on remet les paquets à jour pour NGINX
```
[oceane@supraflix ~]$ sudo snf update
```
- on installe NGINX
```
[oceane@supraflix ~]$ sudo dnf install nginx
```
- on démarrer le service NGINX 
```
[oceane@supraflix ~]$ sudo systemctl start nginx
```
- On regarde l'état du service NGINX
```
[oceane@supraflix ~]$ sudo systemctl status nginx

● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
     Active: active (running) since Fri 2023-05-05 07:15:13 UTC; 1 day 4h ago
   Main PID: 15733 (nginx)
      Tasks: 2 (limit: 10577)
     Memory: 2.0M
        CPU: 87ms
     CGroup: /system.slice/nginx.service
             ├─15733 "nginx: master process /usr/sbin/nginx"
             └─15734 "nginx: worker process"

May 05 07:15:13 SupraFlix.stream systemd[1]: Starting The nginx HTTP and reverse proxy server...
May 05 07:15:13 SupraFlix.stream nginx[15731]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
May 05 07:15:13 SupraFlix.stream nginx[15731]: nginx: configuration file /etc/nginx/nginx.conf test is successful
May 05 07:15:13 SupraFlix.stream systemd[1]: Started The nginx HTTP and reverse proxy server.
```
Ensuite, on ouvre le navigateur web et on y rentre l'adresse IP de la machine pour vérifier que cela fonctionne bien. Spoleur alerte: ça fonctionne.

- on démarre automatique le service NGINX
```
[oceane@supraflix ~]$ sudo systemctl enable nginx
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

● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2023-05-02 11:34:27 UTC; 4 days ago
       Docs: man:firewalld(1)
   Main PID: 692 (firewalld)
      Tasks: 4 (limit: 10577)
     Memory: 46.6M
        CPU: 2.198s
     CGroup: /system.slice/firewalld.service
             └─692 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid

May 02 11:34:32 SupraFlix.stream firewalld[692]: WARNING: COMMAND_FAILED: '/usr/sbin/iptables -w10 -t filter -X DOCKER-ISOLATION-STAGE-1' failed: iptables: No chain/target/match by that>
```
- On met à jour les paquets pour Docker
```
[oceane@supraflix ~]$  sudo dnf update
 ```` 

 - On ajoute le référenciel Docker 
 ``` 
 [oceane@supraflix ~]$ sudo dnf config-manager --add-repo=https://download.docker.com/linux/rocky/docker-ce.repo
 ```
- On installe docker
``` 
[oceane@supraflix ~]$ sudo dnf install -y docker-ce docker-ce-cli containerd.io
```
- On démarre le service Docker
```
[oceane@supraflix ~]$ sudo systemctl start docker
[oceane@supraflix ~]$ sudo systemctl enable docker
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

```` 

- On fait une connexion ssh sans rentrer mot de passe sur rocky linux
- On génère une clé ssh 
```
[oceane@supraflix ~]$  ssh-keygen -t rsa
```
- On copie la clé public 
```
[oceane@supraflix ~]$ ssh-copy-id oceane@supraflix@152.228.173.158
```
- On se connecte 
```
PS C:\Users\rouss> ssh oceane@152.228.173.158
Last login: Sat May  6 12:09:36 2023 from 77.131.222.48
[oceane@SupraFlix ~]$
```
(Et sans mot de passe)





