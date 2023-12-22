# III. Docker compose

🌞 Créez un fichier docker-compose.yml

```yaml
version: "3"

services:
  conteneur_nul:
    image: debian
    entrypoint: sleep 9999
  conteneur_flopesque:
    image: debian
    entrypoint: sleep 9999
```

🌞 Lancez les deux conteneurs avec docker compose

```bash
docker-compose up -d
```

🌞 Vérifier que les deux conteneurs tournent*

```bash
[uliaxe@docker compose_test]$ docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED          STATUS          PORTS     NAMES
091b504ee3c2   debian    "sleep 9999"   55 seconds ago   Up 52 seconds             compose_test-conteneur_flopesque-1
82f873f41a46   debian    "sleep 9999"   55 seconds ago   Up 52 seconds             compose_test-conteneur_nul-1
```

🌞 Pop un shell dans le conteneur conteneur_nul

référez-vous au mémo Docker
effectuez un ping conteneur_flopesque (ouais ouais, avec ce nom là)

```bash
[uliaxe@docker compose_test]$ docker exec -it compose_test-conteneur_nul-1 bash
root@82f873f41a46:/# ping conteneur_flopesque
PING conteneur_flopesque (172.18.0.2) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=1 ttl=64 time=4.47 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=2 ttl=64 time=12.1 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=3 ttl=64 time=0.169 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=4 ttl=64 time=0.178 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=5 ttl=64 time=0.308 ms
^C
--- conteneur_flopesque ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 3996ms
rtt min/avg/max/mdev = 0.169/3.436/12.061/4.615 ms
```

il faudra installer un paquet qui fournit la commande ping pour pouvoir tester

```bash
root@82f873f41a46:/# apt update && apt install iputils
```
