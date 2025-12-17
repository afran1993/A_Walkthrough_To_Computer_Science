# Docker – Comandi principali (CLI)

## Informazioni generali
docker --version                # Mostra la versione di Docker  
docker version                 # Mostra versione client + server  
docker info                    # Info su sistema e daemon Docker  
docker --help                  # Lista comandi disponibili  

## Immagini
docker images                  # Elenca le immagini locali  
docker pull <nome_immagine>    # Scarica immagine da Docker Hub  
docker build -t <nome:tag> .   # Costruisce immagine da Dockerfile  
docker build -t <nome:tag> . --no-cache  # Build senza cache  
docker rmi <nome_immagine>     # Rimuove immagine  
docker image prune             # Rimuove immagini inutilizzate  

## Container
docker ps                      # Lista container in esecuzione  
docker ps -a                   # Lista tutti i container  
docker run <image>             # Crea e avvia container  
docker run -d <image>          # Avvia container in background  
docker run -p 8080:80 <image>  # Mappa porta host -> container  
docker run --name myapp <image> # Avvia container con nome  

docker start <container>       # Avvia container fermo  
docker stop <container>        # Ferma container in esecuzione  
docker restart <container>     # Riavvia container  
docker rm <container>          # Rimuove container fermo  
docker container prune         # Rimuove tutti i container fermati  

## Esecuzione e Log
docker logs -f <container>     # Segui i log del container  
docker exec -it <container> bash  # Apri shell dentro container  
docker inspect <container>     # Dettagli JSON sul container  

## Reti (network)
docker network ls              # Lista reti  
docker network create <name>   # Crea rete  
docker network inspect <net>   # Info sulla rete  
docker network rm <name>       # Rimuovi rete  

## Volumi
docker volume ls               # Lista volumi  
docker volume create <name>    # Crea volume  
docker volume inspect <name>   # Info volume  
docker volume rm <name>        # Rimuovi volume  
docker volume prune            # Pulisce volumi inutilizzati  

## Docker Hub / Registri
docker login -u <user>         # Login su Docker Hub  
docker logout                  # Logout  
docker search <term>           # Cerca immagine su Docker Hub  
docker push <user/image:tag>   # Pubblica immagine  

## Info avanzate
docker stats                   # Statistiche utilizzo container  
docker diff <container>        # Modifiche filesystem container  
docker top <container>         # Processi in esecuzione nel container  
