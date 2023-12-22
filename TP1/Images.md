# II. Images

## 1. Images publiques

🌞 Récupérez des images
récupérez :

l'image python officielle en version 3.11 (python:3.11 pour la dernière version)

```bash
[uliaxe@docker ~]$ docker pull python:3.11
```

l'image mysql officielle en version 5.7

```bash
[uliaxe@docker ~]$ docker pull mysql:5.7
```

l'image wordpress officielle en dernière version

```bash
[uliaxe@docker ~]$ docker pull wordpress:latest
```

l'image linuxserver/wikijs en dernière version

```bash
[uliaxe@docker ~]$ docker pull linuxserver/wikijs:latest
```

listez les images que vous avez sur la machine avec une commande docker

```bash
[uliaxe@docker ~]$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
mysql        5.7       5107333e08a8   9 days ago    501MB
python       latest    fc7a60e86bae   2 weeks ago   1.02GB
wordpress    latest    fd2f5a0c6fba   2 weeks ago   739MB
python       3.11      22140cbb3b0c   2 weeks ago   1.01GB
nginx        latest    d453dd892d93   8 weeks ago   187MB
```

🌞 Lancez un conteneur à partir de l'image Python

lancez un terminal bash ou sh

vérifiez que la commande python est installée dans la bonne version

```bash
[uliaxe@docker ~]$ docker run -it python:3.11 bash
root@71664b36d687:/# python --version
Python 3.11.7
```

## 2. Construire une image

🌞 Ecrire un Dockerfile pour une image qui héberge une application Python

```bash
[uliaxe@docker ~]$ mkdir /home/<USER>/python_app_build
[uliaxe@docker ~]$cd /home/<USER>/python_app_build
```

app.py

```python
import emoji

print(emoji.emojize("Cet exemple d'application est vraiment naze :thumbs_down:"))
```

Dockerfile

```dockerfile
FROM debian:latest

RUN apt update -y && apt install -y python3 && apt install -y python3-pip && apt install python3-emoji

COPY app.py /app.py

ENTRYPOINT ["python3", "/app.py"]
```

🌞 Build l'image

```bash
[uliaxe@docker python_app_build]$ docker build -t python-emoji-app .
```

🌞 Lancer l'image

```bash
[uliaxe@docker python_app_build]$ docker run python-emoji-app
Cet exemple d'application est vraiment naze 👎
```
