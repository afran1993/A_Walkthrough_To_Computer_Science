# Guida completa: Installare e usare Nexus IQ Server su Rocky Linux via Docker

Questa guida è pensata per chi parte da zero e vuole installare Nexus IQ Server su Rocky Linux, usando Docker e Docker Compose, e accedervi anche da un PC Windows remoto.

---

## 1. Prerequisiti

- Rocky Linux con accesso root o sudo.
- Docker installato e funzionante.
- Docker Compose installato (può essere il plugin di Docker moderno).
- PC Windows nella stessa rete della VM (per accedere alla UI).

---

## 2. Creare la directory di lavoro

### 1. Collegati alla VM via SSH dal PC Windows:

```bash
ssh nomeutente@IP_VM
# esempio
ssh root@192.168.1.74
```

### 2. Crea la cartella di lavoro:

```bash
mkdir nexusiq
cd nexusiq
```

## 3. Creare il file docker-compose.yml

### 1. Apri il file con nano:

```bash
nano docker-compose.yml
```
### 2. Incolla il contenuto seguente:

```yaml
services:
  nexus-iq-server:
    image: sonatype/nexus-iq-server:latest
    container_name: nexusiq
    restart: always
    ports:
      - 8070:8070  # UI principale
      - 8071:8071  # Admin / operational menu
    tty: true
    volumes:
      - nexusiq-data:/sonatype-work
      - nexusiq-config:/etc/nexus-iq-server
      - nexusiq-logs:/var/log/nexus-iq-server

volumes:
  nexusiq-data:
  nexusiq-config:
  nexusiq-logs:
```

### 3. Salvare e uscire da nano:

Premi CTRL + O → conferma con INVIO.

Premi CTRL + X per uscire.

## 4. Avviare Nexus IQ Server

```bash
docker compose up -d
```

Verifica lo stato dei container:

```bash
docker ps
```
Lo stato unhealthy è normale all’avvio. Non indica un errore reale.

## 5. Verificare i log del container

```bash
docker logs -f nexusiq
```

Dovresti vedere messaggi come “Started Server” e “Migration completed”.

## 6. Verificare se Nexus IQ risponde

Test UI principale (porta 8070)

```bash
curl -I http://localhost:8070
```

* Risposta prevista: HTTP/1.1 303 See Other

Si potrebbe poi andare a vedere sul browser se Sonatype risponde correttamente sulla porta 8070 con interfaccia grafica e sulla 8071 con l'HTML riferito a "Operational Menu"

Test admin console (porta 8071)

```bash
curl -I http://localhost:8071
```

Test della pagina HTML

```bash
curl http://localhost:8070/assets/index.html
```

* Dovresti vedere l’HTML della pagina UI.


## 7. Accesso da un PC Windows (browser)

### 1. Trova l’IP della VM:

```bash
ip a
```

* Esempio: 192.168.1.74

### 2. Apri il browser sul PC Windows:

* UI principale: http://192.168.1.74:8070
* Admin console: http://192.168.1.74:8071

### 3. Credenziali di default:

```makefile
Username: admin
Password: admin123
```

## 8. Configurare il firewall della VM

Se il browser non riesce a connettersi:
```bash
sudo firewall-cmd --add-port=8070/tcp --permanent
sudo firewall-cmd --add-port=8071/tcp --permanent
sudo firewall-cmd --reload
```


## 8. Configurare il firewall della VM

Se il browser non riesce a connettersi:
```bash
sudo firewall-cmd --add-port=8070/tcp --permanent
sudo firewall-cmd --add-port=8071/tcp --permanent
sudo firewall-cmd --reload
```

## 9. Comandi utili Docker

* Fermare il container:
```bash
docker compose down
```
* Riavviare il container:
```bash
docker compose up -d
```
* Eliminare il container:
```bash
docker rm -f nexusiq
```
* Pulire volumi inutilizzati:
```bash
docker volume prune
```
Attenzione: cancellerà tutti i volumi non usati e potrebbe far perdere dati persistenti.

## 10. Note importanti

* Porte:
  * 8070 → UI principale
  * 8071 → Admin / operational menu
* Stato <span style="background-color: #dddddd;">unhealthy</span> è normale all’avvio.
* RAM minima consigliata: 4 GB.
* Da 8071 puoi fare solo operazioni di amministrazione, la UI vera si usa su 8070.
* Le credenziali di default per accedere a Sonatype sono admin / admin123









