# I. Init

## 1. Installation de Docker

CHECK ✔️

## 2. Vérifier que Docker est bien là

CHECK ✔️

## 3. sudo c pa bo

🌞 Ajouter votre utilisateur au groupe **``docker``**

```bash
[uliaxe@docker ~]$ sudo usermod -aG docker uliaxe
```

recharger la session

```bash
[uliaxe@docker ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 4. Un premier conteneur en vif

🌞 Lancer un conteneur NGINX

```bash
[uliaxe@docker ~]$ docker run -d -p 9999:80 nginx
```

🌞 Visitons

vérifier que le conteneur est actif avec une commande qui liste les conteneurs en cours de fonctionnement

```bash
[uliaxe@docker ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                   NAMES
09aa4fde0a1c   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:9999->80/tcp, :::9999->80/tcp   thirsty_sanderson
```

afficher les logs du conteneur

```bash
[uliaxe@docker ~]$ docker logs 09
```

afficher toutes les informations relatives au conteneur avec une commande docker inspect

```bash
[uliaxe@docker ~]$ docker inspect 09
```

afficher le port en écoute sur la VM avec un sudo ss -lnpt

```bash
[uliaxe@docker ~]$ sudo ss -lnpt
sudo ss -lnpt
[sudo] password for uliaxe:
State         Recv-Q        Send-Q               Local Address:Port                 Peer Address:Port        Process
LISTEN        0             4096                       0.0.0.0:9999                      0.0.0.0:*            users:(("docker-proxy",pid=4183,fd=4))
LISTEN        0             128                        0.0.0.0:22                        0.0.0.0:*            users:(("sshd",pid=688,fd=3))
LISTEN        0             4096                          [::]:9999                         [::]:*            users:(("docker-proxy",pid=4188,fd=4))
LISTEN        0             128                           [::]:22                           [::]:*            users:(("sshd",pid=688,fd=4))
```

ouvrir le port 9999/tcp (vu dans le ss au dessus normalement) dans le firewall de la VM

```bash
[uliaxe@docker ~]$ sudo firewall-cmd --add-port=9999/tcp --permanent
success
[uliaxe@docker ~]$ sudo firewall-cmd --reload
success
```

🌞 On va ajouter un site Web au conteneur NGINX

créez un dossier nginx

pas n'importe où, c'est ta conf caca, c'est dans ton homedir donc /home/<TON_USER>/nginx/

```bash
[uliaxe@docker ~]$ mkdir /home/uliaxe/nginx
```

dedans, deux fichiers : index.html (un site nul) site_nul.conf (la conf NGINX de notre site nul)

```bash
[uliaxe@docker ~]$ cd /home/uliaxe/nginx
[uliaxe@docker nginx]$ touch index.html site_nul.conf
```

🌞 Visitons

vérifier que le conteneur est actif

```bash
[uliaxe@docker nginx]$ docker ps
[uliaxe@docker nginx]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                               NAMES
2db2d3027f10   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp, 0.0.0.0:9999->8080/tcp, :::9999->8080/tcp   nice_wiles
```

aucun port firewall à ouvrir : on écoute toujours port 9999 sur la machine hôte (la VM)
visiter le site web depuis votre PC

---

## 5. Un deuxième conteneur en vif

🌞 Lance un conteneur Python, avec un shell

```bash
[uliaxe@docker nginx]$ docker run -it python bash
```

🌞 Installe des libs Python

une fois que vous avez lancé le conteneur, et que vous êtes dedans avec bash

installez deux libs, elles ont été choisies complètement au hasard (avec la commande pip install):

aiohttp
aioconsole

```bash
root@fcd7fc15efcf:/# pip install aiohttp
root@fcd7fc15efcf:/# pip install aioconsole
```

tapez la commande python pour ouvrir un interpréteur Python

```bash
root@fcd7fc15efcf:/# python
```

taper la ligne import aiohttp pour vérifier que vous avez bien téléchargé la lib

```bash
root@fcd7fc15efcf:/# python
Python 3.9.7 (default, Sep 16 2021, 13:09:58)
[GCC 11.2.1 20210816 (Red Hat 11.2.1-1)] on linux
Type "help", "copyright", "credits" or "license" for
more information.
>>> import aiohttp
>>>
```

➜ Tant que t'as un shell dans un conteneur, tu peux en profiter pour te balader. Tu peux notamment remarquer :

si tu fais des ls un peu partout, que le conteneur a sa propre arborescence de fichiers

```bash
root@fcd7fc15efcf:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```
