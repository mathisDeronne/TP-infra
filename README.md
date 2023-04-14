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