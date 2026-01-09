# Docker – Compact Cheat-Sheet

| Section | Description | Command |
|---------|-------------|---------|
| **Images** | Elenca immagini locali | `docker images` |
| **Images** | Scarica immagine da Docker Hub | `docker pull <nome_immagine>` |
| **Images** | Costruisce immagine da Dockerfile | `docker build -t <nome:tag> .` |
| **Images** | Build senza cache | `docker build -t <nome:tag> . --no-cache` |
| **Images** | Rimuove immagine | `docker rmi <nome_immagine>` |
| **Images** | Rimuove immagini inutilizzate | `docker image prune` |
| **Containers** | Lista container in esecuzione | `docker ps` |
| **Containers** | Lista tutti i container | `docker ps -a` |
| **Containers** | Crea e avvia container | `docker run <image>` |
| **Containers** | Avvia container in background | `docker run -d <image>` |
| **Containers** | Mappa porta host -> container | `docker run -p 8080:80 <image>` |
| **Containers** | Avvia container con nome | `docker run --name myapp <image>` |
| **Containers** | Avvia container fermo | `docker start <container>` |
| **Containers** | Ferma container | `docker stop <container>` |
| **Containers** | Riavvia container | `docker restart <container>` |
| **Containers** | Rimuove container fermo | `docker rm <container>` |
| **Containers** | Rimuove tutti i container fermati | `docker container prune` |
| **Logs & Exec** | Segui i log del container | `docker logs -f <container>` |
| **Logs & Exec** | Apri shell dentro container | `docker exec -it <container> bash` |
| **Logs & Exec** | Dettagli JSON sul container | `docker inspect <container>` |
| **Networks** | Lista reti | `docker network ls` |
| **Networks** | Crea rete | `docker network create <name>` |
| **Networks** | Info sulla rete | `docker network inspect <net>` |
| **Networks** | Rimuovi rete | `docker network rm <name>` |
| **Volumes** | Lista volumi | `docker volume ls` |
| **Volumes** | Crea volume | `docker volume create <name>` |
| **Volumes** | Info volume | `docker volume inspect <name>` |
| **Volumes** | Rimuovi volume | `docker volume rm <name>` |
| **Volumes** | Pulisce volumi inutilizzati | `docker volume prune` |
| **Registry** | Login su Docker Hub | `docker login -u <user>` |
| **Registry** | Logout | `docker logout` |
| **Registry** | Cerca immagine su Docker Hub | `docker search <term>` |
| **Registry** | Pubblica immagine | `docker push <user/image:tag>` |
| **Advanced** | Statistiche utilizzo container | `docker stats` |
| **Advanced** | Modifiche filesystem container | `docker diff <container>` |
| **Advanced** | Processi in esecuzione nel container | `docker top <container>` |
