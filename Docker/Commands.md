# Docker – Comandi principali (CLI)

| Description | Command |
|-------------|---------|
| Mostra la versione di Docker | ```bash
docker --version
``` |
| Mostra versione client + server | `docker version` |
| Info su sistema e daemon Docker | `docker info` |
| Lista comandi disponibili | `docker --help` |
| Elenca le immagini locali | `docker images` |
| Scarica immagine da Docker Hub | `docker pull <nome_immagine>` |
| Costruisce immagine da Dockerfile | `docker build -t <nome:tag> .` |
| Build senza cache | `docker build -t <nome:tag> . --no-cache` |
| Rimuove immagine | `docker rmi <nome_immagine>` |
| Rimuove immagini inutilizzate | `docker image prune` |
| Lista container in esecuzione | `docker ps` |
| Lista tutti i container | `docker ps -a` |
| Crea e avvia container | `docker run <image>` |
| Avvia container in background | `docker run -d <image>` |
| Mappa porta host -> container | `docker run -p 8080:80 <image>` |
| Avvia container con nome | `docker run --name myapp <image>` |
| Avvia container fermo | `docker start <container>` |
| Ferma container in esecuzione | `docker stop <container>` |
| Riavvia container | `docker restart <container>` |
| Rimuove container fermo | `docker rm <container>` |
| Rimuove tutti i container fermati | `docker container prune` |
| Segui i log del container | `docker logs -f <container>` |
| Apri shell dentro container | `docker exec -it <container> bash` |
| Dettagli JSON sul container | `docker inspect <container>` |
| Lista reti | `docker network ls` |
| Crea rete | `docker network create <name>` |
| Info sulla rete | `docker network inspect <net>` |
| Rimuovi rete | `docker network rm <name>` |
| Lista volumi | `docker volume ls` |
| Crea volume | `docker volume create <name>` |
| Info volume | `docker volume inspect <name>` |
| Rimuovi volume | `docker volume rm <name>` |
| Pulisce volumi inutilizzati | `docker volume prune` |
| Login su Docker Hub | `docker login -u <user>` |
| Logout | `docker logout` |
| Cerca immagine su Docker Hub | `docker search <term>` |
| Pubblica immagine | `docker push <user/image:tag>` |
| Statistiche utilizzo container | `docker stats` |
| Modifiche filesystem container | `docker diff <container>` |
| Processi in esecuzione nel container | `docker top <container>` |
