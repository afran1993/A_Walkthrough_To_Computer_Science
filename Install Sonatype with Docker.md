# Guida completa: Installare e usare Nexus IQ Server su Rocky Linux via Docker

Questa guida è pensata per chi parte da zero e vuole installare Nexus IQ Server su Rocky Linux, usando Docker e Docker Compose, e accedervi anche da un PC Windows remoto.

---

## 1. Prerequisiti

- VMWare (https://dn710001.ca.archive.org/0/items/vmwareworkstationarchive/17.x/VMware-workstation-full-17.6.4-24832109.exe)
- Rocky Linux con accesso root o sudo (https://download.rockylinux.org/pub/rocky/10/isos/x86_64/Rocky-10.1-x86_64-dvd1.iso).
- Docker installato e funzionante (https://docs.docker.com/engine/install/rhel/)
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

## 11. Docker-compose, versione classica

```bash
sudo dnf install -y docker-compose
docker-compose --version
docker-compose up -d
docker-compose down
```

## 12. Avviare Rocky Linux in VMware Player

Se stai usando **VMware Player**, segui questi passaggi per far partire la VM Rocky Linux e prepararla a usare Docker:

### 12.1 Creare una nuova macchina virtuale

1. Assicurati che il sistema operativo sia a 64 bit. Verifica che la virtualizzazione hardware sia abilitata nel BIOS. Apri **VMware Player**.
2. Clicca su **Create a New Virtual Machine**.
3. Seleziona **Installer disc image file (iso)** e scegli il file ISO di Rocky Linux scaricato.
4. Clicca **Next**.
5. Scegli un nome per la VM, ad esempio `RockyLinux-NexusIQ`.
6. Scegli il percorso in cui salvare la VM.
7. Imposta la dimensione del disco virtuale (consigliati almeno **40 GB**), la RAM a 4 GB e 2 core.
8. In "Network Adapter", nella schermata a destra "Network Connection", mettere "Bridged: Connected directly to the physical network"
9. Clicca **Finish**.

---

### 12.2 Configurare la VM prima del primo avvio

- Apri **Settings** della VM appena creata.
- Imposta almeno **2 CPU** e **4 GB di RAM** (Nexus IQ funziona meglio con almeno 4 GB).
- Controlla la scheda di rete: scegli **Bridged** se vuoi che la VM ottenga un IP nella stessa rete del PC host (così puoi accedere via browser da Windows).

---

### 12.3 Avviare la VM e installare Rocky Linux

1. Seleziona la VM e clicca su **Play virtual machine**.
2. La VM partirà dall’ISO di Rocky Linux.
3. Segui la procedura guidata di installazione di Rocky:
   - Seleziona lingua e tastiera
   - Configura rete (assicura che ottenga un IP valido)
   - Imposta password di root
   - Installa il sistema base

4. Al termine, riavvia la VM senza ISO (rimuovi o disattiva l’ISO dal boot) e accedi al sistema Rocky Linux appena installato.

---

### 12.4 Trovare l’IP della VM

Dalla VM, apri un terminale e digita:

```bash
ip a
```

FINE (per ora)







