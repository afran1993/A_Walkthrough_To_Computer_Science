
### La Relazione Client-Server

Il concetto di client e server è fondamentale per comprendere il funzionamento delle reti informatiche. Spesso, nei laboratori di Linux, si utilizzano due macchine distinte in cui una assume il ruolo di client e l'altra quello di server.

---

#### 1. Un'Analogia nel Mondo Reale
Per comprendere meglio questa dinamica, si può utilizzare l'esempio di un'ordinazione presso un ristorante (come McDonald's):

* **Il Cliente (Client)**: Rappresenta l'entità che effettua la richiesta (es. "Vorrei un panino al pollo").
* **Il Ristorante (Server)**: Rappresenta l'entità che soddisfa la richiesta fornendo il servizio o il prodotto richiesto.

In sintesi, chi richiede è il **client**, chi fornisce è il **server**. Un server può gestire simultaneamente molteplici richieste di natura diversa (panini, bibite, patatine, ecc.).

---

#### 2. Definizioni Tecniche
Nel contesto informatico, entrambi i soggetti sono computer dotati di un sistema operativo, ma con ruoli software differenti:

* **Client**: È un computer o un programma (come un browser web o un'app mobile) che si affida all'invio di una richiesta a un altro programma per accedere a un servizio.
* **Server**: È un componente hardware o software che fornisce funzionalità, risorse o dati (come hosting web, gestione di database o condivisione di file) ai client.

La comunicazione tra queste due parti avviene attraverso regole specifiche chiamate **protocolli**, come l'**HTTP** o il **TCP/IP**, su reti locali o su Internet. Questo modello è alla base del *distributed computing*.

---

#### 3. Esempio Pratico: Navigazione Web
L'interazione durante la visita a un sito web segue un ciclo preciso:

1.  **Richiesta del Client**: L'utente apre un browser (Firefox, Chrome) e inserisce un URL. Il browser invia una richiesta HTTP al server che ospita il sito.
2.  **Elaborazione del Server**: Il web server riceve la richiesta, la elabora e recupera i contenuti necessari (testi, immagini, video).
3.  **Risposta del Server**: Il server invia una risposta HTTP contenente i dati richiesti al client.
4.  **Visualizzazione**: Il browser riceve la risposta, interpreta i dati e mostra la pagina web all'utente.



---

#### 4. Riepilogo delle Differenze
| Caratteristica | Client | Server |
| :--- | :--- | :--- |
| **Ruolo** | Richiedente | Fornitore |
| **Esempi Software** | Browser, App Mobile, Terminale | Apache, MySQL, File Server |
| **Iniziativa** | Inizia la comunicazione | Attende e risponde alle richieste |

---

### Configurazione dell'Accesso a Internet per la Macchina Virtuale

Per procedere con lo studio del networking in ambiente Linux, è fondamentale che la macchina virtuale disponga di un accesso alla rete funzionante. Questo permette di comunicare con altri server, scaricare pacchetti e installare software.

---

#### 1. Modifica delle Impostazioni di Rete (VirtualBox)
Per consentire alla macchina virtuale di interfacciarsi correttamente con la rete domestica o aziendale, è necessario modificare il tipo di scheda di rete nel gestore della virtualizzazione.

* **Procedura**:
    1.  Aprire le **Impostazioni** (Settings) della macchina virtuale in VirtualBox.
    2.  Selezionare la sezione **Rete** (Network).
    3.  Nel campo "Connesso a" (Attached to), cambiare l'opzione da NAT a **Scheda con bridge** (Bridged Adapter).
    4.  Lasciare le altre opzioni ai valori predefiniti e cliccare su **OK**.

> **Nota**: Questa modalità permette alla macchina virtuale di ottenere un indirizzo IP direttamente dal router, rendendola un nodo indipendente nella stessa rete del computer host (laptop o desktop).

---

#### 2. Applicazione delle Modifiche e Riavvio
Se dopo la modifica la rete non risulta immediatamente disponibile (ad esempio, se il comando `ping google.com` restituisce un errore di servizio non disponibile), è necessario riavviare il sistema per forzare il rinnovo della configurazione di rete.

* **Comando di riavvio** (eseguito come root):
    ```bash
    reboot
    ```
    *(In alternativa si può utilizzare il comando `init 6`).*

---

#### 3. Verifica della Connettività
Una volta riavviata la macchina, è possibile verificare se l'integrazione è avvenuta correttamente seguendo questi passaggi:

1.  **Controllo dell'Indirizzo IP**:
    Eseguire il comando `ifconfig` (o `ip addr`) per verificare che l'indirizzo IP appartenga ora alla stessa classe di rete della propria infrastruttura locale.
    ```bash
    ifconfig | more
    ```
2.  **Test di connettività esterna**:
    Utilizzare il comando `ping` per verificare la comunicazione con un server esterno:
    ```bash
    ping [www.google.com](https://www.google.com)
    ```
    Se si ricevono risposte (es. "64 bytes from..."), la macchina è correttamente online.



---

#### 4. Conclusione
Con la configurazione in modalità **Bridged**, la macchina virtuale è ora in grado di:
* Navigare in internet.
* Scaricare e aggiornare pacchetti software.
* Essere raggiunta da altri dispositivi nella rete locale.

---

### Concetti Fondamentali di Networking in Linux

Sebbene la gestione avanzata di switch e router sia un compito dedicato agli specialisti di rete, un amministratore di sistema o un ingegnere Linux deve possedere una solida comprensione dei componenti di base del networking. Queste competenze sono essenziali per configurare correttamente una macchina e garantirne la connettività online.

---

#### 1. Componenti Chiave della Configurazione
Per rendere operativo un sistema Linux in rete, è necessario conoscere e configurare i seguenti elementi:

* **Indirizzo IP (Internet Protocol)**: È l'identificativo numerico univoco assegnato a una macchina in una rete (es. `192.168.1.234`). Il range degli indirizzi varia in base all'organizzazione e alla struttura della rete aziendale.
* **Subnet Mask (Maschera di Sottorete)**: È un parametro associato all'indirizzo IP che definisce la dimensione della rete e permette di distinguere tra la porzione di indirizzo che identifica la rete e quella che identifica l'host.
* **Gateway (Gateway Predefinito)**: Indica al computer il percorso da seguire per instradare il traffico verso l'esterno o riceverlo dall'esterno. In un ambiente domestico, l'indirizzo IP del modem funge solitamente da gateway.

---

#### 2. Modalità di Assegnazione: Statico vs DHCP
Esistono due modi principali per assegnare un indirizzo IP a un'interfaccia di rete:

| Caratteristica | IP Statico | DHCP |
| :--- | :--- | :--- |
| **Definizione** | Assegnato manualmente dall'amministratore. | Assegnato automaticamente da un server. |
| **Persistenza** | L'indirizzo rimane invariato dopo il riavvio. | L'indirizzo può cambiare a ogni riavvio. |
| **Uso Comune** | Server e infrastrutture aziendali critiche. | Dispositivi client e reti domestiche. |

> **Nota**: Nella maggior parte degli ambienti aziendali, i server Linux vengono configurati con un **IP statico** per garantire che i servizi ospitati siano sempre raggiungibili allo stesso indirizzo.

---

#### 3. Interfaccia di Rete e Indirizzo MAC
L'**interfaccia** (o scheda di rete/NIC) è il componente hardware che permette la connessione fisica (es. tramite cavo CAT-5).

* **Indirizzo MAC (Media Access Control)**: Ogni interfaccia dispone di un indirizzo MAC fisico permanente. A differenza dell'indirizzo IP, che è logico e può essere modificato, l'indirizzo MAC è impresso sulla scheda dal produttore e non cambia mai.



---

### File di Configurazione e Comandi di Rete in Linux

Per configurare correttamente una macchina Linux e consentirle di comunicare all'interno di una rete, è fondamentale conoscere i file di configurazione principali e i comandi di gestione. Questi strumenti permettono di assegnare indirizzi IP, risolvere nomi host e monitorare il traffico.

---

#### 1. File di Configurazione Principali
Quasi tutti i file sotto elencati si trovano nella directory `/etc` e sono comuni a diverse distribuzioni Linux.

* **`/etc/nsswitch.conf`**: Questo file indica al sistema l'ordine di priorità per la risoluzione dei nomi (ad esempio, se consultare prima i file locali o il servizio DNS). La riga `hosts: files dns` suggerisce di cercare prima in `/etc/hosts` e poi di interrogare il DNS.
* **`/etc/hosts`**: Funziona come una versione locale e semplificata del DNS. Permette di mappare manualmente indirizzi IP a nomi host (es: `192.168.1.14 MyFirstLinuxOS`).
* **`/etc/resolv.conf`**: Specifica gli indirizzi IP dei server DNS che il sistema deve interrogare per tradurre i nomi di dominio (come www.google.com) in indirizzi IP.
* **`/etc/sysconfig/network`**: Utilizzato principalmente per definire il nome host globale del sistema e altri parametri di rete generali.
* **Script di Interfaccia (`/etc/sysconfig/network-scripts/ifcfg-<nome_interfaccia>`)**: È il file dove si definiscono i dettagli specifici di ogni scheda di rete. Qui si specifica se l'IP deve essere ottenuto tramite **DHCP** o se deve essere **statico**, definendo in tal caso IP, Subnet Mask e Gateway.

---

#### 2. Comandi di Rete Essenziali
Oltre ai file di testo, esistono comandi specifici per interagire con lo stack di rete:

| Comando | Descrizione |
| :--- | :--- |
| **`ifconfig`** | Visualizza le interfacce di rete attive, i relativi indirizzi IP e l'indirizzo MAC. |
| **`ping`** | Verifica la connettività verso un altro host (es: `ping www.google.com`). |
| **`ifup` / `ifdown`** | Attiva o disattiva un'interfaccia di rete specifica. |
| **`netstat -rnv`** | Mostra la tabella di routing, indicando attraverso quale gateway fluisce il traffico. |
| **`tcpdump`** | Strumento di analisi (sniffing) che traccia ogni pacchetto in entrata e in uscita dall'interfaccia (es: `tcpdump -i enp0s3`). |

---

#### 3. Esempio Pratico: Risoluzione Host Locale
Se si aggiunge una riga al file `/etc/hosts` associando l'IP `192.168.1.14` al nome `MyFirstLinuxOS`, il comando `ping MyFirstLinuxOS` risponderà direttamente all'indirizzo IP specificato, senza interrogare server esterni. Questo è estremamente utile per configurazioni in reti locali o ambienti di test.

> **Consiglio per il System Administrator**: La memorizzazione dei percorsi di questi file è spesso oggetto di domande durante i colloqui tecnici per ruoli legati al mondo Linux.

---

### Informazioni sulle Schede di Rete (NIC)

La **NIC** (Network Interface Card), comunemente nota come scheda di rete, è il componente hardware che permette a un computer (desktop, laptop o server) di connettersi a una rete. Fisicamente, si identifica con la porta situata sul retro o sul lato del dispositivo dove viene inserito il cavo Ethernet (solitamente di categoria 5 o 6).

---

#### 1. Identificazione delle Interfacce
In ambiente Linux, per gestire correttamente la connettività, è necessario identificare i nomi assegnati dal sistema alle diverse interfacce. Il comando principale per questa operazione è:

```bash
ifconfig
```

Eseguendo questo comando, si noteranno solitamente tre tipi di interfacce:

1.  **lo (loopback)**: Un'interfaccia virtuale utilizzata dal computer per comunicare con se stesso. È fondamentale per scopi diagnostici e per far comunicare servizi che girano sulla stessa macchina locale.
2.  **virbr0 (virtual bridge)**: Spesso presente in ambienti virtualizzati, viene utilizzata per il NAT (*Network Address Translation*), permettendo alle macchine virtuali di interfacciarsi con la rete esterna.
3.  **Interfaccia Principale**: È la scheda fisica (o virtualizzata tramite bridge) usata per comunicare con il mondo esterno. Può essere nominata in vari modi: `enp0s3` (tipico di VirtualBox) o `eth0`, `eth1`, ecc. (standard su molti server).

---

#### 2. Analisi Avanzata con `ethtool`
Per ottenere dettagli tecnici profondi su una specifica scheda di rete, si utilizza lo strumento **`ethtool`**. Per eseguire questo comando sono necessari i privilegi di **root**.

* **Sintassi**: `ethtool [nome_interfaccia]`
* **Esempio**: `ethtool enp0s3`



---

#### 3. Parametri Chiave per l'Amministratore
Dall'output di `ethtool` è possibile estrarre informazioni critiche, spesso richieste dagli amministratori di rete per configurare correttamente le porte degli switch:

* **Supported link modes**: Indica le velocità supportate dalla scheda (es. 10Mb, 100Mb o 1000Mb/1Gb). I sistemi più moderni mostrano anche 10.000Mb (10Gb).
* **Speed**: La velocità di connessione attualmente attiva.
* **Duplex**: Indica se la comunicazione è **Full** (trasmissione e ricezione simultanea) o **Half**.
* **Link detected**: Parametro fondamentale per il troubleshooting. Se il valore è `yes`, la connessione fisica è attiva; se è `no`, indica un problema hardware, un cavo scollegato o una scheda danneggiata.

---

### Configurazione del NIC Bonding in Linux

Il **NIC Bonding** (o network bonding) è un aspetto critico della configurazione di rete in Linux, spesso oggetto di approfondimento durante i colloqui tecnici. Consiste nell'aggregazione di più interfacce di rete fisiche in un'unica interfaccia logica (chiamata `bond`).

---

#### 1. Obiettivi del Bonding
L'unione di due o più schede di rete (NIC) persegue due scopi principali:
* **Alta Disponibilità (Ridondanza)**: Se una porta fisica si guasta, il traffico continua a fluire attraverso l'altra senza interruzioni.
* **Aggregazione del Link (Throughput)**: Combinando, ad esempio, due interfacce da 1 Gbps, è possibile ottenere una larghezza di banda teorica di 2 Gbps.



---

#### 2. Procedura di Configurazione (RHEL/CentOS)

Prima di iniziare, è consigliabile eseguire uno **snapshot** della macchina virtuale per poter ripristinare lo stato precedente in caso di errore.

##### A. Preparazione Hardware Virtuale
1.  Spegnere la VM.
2.  Su VirtualBox, andare in **Impostazioni > Rete**.
3.  Abilitare una seconda scheda di rete (**Adattatore 2**) e impostarla come "Scheda con bridge".

##### B. Verifica del Driver
Assicurarsi che il driver di bonding sia caricato nel kernel:
```bash
modinfo bonding
```

Se l'output mostra "Ethernet Channel Bonding Driver", il sistema è pronto.

##### C. Creazione dell'interfaccia Bond
Creare il file `/etc/sysconfig/network-scripts/ifcfg-bond0` definendo l'indirizzo IP statico e le opzioni di aggregazione:

```ini
TYPE=Bond
NAME=bond0
DEVICE=bond0
BONDING_MASTER=yes
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.1.80
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
BONDING_OPTS="mode=5 miimon=100"
```

* **Mode 5 (Adaptive Transmit Load Balancing)**: Bilancia il traffico in uscita in base al carico.
* **miimon=100**: Verifica lo stato del link ogni 100 millisecondi per rilevare eventuali guasti.

##### D. Configurazione delle interfacce Slave
È necessario modificare i file delle interfacce fisiche (es. `enp0s3` e `enp0s8`) affinché puntino al master `bond0`. Tutti i contenuti precedenti devono essere sostituiti:

```ini
TYPE=Ethernet
BOOTPROTO=none
DEVICE=enp0s3
ONBOOT=yes
HWADDR=XX:XX:XX:XX:XX:XX  # Inserire il MAC address specifico dell'interfaccia
MASTER=bond0
SLAVE=yes
```

*(L'operazione va ripetuta per la seconda interfaccia, assicurandosi di aggiornare i campi `DEVICE` e `HWADDR` con i valori corretti rilevati tramite il comando `ifconfig`).*

##### E. Attivazione e Verifica
Dopo aver configurato i tre file (`bond0` e le due slave), si procede al riavvio del servizio di rete:

```bash
systemctl restart network
```

Per confermare che il bonding sia operativo e visualizzare quale slave sia attivo, si può consultare il file di stato dinamico del kernel:

```bash
cat /proc/net/bonding/bond0
```

---

### Introduzione a NetworkManager

NetworkManager è il servizio predefinito in **Red Hat Enterprise Linux (RHEL) 8** e versioni successive per la gestione e la configurazione della rete. Questo strumento sostituisce le vecchie utilità (servizio `network`), semplificando notevolmente l'amministrazione del sistema poiché non è più necessario ricordare complessi file di configurazione o numerosi comandi manuali.

---

#### 1. Metodi di Configurazione Supportati
NetworkManager offre diversi strumenti per gestire le interfacce:

* **nmcli**: Interfaccia a riga di comando (*Command Line Interface*). Ideale per server senza ambiente grafico o per l'automazione tramite script.
* **nmtui**: Interfaccia testuale (*Text User Interface*). Consente di configurare la rete tramite menu interattivi direttamente nel terminale.
* **nm-connection-editor**: Strumento grafico completo (GUI) accessibile solo da console o desktop.
* **Impostazioni GNOME**: Pannello di controllo grafico integrato nel desktop environment.



---

#### 2. Configurazione del Teaming (Network Teaming)
Il **Teaming** è il successore del tradizionale *Bonding*. Entrambi servono a combinare più schede di rete fisiche in un'unica interfaccia logica per ottenere ridondanza e bilanciamento del carico.

##### Procedura con `nmtui`:
1.  Eseguire il comando `nmtui`.
2.  Eliminare o disattivare le connessioni esistenti sulle interfacce fisiche (es. `enp0s3`, `enp0s8`).
3.  Selezionare **Modifica una connessione** > **Aggiungi**.
4.  Scegliere il tipo **Team**.
5.  Assegnare un nome (es. `team1`) e aggiungere le interfacce fisiche come "Slave".
6.  Configurare l'IPv4 come **Automatico (DHCP)** o **Manuale** se è richiesto un IP statico.

---

#### 3. Gestione tramite `nmcli` (Esempi Pratici)
Per gli amministratori che preferiscono la riga di comando, `nmcli` permette di operare rapidamente.

**Visualizzare lo stato delle connessioni:**
```bash
nmcli connection show
```

**Assegnare un indirizzo IP statico:**
```bash
# Modifica dell'IP e del Gateway
nmcli con mod enp0s3 ipv4.addresses 10.253.1.211/24
nmcli con mod enp0s3 ipv4.gateway 10.253.1.1
# Impostazione del DNS e del metodo manuale
nmcli con mod enp0s3 ipv4.dns "8.8.8.8"
nmcli con mod enp0s3 ipv4.method manual
# Riavvio dell'interfaccia per applicare le modifiche
nmcli con down enp0s3 && nmcli con up enp0s3
```

**Aggiungere un IP secondario: È possibile aggiungere un secondo indirizzo IP sulla stessa interfaccia senza sovrascrivere quello esistente:**
```bash
nmcli con mod enp0s3 +ipv4.addresses 192.168.10.50/24
nmcli con reload
# In alcuni casi è richiesto un riavvio del sistema o dell'interfaccia
```

#### 4. Verifica Finale

Per confermare che gli indirizzi siano stati assegnati correttamente, si utilizza:
```bash
ip address show enp0s3
```

---

### Download di File e Applicazioni in Linux tramite CLI

In ambiente Windows, il download di un file o di un'applicazione avviene solitamente tramite browser: si inserisce l'URL, si accede alla pagina e si clicca sul link di download. In ambiente Linux, specialmente quando si opera su server senza interfaccia grafica (GUI), è necessario utilizzare strumenti da riga di comando.

---

#### 1. L'utilità `wget`
Il comando principale per scaricare contenuti dal Web è **`wget`** (acronimo di *World Wide Web get*). Questo strumento permette di prelevare file direttamente da Internet specificando l'URL esatto.

**Sintassi del comando:**
```bash
wget [URL_completo_del_file]
```

Esempio: se il file è un archivio con estensione .tar.gz, .zip o un eseguibile, è necessario indicare l'indirizzo completo nel comando per avviarne il trasferimento.

Esempio: se il file è un archivio con estensione `.tar.gz`, `.zip` o un eseguibile, è necessario indicare l'indirizzo completo nel comando per avviarne il trasferimento.

---

#### 2. Perché usare `wget` invece di `yum`?
In sistemi basati su Red Hat/CentOS, si utilizza solitamente il comando `yum install [pacchetto]` per installare software. `yum` interroga dei repository (archivi centralizzati) predefiniti. Tuttavia, `wget` diventa indispensabile quando:
* Il pacchetto desiderato non è presente nei repository ufficiali.
* Il software è ospitato su un server di terze parti non censito nel sistema.

In questi casi, si scarica il file sorgente o il pacchetto tramite `wget` e si procede successivamente con l'installazione manuale.



---

#### 3. Esempio Pratico: Download di un pacchetto
Si ipotizzi di voler scaricare i sorgenti di PuTTY su una macchina Linux.

1.  **Verifica con `yum`**:
    Eseguendo `yum install putty`, il sistema potrebbe restituire l'errore: *"No package putty available"*.
2.  **Ricerca del link**:
    Tramite un browser su un'altra postazione, si individua l'URL diretto del file (es. cliccando con il tasto destro sul link di download e selezionando *"Copia indirizzo link"*).
3.  **Download via CLI**:
    Si torna sul terminale Linux (es. tramite sessione PuTTY) e si utilizza `wget`:
    ```bash
    wget [https://the.earth.li/~sgtatham/putty/latest/putty-0.XX.tar.gz](https://the.earth.li/~sgtatham/putty/latest/putty-0.XX.tar.gz)
    ```

---

#### 4. Requisiti di Rete
Perché `wget` funzioni, la macchina Linux deve avere accesso a Internet.
* **Configurazione di rete**: Se si utilizza una macchina virtuale, è consigliabile impostare la scheda di rete in modalità **Bridged Adapter**. Questo permette alla VM di uscire dalla rete locale e raggiungere il Web esterno, a differenza della modalità *Host-only* che limita la comunicazione tra host e guest.
* **Verifica IP**: È possibile controllare l'indirizzo IP assegnato con il comando `ip addr` o `ifconfig`.



#### 5. Destinazione del Download
Per impostazione predefinita, il file viene scaricato nella directory corrente (*Current Working Directory*). È possibile verificare la posizione attuale con il comando:
```bash
pwd
```
Se si opera come utente root, il file sarà disponibile nella cartella /root.

---

### Risoluzione dei Problemi di Rete: I Comandi `curl` e `ping`

I comandi `curl` e `ping` sono strumenti fondamentali per l'amministrazione di sistema, essenziali per diagnosticare e risolvere problemi di connettività di rete. Quando un server o un sito web risultano irraggiungibili, questi comandi permettono di verificare dove si interrompe la comunicazione.

---

#### 1. Il comando `ping`
Il comando **`ping`** viene utilizzato per verificare se un host (server o workstation) è attivo e raggiungibile sulla rete.

* **Funzionamento**: Invia pacchetti ICMP verso un indirizzo IP o un nome host (es. `google.com`). Se l'host risponde, significa che il server è acceso e la connessione di rete di base è operativa.
* **Differenza Linux/Windows**: In ambiente Linux, il `ping` continua all'infinito finché non viene interrotto manualmente (solitamente con `Ctrl+C`). In Windows, vengono inviati di default solo quattro pacchetti (per il ping continuo in Windows si usa l'opzione `-t`).

**Esempio di utilizzo:**
```bash
ping [www.google.com](https://www.google.com)
# oppure tramite IP
ping 216.58.217.68
```

#### 2. Il comando `curl`
Mentre `ping` verifica la raggiungibilità del server, **`curl`** (Client URL) verifica la raggiungibilità di una specifica risorsa o servizio web (HTTP/HTTPS).

* **Utilità**: È possibile che un server risponda al `ping` ma che il sito web sia comunque irraggiungibile perché il servizio web (come Apache o Nginx) è fermo. `curl` permette di testare direttamente l'URL.
* **Output**: Non avendo una GUI (interfaccia grafica), `curl` restituisce il codice sorgente HTML della pagina. Se viene visualizzato il codice, significa che il servizio web è attivo e funzionante.

**Esempio di verifica sito:**
```bash
curl [http://www.google.com](http://www.google.com)
```

#### 3. Download di file con `curl`
Oltre alla diagnostica, `curl` può essere utilizzato per scaricare file da Internet, rappresentando un'ottima alternativa a `wget` qualora quest'ultimo non fosse installato sul sistema.

* **Opzione `-O` (O maiuscola)**: Permette di scaricare il file mantenendo il nome originale definito sul server. Il file verrà salvato nella directory di lavoro corrente (verificabile con il comando `pwd`).
* **Verifica**: Una volta completato il download, è possibile confermare la presenza del file e controllarne i dettagli (come data e ora di creazione) utilizzando il comando `ls -l`.

**Esempio di download:**
```bash
# Download dell'archivio sorgente di PuTTY
curl -O [https://the.earth.li/~sgtatham/putty/latest/putty-0.XX.tar.gz](https://the.earth.li/~sgtatham/putty/latest/putty-0.XX.tar.gz)
```

#### 4. Riepilogo Diagnostico per interviste tecniche
In ambito lavorativo o durante un colloquio tecnico, alla domanda su come diagnosticare l'impossibilità di comunicazione tra due server (es. Server A non raggiunge Server B), è opportuno citare questa progressione logica:

1.  **Ping**: Per verificare la connettività di base (Layer 3) e la risposta dell'host tramite indirizzo IP o hostname.
2.  **Curl**: Per verificare se il servizio applicativo specifico (Layer 7) è attivo e se il server web restituisce correttamente il codice HTML o i dati richiesti.
3.  **nslookup / dig**: Per assicurarsi che la risoluzione dei nomi (DNS) funzioni correttamente e che l'hostname venga tradotto nell'indirizzo IP corretto.

---

### Analisi delle Connessioni di Rete in Linux: Il Comando `ss`

Il comando **`ss`** (Socket Statistics) è una utility a riga di comando utilizzata per visualizzare informazioni statistiche sui socket. È lo strumento moderno che ha sostituito il vecchio comando `netstat`, offrendo maggiore velocità e dettagli più approfonditi sulle connessioni di rete e le performance del dispositivo.

---

#### 1. Che cos'è un Socket?
Un **socket** può essere immaginato come una linea telefonica che permette a due computer di connettersi e comunicare attraverso una rete, abilitando l'invio e la ricezione di dati. Il comando `ss` monitora questi "punti di accesso" mostrando dettagli come indirizzi IP di origine e destinazione, porte e stato della connessione (es. *established*, *listening*).



#### 2. Tipologie di Socket Monitorati
Il comando `ss` analizza principalmente tre tipi di protocolli:

* **TCP (Transmission Control Protocol)**: Un protocollo orientato alla connessione che garantisce il recapito dei dati. Utilizzato per navigazione web, email e trasferimento file.
* **UDP (User Datagram Protocol)**: Un protocollo veloce ma meno affidabile, poiché non verifica se i dati arrivano a destinazione. Ideale per lo streaming video e il gaming.
* **UNIX Socket**: Utilizzati per la comunicazione tra processi residenti sulla **stessa macchina**, senza passare per la rete internet (es. database che parlano con server web locali).

---

#### 3. Analisi dell'Output del Comando `ss`
Eseguendo semplicemente il comando `ss` nel terminale, l'output viene organizzato in colonne:

| Colonna | Descrizione |
| :--- | :--- |
| **Netid** | Il protocollo utilizzato (TCP, UDP, o `u_str` per i socket UNIX). |
| **State** | Lo stato della connessione (es. `ESTAB` per connessioni attive). |
| **Recv-Q / Send-Q** | Quantità di dati in coda per la ricezione o l'invio. |
| **Local Address:Port** | L'indirizzo IP e la porta della macchina locale. |
| **Peer Address:Port** | L'indirizzo IP e la porta del dispositivo remoto. |
| **Process** | Il processo specifico che sta utilizzando la connessione. |

---

#### 4. Opzioni Comuni e Filtri
Per gestire l'enorme quantità di dati, è possibile filtrare l'output di `ss` utilizzando diverse opzioni:

* **`ss -t`**: Visualizza solo le connessioni **TCP**.
* **`ss -u`**: Visualizza solo le connessioni **UDP**.
* **`ss -x`**: Visualizza solo i socket **UNIX**.
* **`ss -l`**: Mostra i socket in stato di **ascolto** (*listening*), ovvero servizi pronti ad accettare connessioni in entrata.
* **`ss -n`**: Mostra gli indirizzi e le porte in formato **numerico** invece di tentare di risolvere i nomi dei servizi (es. mostra `22` invece di `ssh`).

**Esempio di comando combinato:**
```bash
ss -tln
```
Questo comando mostra tutte le connessioni TCP (`-t`), in ascolto (`-l`), espresse in formato numerico (`-n`).

---

#### 5. Evoluzione Storica
* **2010**: Introdotto nel pacchetto `iproute2` per sostituire `netstat`.
* **2016**: Con il kernel Linux 4.x, diventa ancora più integrato nella gestione di nuovi protocolli.
* **Oggi**: È lo standard per i sistemisti per il troubleshooting delle performance e della sicurezza di rete.

---

### Introduzione al Protocollo FTP (File Transfer Protocol)

Il protocollo **FTP** è uno standard di rete utilizzato per il trasferimento di file tra un client e un server su una rete informatica. Si basa su un'architettura **client-server** e utilizza connessioni separate per il controllo e per i dati.

In termini semplici, l'FTP è il sistema di regole che permette di spostare un file dal Server A al Server B, definendo parametri come le porte di comunicazione e i metodi di crittografia.

---

#### 1. Requisiti e Concetti Base
* **Porta di default**: L'FTP utilizza tipicamente la **porta 21** per il controllo (mentre SSH usa la 22 e il DNS la 53).
* **Modello di comunicazione**: Il mittente agisce come **client**, mentre il destinatario agisce come **server**. Quest'ultimo deve avere un servizio (demone) attivo in ascolto per accettare il trasferimento.



---

#### 2. Configurazione del Server (Lato Destinatario)
Per configurare un server Linux (es. CentOS) affinché riceva file tramite FTP, è necessario installare e configurare il pacchetto `vsftpd` (*Very Secure FTP Daemon*).

**Passaggi principali:**
1.  **Installazione**: Verificare la connettività internet ed eseguire `yum install vsftpd`.
2.  **Configurazione**: Modificare il file `/etc/vsftpd/vsftpd.conf` (si consiglia sempre di crearne una copia di backup `.orig`).
    * Disabilitare l'accesso anonimo: `anonymous_enable=NO`.
    * Abilitare le modalità ASCII per upload/download.
    * Impostare il tempo locale: `use_localtime=YES`.
3.  **Gestione Servizio**:
    * Avviare il servizio: `systemctl start vsftpd`.
    * Abilitarlo al boot: `systemctl enable vsftpd`.
4.  **Firewall**: Fermare il firewall (`systemctl stop firewalld`) o aggiungere una regola specifica per permettere il traffico sulla porta 21.
5.  **Utente**: Assicurarsi che esista un account utente (es. `iafzal`) per l'autenticazione.

---

#### 3. Configurazione del Client (Lato Mittente)
Sul computer che invierà il file, è necessario installare il client FTP.

**Procedura di invio:**
1.  **Installazione client**: Eseguire `yum install ftp`.
2.  **Preparazione file**: Creare un file di test (es. `kruger`) e popolarlo con dei dati.
3.  **Connessione**: Avviare la sessione verso l'IP del server:
    ```bash
    ftp [INDIRIZZO_IP_SERVER]
    ```
4.  **Autenticazione**: Inserire nome utente e password quando richiesto.

---

#### 4. Trasferimento Pratico del File
Una volta stabilita la connessione interattiva FTP, seguire questi comandi:

* **`bin`**: Imposta la modalità di trasferimento binaria (raccomandata per evitare corruzione dei dati).
* **`hash`**: Attiva la visualizzazione del progresso tramite il carattere `#` (molto utile per file di grandi dimensioni).
* **`put [nome_file]`**: Invia il file dal client al server.
* **`bye`**: Chiude la connessione FTP.



---

#### 5. Verifica Finale
Per confermare il successo dell'operazione, è sufficiente collegarsi al server destinatario e controllare la directory home dell'utente con il comando `ls -l`. Il file apparirà con la dimensione corretta e i permessi associati.

---

### Introduzione a SCP (Secure Copy Protocol)

Il protocollo **SCP** (Secure Copy Protocol) viene utilizzato per il trasferimento sicuro di file tra un host locale e un host remoto. Sebbene sia simile all'FTP, SCP si distingue per l'integrazione di sistemi di sicurezza e autenticazione avanzati, rendendolo il metodo preferito in molti contesti amministrativi.

---

#### 1. Caratteristiche e Funzionamento
A differenza di altri protocolli che richiedono porte dedicate, SCP si appoggia interamente al protocollo **SSH** (Secure Shell).

* **Sicurezza**: Ogni trasferimento è crittografato, garantendo l'integrità e la riservatezza dei dati.
* **Porta di default**: Utilizza la **porta 22**, la stessa utilizzata per le connessioni SSH standard.
* **Requisiti**: Affinché il trasferimento avvenga, il demone SSH (*sshd*) deve essere attivo e funzionante sul server di destinazione.



---

#### 2. Sintassi del Comando
Il processo di copia di un file verso un server remoto segue una struttura precisa. Si prenda come esempio il trasferimento di un file denominato `Jack` verso un server con indirizzo IP `192.168.1.58`:

```bash
scp [NOME_FILE] [UTENTE]@[IP_REMOTO]:[PERCORSO_DESTINAZIONE]
```

Esempio pratico:
```bash
scp Jack iafzal@192.168.1.58:/home/iafzal
```

#### 3. Esecuzione del Trasferimento (Lab Pratico)
Per testare il comando, si utilizzano due macchine Linux (Client e Server). Ecco la procedura operativa impersonale:

1.  **Preparazione del file**: Sul client, viene creato un file (es. `Jack`) e popolato con del testo per verificarne il contenuto post-trasferimento.
    * Comando: `echo "Jack is Jerry's uncle" > Jack`
2.  **Identificazione del Server**: Si recupera l'indirizzo IP della macchina di destinazione tramite il comando `ifconfig` o `ip a`.
3.  **Avvio della copia**: Si esegue il comando SCP. Se è la prima volta che viene stabilita una connessione verso quel server, il sistema richiederà di accettare l'impronta digitale (*fingerprint*) della chiave SSH digitando `yes`.
4.  **Autenticazione**: Viene richiesta l'immissione della password dell'utente remoto per autorizzare il trasferimento.

---

#### 4. Verifica della Ricezione
Al termine dell'operazione, il client visualizza una barra di progresso (100%) indicando la dimensione del file inviato e il tempo impiegato. Sul server di destinazione, è possibile confermare l'avvenuto trasferimento posizionandosi nella directory scelta e utilizzando i seguenti comandi:

* **`ls -l`**: Per verificare la presenza del file e la sua dimensione.
* **`cat [NOME_FILE]`**: Per assicurarsi che il contenuto sia integro.

> **Nota**: Il comando può essere invertito per "prelevare" un file da un server remoto verso la macchina locale, scambiando semplicemente l'ordine degli argomenti (origine e destinazione).

---

### Introduzione a rsync (Remote Synchronization)

**rsync** è una potente utility riga di comando per il trasferimento e la sincronizzazione efficiente di file e directory, sia localmente sullo stesso computer, sia tra macchine diverse attraverso una rete.

---

#### 1. Perché utilizzare rsync?
A differenza di strumenti come FTP o SCP, rsync è progettato per la massima efficienza:
* **Sincronizzazione Delta**: rsync confronta le dimensioni dei file e i tempi di modifica. Se un file è già presente nella destinazione, trasferisce solo le parti modificate (i "delta"), riducendo drasticamente il tempo di trasferimento.
* **Velocità**: È molto più rapido dei metodi di copia tradizionali per aggiornamenti di file preesistenti.
* **Utilizzo comune**: Viene ampiamente utilizzato dagli amministratori di sistema per backup automatizzati tramite `crontab`.

---

#### 2. Sicurezza e Protocollo
* **Porta di default**: Utilizza la **porta 22**, la stessa del protocollo **SSH**.
* **Integrazione SSH**: Proprio come SCP, rsync si appoggia a SSH (piggyback) per l'autenticazione e la crittografia. Non è necessario installare un server rsync dedicato sulla macchina remota; è sufficiente che il servizio SSH sia attivo.



---

#### 3. Sintassi di base
La struttura fondamentale del comando è:
```bash
rsync [OPZIONI] [SORGENTE] [DESTINAZIONE]
```

**Opzioni comuni:**
* `-a` (**archive**): La modalità archivio permette di preservare permessi, proprietari, timestamp e collegamenti simbolici. È l'opzione più utilizzata per i backup.
* `-z` (**compress**): Comprime i dati durante il trasferimento per ridurre l'occupazione di banda, utile specialmente su connessioni lente.
* `-v` (**verbose**): Fornisce informazioni dettagliate sui file trasferiti durante l'esecuzione del comando.
* `-h` (**human-readable**): Formatta le dimensioni dei file e le velocità di trasferimento in unità leggibili (come KB, MB o GB).

---

#### 4. Esempi Pratici di Utilizzo

**A. Sincronizzazione locale (file e directory)**
Per sincronizzare un file archivio in una cartella di backup locale:
```bash
rsync -zvh backup.tar /tmp/backups/
```

Per sincronizzare l'intero contenuto di una directory verso una cartella locale:
```bash
rsync -azvh /home/iafzal/ /tmp/backups/
```

**B. Trasferimento verso un server remoto (Push) Inviare un file dal client locale a un server remoto specificando utente e IP:**
```bash
rsync -avz backup.tar iafzal@192.168.1.58:/tmp/backups/
```

**C. Recupero da un server remoto (Pull) Prelevare un file presente sul server remoto per salvarlo in una directory locale:**
```bash
rsync -avzh iafzal@192.168.1.58:/home/iafzal/serverfile /tmp/backups/
```

#### 5. Verifica e Installazione
Qualora l'utility non fosse disponibile nel sistema, è possibile procedere all'installazione tramite il gestore pacchetti dedicato alla propria distribuzione:

* **Sistemi RHEL/CentOS**: `yum install rsync`
* **Sistemi Ubuntu/Debian**: `apt-get install rsync`

Per confermare la corretta installazione del pacchetto su sistemi basati su RPM, è possibile eseguire il comando di interrogazione:

```bash
rpm -qa | grep rsync
```

---

### Aggiornamenti di Sistema e Repository

I repository (o "repos") sono archivi centralizzati che contengono i pacchetti software per il sistema Linux. La gestione di questi pacchetti avviene principalmente tramite due strumenti: **dnf** (o yum) e **rpm**.

---

#### 1. dnf e yum (Gestori di Pacchetti ad alto livello)
Il comando **dnf** (Dandified YUM) è l'evoluzione di **yum**. 

* **Utilizzo**: Si utilizza `yum` su versioni datate di CentOS (7 o precedenti) e `dnf` su versioni recenti (CentOS 8+, Red Hat Enterprise Linux).
* **Funzionamento**: dnf consulta i file di configurazione in `/etc/yum.repos.d/` per trovare l'URL del repository online, scarica il pacchetto e lo installa.
* **Dipendenze**: dnf è uno strumento "intelligente" poiché risolve automaticamente le dipendenze (installa tutti i pacchetti aggiuntivi necessari al funzionamento del software principale).
* **Requisiti**: Richiede una connessione internet (o l'accesso a un repository locale configurato).

---

#### 2. rpm (Red Hat Package Manager)
**rpm** è lo strumento di base per la gestione dei pacchetti nei sistemi Red Hat e derivati.

* **Differenza con dnf**: rpm viene utilizzato per gestire pacchetti già scaricati localmente (file con estensione `.rpm`).
* **Limiti**: A differenza di dnf, rpm non risolve automaticamente le dipendenze; se mancano componenti necessari, il comando restituirà un errore e l'utente dovrà installarli manualmente.
* **Query**: È molto utile per interrogare il database dei pacchetti già installati sul sistema.

---

#### 3. Esempi Pratici e Comandi Comuni

Prima di installare pacchetti, è prassi corretta verificare il proprio **hostname** per evitare modifiche su sistemi errati.

**A. Verificare i pacchetti installati**
Per elencare tutti i pacchetti presenti nel sistema e contarli:
```bash
rpm -qa               # Elenca tutti i pacchetti
rpm -qa | wc -l       # Conta il numero totale di pacchetti (linee di output)
rpm -qa | grep bind   # Cerca se uno specifico pacchetto (es. bind) è installato
```

**B. Installare pacchetti con dnf Il comando gestisce download, risoluzione delle dipendenze, installazione e pulizia dei file temporanei:**
```bash
dnf install bind      # Installa il pacchetto DNS 'bind'
```
**C. Installare e Rimuovere con rpm Per gestire manualmente un file locale o rimuovere un pacchetto:**
```bash
rpm -ihv /percorso/pacchetto.rpm   # Installa (-i), mostra progresso (-h) e dettagli (-v)
rpm -e nome_pacchetto              # Rimuove (erase) un pacchetto dal sistema
```

**D. Rimuovere pacchetti con dnf Alternativa più completa a `rpm -e:`**
```bash
dnf remove bind       # Rimuove il pacchetto e gestisce le relative dipendenze
```

#### Riepilogo differenze

Il confronto tra i due strumenti permette di comprendere quale utilizzare in base allo scenario operativo (presenza di internet, gestione delle dipendenze o necessità di interrogazione).

| Caratteristica | dnf / yum | rpm |
| :--- | :--- | :--- |
| **Livello** | Alto livello (Gestore completo) | Basso livello (Gestore pacchetti) |
| **Download** | Automatico dai repository online | Richiede file locale `.rpm` pre-scaricato |
| **Dipendenze** | Risolte e installate automaticamente | Segnalate ma non risolte automaticamente |
| **Utilizzo principale** | Installazione e aggiornamento software | Query del database e installazioni offline |
| **Connessione** | Necessaria (Internet o Repo locale) | Non necessaria (opera su file locali) |



---

### Gestione degli Aggiornamenti di Sistema e Patching

L'aggiornamento del sistema e la gestione delle patch sono attività critiche per la sicurezza e la stabilità. A seconda delle policy aziendali, queste operazioni possono essere eseguite periodicamente (ad esempio, su base trimestrale).

---

#### 1. Tipologie di Aggiornamento
In ambito Linux, si distinguono due tipi principali di aggiornamento:

* **Aggiornamento di Versione Major (Maggiore)**: Passaggio da una versione principale all'altra (es. da CentOS 7 a 8, o da 8 a 9).
    * **Nota**: Non è possibile eseguire questo passaggio tramite il semplice comando `dnf`. In genere richiede il backup dei dati, la reinstallazione del sistema da zero e il trasferimento dei file, sebbene esistano procedure specifiche più complesse.
* **Aggiornamento di Versione Minor (Minore)**: Passaggio tra revisioni della stessa versione principale (es. da Red Hat 9.1 a 9.2).
    * **Rolling Release**: Sistemi come **CentOS Stream 9** seguono un modello a rilascio continuo. Non presentano versioni puntuali (9.1, 9.2), ma mostrano solo la versione major. Al contrario, Red Hat Enterprise Linux (RHEL) o versioni precedenti di CentOS mantengono la distinzione delle versioni minor.

---

#### 2. Comandi Principali per l'Aggiornamento
Il gestore pacchetti `dnf` offre due opzioni principali, con una differenza fondamentale nella gestione dei pacchetti obsoleti:

| Comando | Descrizione | Gestione Pacchetti Vecchi |
| :--- | :--- | :--- |
| **`dnf update`** | Aggiorna i pacchetti alla versione più recente. | **Preserva** le versioni precedenti (utile per compatibilità). |
| **`dnf upgrade`** | Aggiorna i pacchetti alla versione più recente. | **Elimina** i pacchetti obsoleti sostituiti dai nuovi. |

**Utilizzo del flag `-y`**:
L'aggiunta dell'opzione `-y` (es. `dnf update -y`) istruisce il sistema a rispondere automaticamente "sì" a ogni richiesta di conferma, permettendo un'esecuzione non interattiva.

---

#### 3. Esecuzione e Verifica (Lab Pratico)

**A. Verificare la versione corrente**
Prima di procedere, è fondamentale identificare il sistema operativo in uso per evitare errori:
```bash
cat /etc/redhat-release
# Oppure utilizzare uname
uname -a
```

**B. Controllo della connettività Poiché dnf deve scaricare i pacchetti dai repository online, è necessario assicurarsi che la macchina abbia accesso a Internet:**
```bash
ping google.com
```

**C. Avvio del processo di aggiornamento Eseguendo `dnf update`, il sistema esegue le seguenti fasi:**

1. **Analisi delle dipendenze:** Identifica quali pacchetti aggiuntivi sono necessari (es. come l'installazione di Java per certi software).
2. **Download**: Scarica tutti i pacchetti necessari e le relative dipendenze.
3. **Verifica GPG**: Richiede l'importazione delle chiavi GPG ufficiali per garantire l'autenticità e l'integrità dei pacchetti, evitando l'installazione di software malevolo.
4. **Installazione**: Applica gli aggiornamenti e pulisce i processi temporanei.

---

### Creazione di un Repository Locale da DVD/ISO

La creazione di un repository locale è una procedura essenziale per gestire l'installazione di software in ambienti privi di accesso a Internet o dove non sia presente un server di gestione pacchetti centralizzato (come Red Hat Satellite).

---

#### 1. Preparazione del Sistema
Prima di procedere, è consigliabile eseguire uno **snapshot** della macchina virtuale (se applicabile) per poter ripristinare lo stato del sistema in caso di errori critici.

Sulle versioni più recenti di CentOS/RHEL (8 e successive), è necessario installare l'utility `createrepo_c`:

```bash
dnf install createrepo_c -y
```

*(Nota: su CentOS 7 o precedenti il comando è `createrepo`, solitamente già presente).*

---

#### 2. Acquisizione dei Pacchetti
I pacchetti RPM devono essere trasferiti dal supporto (DVD o ISO) al disco locale per renderli accessibili permanentemente.

1.  **Identificazione del punto di montaggio**: Verificare dove è montato il DVD tramite il comando:
    ```bash
    df -h
    ```
    *In caso di montaggio manuale, utilizzare:* `mount /dev/cdrom /punto/di/mount`.

2.  **Creazione della directory locale**:
    ```bash
    mkdir /localrepo
    ```

3.  **Copia dei file**: Identificare la cartella contenente i pacchetti nel supporto (solitamente sotto `AppStream/Packages`) e copiarne il contenuto. È consigliabile verificare prima lo spazio disponibile con `du -sh`:
    ```bash
    cp /run/media/iafzal/CentOS-Stream-9/AppStream/Packages/* /localrepo/
    ```

---

#### 3. Configurazione del Repository
Per istruire il gestore pacchetti `dnf` a utilizzare esclusivamente la risorsa locale, è necessario configurare un file dedicato. 

> **Importante**: Spostare o eliminare i file esistenti in `/etc/yum.repos.d/` prima di procedere per evitare che il sistema tenti di connettersi ai mirror online.

1.  Creare il nuovo file di configurazione:
    ```bash
    vi /etc/yum.repos.d/local.repo
    ```

2.  Inserire i seguenti parametri (notare il triplo slash in `baseurl` per indicare il percorso locale):
    ```ini
    [CentOS9]
    name=CentOS9 Locale
    baseurl=file:///localrepo
    enabled=1
    gpgcheck=0
    ```

---

#### 4. Indicizzazione e Test Finale
È necessario generare i metadati affinché la directory venga riconosciuta come repository e pulire la cache precedente.

1.  **Generazione metadati**:
    ```bash
    createrepo_c /localrepo
    ```

2.  **Sincronizzazione di dnf**:
    ```bash
    dnf clean all
    dnf repolist all
    ```

3.  **Verifica dell'installazione**: Testare il repository installando un pacchetto di esempio:
    ```bash
    dnf install httpd
    ```

---

### Gestione Avanzata dei Pacchetti

In questa lezione viene approfondita la gestione dei pacchetti su sistemi CentOS e Red Hat, esplorando l'uso avanzato dei comandi `dnf` e `rpm`. L'obiettivo è comprendere come installare, aggiornare, eliminare e interrogare i pacchetti per ottenere dettagli tecnici e individuare i relativi file di configurazione.

---

#### 1. Metodi di Installazione e Rimozione
Esistono due approcci principali per gestire il software: l'uso di un gestore ad alto livello (`dnf`) o l'uso dello strumento a basso livello (`rpm`).

**A. Utilizzo di DNF (Metodo preferito)**
DNF gestisce automaticamente il download e la risoluzione delle dipendenze dai repository.
* **Installazione**:
    ```bash
    dnf install ksh -y
    ```
* **Rimozione**:
    ```bash
    dnf remove ksh -y
    ```

**B. Utilizzo di RPM (Installazione manuale)**
Se non si ha accesso ai repository o si dispone solo del file locale `.rpm`, si utilizza il comando `rpm`.
1.  **Download del pacchetto**: È possibile scaricare il file tramite `wget` (se si ha l'URL diretto):
    ```bash
    wget [https://esempio.com/percorso/pacchetto.rpm](https://esempio.com/percorso/pacchetto.rpm)
    ```
2.  **Installazione**: L'opzione `-ivh` sta per *install*, *verbose* (dettagliato) e *hash* (barra di avanzamento):
    ```bash
    rpm -ivh ksh-2020.0.0-1.el9.x86_64.rpm
    ```
3.  **Rimozione**: Si utilizza l'opzione `-e` (*erase*). È necessario specificare il nome esatto del pacchetto:
    ```bash
    rpm -e ksh
    ```

---

#### 2. Interrogazione e Informazioni sui Pacchetti
Il comando `rpm` permette di interrogare il database locale per ottenere informazioni sui pacchetti installati.

| Comando | Descrizione |
| :--- | :--- |
| `rpm -qa | grep ksh` | Verifica se il pacchetto è installato (*query all*). |
| `rpm -qi ksh` | Mostra informazioni dettagliate (versione, data, descrizione). |
| `rpm -qc ksh` | Elenca i **file di configurazione** del pacchetto (solitamente in `/etc`). |
| `rpm -qf /percorso/file` | Identifica a quale pacchetto appartiene un file o comando. |



---

#### 3. Identificare l'Origine di un Comando
Per sapere quale pacchetto fornisce un comando specifico (es. `pwd` o `ls`), si segue questa procedura:

1.  Trovare il percorso completo del comando:
    ```bash
    which pwd
    ```
    *Esempio di output: `/usr/bin/pwd`*

2.  Interrogare il pacchetto di origine tramite il file:
    ```bash
    rpm -qf /usr/bin/pwd
    ```
    *Risultato: `coreutils-8.32-31.el9.x86_64`*

> **Nota**: La rimozione del pacchetto `coreutils` renderebbe il sistema instabile poiché comandi essenziali come `pwd` non sarebbero più disponibili.

---

### Ripristino di Aggiornamenti e Patch (Rollback)

In qualità di amministratore di sistema, l'aggiornamento dei pacchetti e l'applicazione di patch di sicurezza sono attività critiche. Tuttavia, i nuovi aggiornamenti possono talvolta introdurre problemi di compatibilità con le applicazioni o i database esistenti. In questa lezione viene spiegato come gestire e annullare tali aggiornamenti.

---

#### 1. Best Practice: Snapshot su Macchine Virtuali
Prima di eseguire qualsiasi aggiornamento (`yum update` o `yum upgrade`), la strategia di rollback più sicura per chi opera in ambienti virtualizzati (come Oracle VirtualBox, VMware o piattaforme Cloud) è l'utilizzo degli **snapshot**.

* **Vantaggio**: Permette di riportare l'intero sistema allo stato esatto precedente l'aggiornamento in pochi secondi.
* **Procedura**: Creare uno snapshot rinominandolo con la data corrente prima di applicare le patch. Se il sistema diventa instabile, utilizzare la funzione "Revert" o "Ripristina".



---

#### 2. Differenza tra Update e Upgrade
È fondamentale distinguere i due comandi prima di eseguire un aggiornamento di sistema:

* **`yum update`**: Aggiorna i pacchetti alla versione più recente, ma **preserva** le versioni obsolete nel sistema. È il metodo più sicuro se si prevede di dover effettuare un rollback.
* **`yum upgrade`**: Aggiorna i pacchetti e **elimina** quelli obsoleti. Questo rende il rollback tramite cronologia molto più difficile o impossibile.

---

#### 3. Rollback tramite Cronologia YUM (Macchine Fisiche e Virtuali)
Se non è possibile usare gli snapshot (ad esempio su server fisici), si utilizza lo strumento di cronologia di YUM/DNF.

**A. Annullare l'installazione di un singolo pacchetto**
1.  Identificare l'ID dell'operazione:
    ```bash
    yum history
    ```
2.  Annullare l'operazione specifica (sostituendo `ID` con il numero trovato, es. 17):
    ```bash
    yum history undo ID
    ```

**B. Rollback di un intero aggiornamento di sistema**
Se un aggiornamento massivo (es. 150+ pacchetti) causa problemi, è possibile tentare il rollback dell'intera transazione:
1.  Trovare l'ID della transazione "update":
    ```bash
    yum history
    ```
2.  Eseguire l'undo della transazione:
    ```bash
    yum history undo 19
    ```
    *Il sistema proporrà il downgrade automatico di tutti i pacchetti coinvolti alla versione precedente.*

---

#### 4. Avvertenze Importanti
* **Stabilità del sistema**: Red Hat e CentOS non raccomandano il downgrade di intere versioni "minor" (es. da 7.5 a 7.1) su macchine fisiche, poiché il sistema potrebbe trovarsi in uno stato instabile.
* **Installazione pulita**: Se un rollback fallisce o non è possibile, la soluzione consigliata è procedere con una nuova installazione della versione specifica compatibile con l'applicazione.

---

### Connettività Remota: SSH vs Telnet

In ambiente Linux, SSH (Secure Shell) e Telnet sono i due servizi principali utilizzati per accettare connessioni da computer esterni. Sebbene servano allo stesso scopo, differiscono radicalmente per quanto riguarda la sicurezza.

---

#### 1. Differenze Fondamentali
Il modello di connessione si basa sul paradigma **Client/Server**. Quando ci si connette a un altro sistema, il proprio computer agisce come client, mentre il computer remoto agisce come server.

* **Telnet**: È un protocollo datato e **non sicuro**. Tutte le comunicazioni (inclusi username e password) vengono inviate in chiaro. Per questo motivo, la maggior parte delle aziende non lo utilizza più ed è spesso escluso dalle installazioni standard di Linux.
* **SSH**: È il protocollo moderno e **completamente sicuro**. Cripta tutto il traffico tra il client e il server, rendendolo lo standard per l'amministrazione remota.



---

#### 2. Gestione del Servizio SSH (sshd)
Il demone che gestisce le connessioni SSH in entrata si chiama `sshd`. Se questo processo viene interrotto, il server rifiuterà qualsiasi tentativo di accesso remoto.

**Comandi principali per la gestione:**

* **Verifica dello stato del servizio**:
    ```bash
    systemctl status sshd
    ```
    *Cercare la dicitura "active (running)" nell'output.*

* **Verifica del processo tramite `ps`**:
    ```bash
    ps -ef | grep sshd
    ```

* **Arresto del servizio** (Attenzione: interrompe la possibilità di nuovi accessi):
    ```bash
    systemctl stop sshd
    ```

* **Avvio del servizio**:
    ```bash
    systemctl start sshd
    ```



---

#### 3. Test di Connettività
È possibile testare il servizio tentando una connessione SSH verso l'indirizzo IP locale (localhost) o l'IP della macchina stessa:

1.  **Identificare l'IP**:
    ```bash
    ip addr
    ```
2.  **Tentare l'accesso**:
    ```bash
    ssh root@192.168.1.12
    ```

Se il servizio è attivo, verrà richiesta la password. Se è spento, il client restituirà l'errore: `Network error: Connection refused`.

---

#### 4. Installazione di Telnet (Solo per Troubleshooting)
Sebbene sconsigliato per l'accesso remoto, Telnet viene talvolta installato come strumento di diagnostica per verificare se una specifica porta di rete è aperta.

* **Installazione**:
    ```bash
    dnf install telnet -y
    ```

---

### DNS: Domain Name System

Il DNS è il sistema fondamentale che permette di tradurre i nomi di dominio leggibili dall'uomo (come google.com) in indirizzi IP numerici comprensibili dai computer. Senza il DNS, dovremmo ricordare gli indirizzi IP di ogni sito web, proprio come dovremmo ricordare a memoria ogni numero di telefono se non avessimo una rubrica sui nostri smartphone.

---

#### 1. Concetti Fondamentali e Record DNS
In un colloquio di lavoro su Linux, è molto probabile che ti vengano chiesti i seguenti tipi di record:

* **A Record (Address):** Traduce un **Hostname in un Indirizzo IP**.
* **PTR Record (Pointer):** Traduce un **Indirizzo IP in un Hostname** (Reverse Lookup).
* **CNAME (Canonical Name):** Crea un **Alias** (traduce un hostname in un altro hostname). Ad esempio, più server con nomi diversi possono rispondere tutti all'alias "google.com".



---

#### 2. Componenti del Servizio DNS in Linux
* **Pacchetto:** Il software principale si chiama `bind` (Berkeley Internet Name Domain).
* **Demone:** Il processo che gira in background si chiama `named` (Name Daemon).
* **Directory di configurazione:** `/etc/`
* **File di configurazione principale:** `/etc/named.conf`
* **Directory dei file di zona:** `/var/named/` (dove risiedono i database dei nomi).

---

#### 3. Configurazione del Laboratorio
In questo scenario, configuriamo un server DNS per il dominio fittizio `lab.local`.



**A. Installazione**
```bash
dnf install bind bind-utils -y
```

**B. Modifica di `/etc/named.conf`**
Bisogna istruire il server ad ascoltare sull'indirizzo IP della propria interfaccia di rete (es. `enp0s3`) e definire le zone:
* **Forward Zone (`forward.lab`):** Per le ricerche nome -> IP.
* **Reverse Zone (`reverse.lab`):** Per le ricerche IP -> nome.



**C. Creazione dei File di Zona**
I file in `/var/named/` devono contenere i record specifici. 
> **Importante:** Ogni volta che si modifica un file di zona, è necessario incrementare il **Serial Number** all'interno del file affinché le modifiche vengano riconosciute.

---

#### 4. Strumenti di Verifica e Test
Dopo aver avviato il servizio con `systemctl start named`, si utilizzano strumenti specifici per testare la risoluzione:

| Comando | Scopo |
| :--- | :--- |
| `named-checkconf` | Verifica la sintassi del file di configurazione principale. |
| `named-checkzone` | Verifica la coerenza di un file di zona specifico. |
| `dig` | Strumento avanzato per interrogare il DNS (mostra molti dettagli). |
| `nslookup` | Strumento semplice per testare la risoluzione nome/IP. |

Esempio di test:
```bash
nslookup clienta.lab.local
```

#### 5. Configurazione del Client
Affinché il sistema utilizzi il nuovo server DNS, è necessario seguire questi passaggi:

1.  **Modifica temporanea**: Modificare il file `/etc/resolv.conf` impostando l'indirizzo del nuovo server:
    ```bash
    nameserver 192.168.100.153
    ```
2.  **Modifica persistente**: Aggiornare la configurazione della scheda di rete (es. `enp0s3`) all'interno della directory `/etc/NetworkManager/system-connections/` aggiungendo il parametro `DNS=192.168.100.153`.
3.  **Riavvio**: Riavviare il NetworkManager per applicare le modifiche:
    ```bash
    systemctl restart NetworkManager
    ```

---

### Strumenti di Risoluzione: nslookup e dig

In ambiente Linux, la risoluzione dei nomi (DNS lookup) è il processo attraverso il quale un nome host (hostname) viene tradotto nel corrispondente indirizzo IP. Sebbene protocolli come il `ping` o i browser web effettuino questa traduzione automaticamente, esistono strumenti specifici per interrogare direttamente i server DNS.

---

#### 1. Funzionamento della Risoluzione
I computer comunicano esclusivamente tramite indirizzi IP, ma per gli esseri umani è molto più semplice memorizzare nomi testuali (es. www.google.com). Il sistema DNS agisce come un traduttore tra queste due realtà.



* **Server DNS (Resolver):** È il server che riceve la richiesta di traduzione. In un ambiente domestico, solitamente coincide con il modem/router.
* **Porta DNS:** Il servizio DNS opera generalmente sulla **porta 53**.
* **Risposta Non Autorevole (Non-authoritative answer):** Indica che il server interrogato (es. il proprio modem) non possiede direttamente il record richiesto nel proprio database locale, ma ha dovuto recuperare l'informazione da altri server sulla rete internet.

---

#### 2. Lo Strumento nslookup
`nslookup` (Name Service Look Up) è un'utility storica disponibile sia su Linux che su Windows. Può essere utilizzata in due modalità:

* **Interattiva:** Digitando semplicemente `nslookup` si accede a un prompt dedicato dove è possibile inserire più nomi di dominio in sequenza.
* **Diretta:** Seguita dal nome del sito per ottenere un risultato immediato.

```bash
# Esempio di utilizzo diretto
nslookup [www.hotmail.com](https://www.hotmail.com)
```

#### 3. Lo Strumento dig

`dig` (Domain Information Groper) è lo strumento moderno preferito dagli amministratori di sistema Linux. Rispetto a `nslookup`, fornisce dettagli molto più approfonditi sulla struttura della risposta DNS e sui diversi tipi di record.

```bash
# Esempio di utilizzo
dig [www.facebook.com](https://www.facebook.com)
```

#### 4. Verifica Pratica

Per comprendere l'efficacia della risoluzione, è possibile:

1. Ottenere l'indirizzo IP di un sito tramite nslookup o dig.

2. Inserire tale indirizzo IP direttamente nella barra degli indirizzi di un browser.

3. Osservare come il browser carichi correttamente la pagina (es. la pagina di login di Facebook), dimostrando che la navigazione avviene tecnicamente tramite IP, anche se l'utente utilizza il nome.

---

### NTP: Network Time Protocol

Il Network Time Protocol (NTP) è un servizio fondamentale per la sincronizzazione dell'orario dei computer all'interno di una rete. 

#### Importanza della sincronizzazione
In ambito aziendale, mantenere l'orario sincronizzato tra tutti i server è di vitale importanza. Quando si opera in ambienti cluster o con workflow complessi, anche un piccolo sfasamento temporale (time drift) di pochi secondi o minuti può causare:
* Errori di autenticazione.
* Incongruenze nei log di sistema (difficoltà nel troubleshooting).
* Problemi nei processi di backup e replica dei dati.



---

#### 1. Verifica e Installazione
Per gestire il servizio NTP su Linux, è necessario innanzitutto verificare la presenza del pacchetto.

* **Verifica installazione:**
  ```bash
  rpm -qa | grep ntp
  ```
* **Installazione (se mancante):**
  ```bash
  yum install ntp -y

#### 2. Configurazione del Servizio
Il file di configurazione principale si trova in `/etc/ntp.conf`. All'interno di questo file vengono definiti i server esterni dai quali il sistema preleverà l'orario corretto.

* **Modifica del file:**
  Aprire `/etc/ntp.conf` e individuare le righe che iniziano con `server`. È possibile aggiungere server pubblici affidabili, come quelli di Google o i pool predefiniti della distribuzione:
  ```text
  server 8.8.8.8
  ```

*(Nota: In contesti reali, si utilizzano solitamente i server del progetto pool.ntp.org per una maggiore precisione e affidabilità).*



---

#### 3. Gestione del Demone ntpd
Il servizio è gestito dal demone `ntpd` (NTP Daemon), che rimane attivo in background per monitorare e correggere costantemente lo scostamento temporale del sistema.

* **Avvio del servizio:**
  ```bash
  systemctl start ntpd
  ```

* **Verifica dello stato:**
  Utilizzare `status` per confermare che il servizio sia "active (running)":
  ```bash
  systemctl status ntpd
  ```
#### 4. Monitoraggio e Diagnostica
Per verificare l'effettiva sincronizzazione con i server di riferimento e misurare la qualità del segnale temporale, si utilizza l'interfaccia di controllo `ntpq`.

* **Comando Peers:**
  Entrare in modalità interattiva e digitare `peers` per visualizzare l'elenco dei server remoti e lo stato della connessione:
  ```bash
  ntpq
  ntpq> peers
  ```

L'output mostrerà i server configurati, indicando parametri fondamentali per la diagnostica: * Stratum: La distanza gerarchica dalla sorgente temporale atomica (più è basso, più la sorgente è considerata primaria e precisa). * Delay: Il tempo di andata e ritorno (latenza) dei pacchetti di rete tra il client e il server. * Offset: La differenza temporale calcolata (in millisecondi) tra il clock del server e il clock locale del client. * Jitter: Indica la variabilità o l'instabilità della latenza di rete rilevata.

### Chrony: L'evoluzione di NTP

Nelle distribuzioni Linux moderne (come Red Hat Enterprise Linux 7/8+ e CentOS 7/8+), il demone tradizionale `ntpd` è stato sostituito da **Chrony**. Chrony è un'implementazione più versatile del Network Time Protocol, progettata per sincronizzare l'orologio di sistema in modo più rapido e con una precisione superiore, specialmente in sistemi che non sono costantemente connessi alla rete o che presentano variazioni di latenza.

---

#### 1. Componenti Principali
* **Pacchetto:** `chrony`
* **Demone:** `chronyd` (il processo in background).
* **Utility CLI:** `chronyc` (l'interfaccia a riga di comando per il monitoraggio).
* **File di configurazione:** `/etc/chrony.conf`
* **Log di sistema:** `/var/log/chrony/`

---

#### 2. Installazione e Configurazione
Prima di attivare Chrony, è fondamentale assicurarsi che il vecchio servizio NTP non sia in esecuzione per evitare conflitti.

**A. Disattivazione di NTP (se presente):**
```bash
systemctl stop ntpd
systemctl disable ntpd
```

**B. Installazione di Chrony:**
```bash
yum install chrony -y
```

**C. Configurazione dei Server: All'interno di /etc/chrony.conf, si definiscono i server di riferimento tramite la direttiva server o pool.**
```text
# Esempio di aggiunta server nel file /etc/chrony.conf
server pool.ntp.org iburst
```

* **Nota:** L'opzione `iburst` permette una sincronizzazione molto più rapida all'avvio del servizio.*


#### 3. Gestione del Servizio

Una volta configurato, il demone deve essere avviato e impostato per l'esecuzione automatica al boot.
```bash
# Avvio e abilitazione al boot
systemctl start chronyd
systemctl enable chronyd

# Verifica dello stato
systemctl status chronyd
```

#### 4. Monitoraggio con chronyc
#### 4. Monitoraggio con chronyc
L'utility `chronyc` rappresenta l'interfaccia a riga di comando per interagire con il demone. Consente di monitorare le prestazioni di sincronizzazione e lo stato delle sorgenti temporali senza dover riavviare il servizio.

* **Modalità interattiva**: Digitando `chronyc` nel terminale si accede a una shell dedicata dove è possibile impartire vari comandi (es. `sources`, `tracking`, `activity`).
* **Verifica delle sorgenti**: Il comando più utilizzato è `sources`, che elenca i server a cui il client è connesso. L'opzione `-v` (verbose) fornisce una spiegazione dettagliata delle colonne.

```bash
# Visualizzazione delle sorgenti temporali con output dettagliato
chronyc sources -v
```

**Interpretazione dei simboli (Colonna MS):**
* **`*` (Current)**: Indica la sorgente temporale primaria con cui il sistema è attualmente sincronizzato.
* **`+` (Combined)**: Indica sorgenti affidabili e utilizzabili come backup in combinazione con la principale.
* **`-` (Excluded)**: Indica sorgenti scartate dall'algoritmo di selezione.
* **`?` (Unreachable)**: Indica che il server non è raggiungibile o la connessione è stata persa.



Qualsiasi modifica apportata al file di configurazione `/etc/chrony.conf` richiede un riavvio del demone per essere applicata:
```bash
systemctl restart chronyd
```

Infine, per assicurarsi che il servizio rimanga attivo anche dopo un eventuale riavvio del server:
```bash
systemctl enable chronyd
```

---

### Gestione dell'orario con timedatectl

Nelle distribuzioni Linux moderne basate su **systemd** (come RHEL 7/8+ e CentOS 7/8+), il comando `timedatectl` è diventato lo standard per la gestione dell'orario e della data, sostituendo gradualmente il comando tradizionale `date`.

---

#### 1. Caratteristiche Principali
* **Integrazione:** Fa parte del sistema `systemd`.
* **Completezza:** Permette di visualizzare e modificare l'ora locale, l'ora universale (UTC), l'orologio hardware (RTC) e i fusi orari.
* **Sincronizzazione:** Può gestire la sincronizzazione automatica tramite client come `chronyd`, `ntpd` o il leggero `systemd-timesyncd`.



---

#### 2. Comandi di Visualizzazione
Per ottenere una panoramica completa dello stato attuale dell'orologio di sistema:

```bash
# Mostra lo stato attuale (ora, fuso orario e stato NTP)
timedatectl
# oppure
timedatectl status
```

L'output fornirà dettagli cruciali come:
* **Local time / Universal time:** L'ora attuale del sistema e quella coordinata universale (UTC).
* **RTC time:** L'orario dell'orologio hardware (Real Time Clock).
* **Time zone:** Il fuso orario impostato (es. America/New_York).
* **System clock synchronized:** Indica se l'orologio è attualmente sincronizzato con una sorgente esterna.
* **NTP service:** Indica se il servizio di sincronizzazione è attivo o inattivo.



---

#### 3. Gestione dei Fusi Orari (Timezone)
È possibile visualizzare l'elenco di tutti i fusi orari supportati e impostare quello corretto per la propria area geografica.

* **Elencare i fusi orari:**
  ```bash
  timedatectl list-timezones
  ```
  *(Utilizzare lo spazio per scorrere e `q` per uscire).*
* **Impostare un nuovo fuso orario:**
  ```bash
  timedatectl set-timezone "Europe/Rome"
  ```

#### 4. Modifica Manuale di Data e Ora
Se la sincronizzazione automatica non è attiva, è possibile impostare manualmente i parametri temporali. **Nota:** Il comando fallirà se la sincronizzazione NTP è già abilitata; in tal caso, occorre prima disabilitarla con `timedatectl set-ntp false`.

* **Impostare solo la data:**
    ```bash
    timedatectl set-time "2026-01-08"
    ```
* **Impostare data e ora insieme:**
    ```bash
    timedatectl set-time "2026-01-08 15:30:00"
    ```



---

#### 5. Abilitazione della Sincronizzazione NTP
Per far sì che il sistema utilizzi un servizio esterno (come `chronyd` o `ntpd`) per mantenere l'ora precisa, è necessario abilitare la funzione NTP tramite il comando di controllo.

**A. Verifica del servizio sottostante:**
È necessario assicurarsi che un demone di sincronizzazione sia installato e attivo sul sistema.
```bash
systemctl status chronyd
```

**B. Attivazione della sincronizzazione:**
```bash
# Abilita la sincronizzazione automatica via rete
timedatectl set-ntp true
```

Dopo l'esecuzione, verificando nuovamente con timedatectl, la voce System clock synchronized passerà a "yes" non appena il demone avrà agganciato correttamente il server remoto.

---

### Server di Posta in Linux: Postfix

Un server di posta è un sistema informatico che funge da ufficio postale digitale, gestendo l'invio, la ricezione, l'elaborazione e l'archiviazione delle email tra utenti e domini. In ambito amministrativo Linux, è essenziale per ricevere notifiche automatiche sull'esito di workflow e processi di sistema.



---

#### 1. Panoramica dei Software di Posta
CentOS e le distribuzioni RHEL-based offrono diverse opzioni:
* **Postfix:** Il Mail Transfer Agent (MTA) moderno, sicuro e predefinito per CentOS/RHEL 8+. È preferito per la sua semplicità di configurazione.
* **Sendmail:** Uno dei più antichi e complessi, ora deprecato nelle versioni più recenti.
* **Dovecot:** Utilizzato specificamente come server IMAP e POP3 per la ricezione e gestione delle caselle postali.
* **s-nail:** Un pacchetto leggero che fornisce il comando `mail` per scrivere e inviare email direttamente dal terminale.

---

#### 2. Installazione di Postfix e s-nail
Per configurare un sistema di invio mail, è necessario installare sia il motore di trasporto (Postfix) che l'utility client (s-nail).

```bash
# Installazione dei pacchetti necessari
dnf install postfix s-nail -y
```

#### 3. Configurazione: Il Relay Host
In ambito aziendale, i server Linux spesso non inviano mail direttamente verso l'esterno, ma si appoggiano a un **Mail Relay Server**. Questo funge da intermediario (middleman) che accetta le email dai server interni e le consegna ai destinatari finali.

La configurazione principale avviene nel file `/etc/postfix/main.cf`.

**Procedura di configurazione:**
1.  Aprire il file con un editor di testo: `vi /etc/postfix/main.cf`.
2.  Individuare la riga contenente `#relayhost`.
3.  Decommentare la riga (rimuovendo il carattere `#`) e specificare il server di inoltro.

```text
# Esempi di configurazione nel file main.cf
relayhost = [192.168.1.100]          # Uso di un indirizzo IP (tra parentesi quadre)
relayhost = [smtp.nomedominio.it]    # Uso di un nome di dominio completo (FQDN)
```

#### 4. Gestione del Servizio
Postfix opera come un demone in background gestito tramite `systemctl`. È importante notare che, a differenza di servizi come `chronyd` o `sshd`, il nome di questo servizio non termina con la lettera "d".

```bash
# Avvio o riavvio del servizio per applicare le modifiche
systemctl restart postfix

# Abilitazione all'avvio automatico del sistema
systemctl enable postfix

# Verifica dello stato operativo
systemctl status postfix
```

Per una verifica più approfondita a livello di processi, è possibile cercare il "master process" di Postfix, responsabile della gestione di tutte le code di messaggi:


```bash
ps -ef | grep postfix
```

#### 5. Invio di una Mail di Test

Per inviare messaggi dalla riga di comando si utilizza il pacchetto s-nail, che fornisce l'interfaccia necessaria per comunicare con il motore di Postfix. Questa operazione è fondamentale per automatizzare le notifiche di sistema.

Esempio di invio:

```bash
mail -s "Test Configurazione Mail" utente@esempio.com
```

**Procedura operativa:**
1.  **Esecuzione**: Dopo aver impartito il comando `mail -s`, il cursore si sposterà su una nuova riga vuota.
2.  **Composizione**: Digitare il testo del messaggio. In questa fase il terminale funziona come un editor di testo semplificato.
3.  **Conclusione**: Premere la combinazione di tasti **Ctrl + D** per segnalare la fine del messaggio (EOF - End Of File).
4.  **Invio**: Il sistema potrebbe chiedere conferma per l'invio o mostrare i destinatari; rispondere con **Y** (Yes) o premere **Invio** per finalizzare la spedizione.

---

### Web Server (HTTPD): Creazione del primo sito web

Il servizio HTTP (o Web Server) ha lo scopo di ospitare e servire pagine web (come Google, Facebook o siti personali) ai client che ne fanno richiesta tramite un browser. In ambiente Linux Red Hat/CentOS, il software di riferimento è **Apache**, il cui pacchetto e servizio sono denominati `httpd`.

---

#### 1. Componenti Fondamentali
Per gestire un server web, è necessario conoscere i seguenti percorsi e file:

* **Pacchetto:** `httpd`
* **File di configurazione:** `/etc/httpd/conf/httpd.conf`. Permette di definire la porta di ascolto (default 80), le directory degli utenti e i permessi di accesso.
* **Document Root:** `/var/www/html/`. È la directory principale che contiene i file del sito.
* **Homepage:** `index.html`. È il file predefinito che il server cerca quando un utente visita l'indirizzo IP o il dominio.
* **Log di sistema:** `/var/log/httpd/`. Essenziale per il troubleshooting (es. `access_log` e `error_log`).



---

#### 2. Installazione e Configurazione
Prima di procedere, occorre verificare la presenza del software e avviarlo.

```bash
# Verificare se il pacchetto è installato
rpm -qa | grep httpd

# Se non presente, installarlo
dnf install httpd -y

# Abilitare il servizio al boot e avviarlo
systemctl enable httpd
systemctl start httpd
```

#### 3. Creazione della Pagina Web (HTML)

Per creare la propria homepage, bisogna editare il file index.html all'interno della Document Root.

```bash
cd /var/www/html/
vi index.html
```

Esempio di codice HTML di base da inserire:


```HTML
<!DOCTYPE html>
<html>
<body style="background-color:lightgrey;">
    <h1 style="text-align:center;">Benvenuti nella mia prima pagina!</h1>
    <p style="text-align:center;">Sito web ospitato su server Linux.</p>
</body>
</html>
```

#### 4. Risoluzione dei Problemi di Accesso
Se, inserendo l'indirizzo IP del server nel browser, la pagina non viene visualizzata o viene restituito un errore di connessione, le cause più comuni sono legate al firewall di sistema o allo stato del demone.



* **Firewall del sistema**: Per impostazione predefinita, Linux blocca le connessioni in entrata sulla porta 80. Per consentire l'accesso, è necessario configurare le regole o, per test rapidi, arrestare il servizio:
    ```bash
    # Arresto temporaneo del firewall per test
    systemctl stop firewalld
    ```
* **Stato del servizio**: Il web server deve essere esplicitamente avviato. Un errore tipico è configurare i file senza aver attivato il processo.
    ```bash
    # Controllo se il servizio è 'active (running)'
    systemctl status httpd
    ```

---

#### 5. Verifica Finale e Modifiche in Tempo Reale
Una volta rimosse le restrizioni del firewall e avviato `httpd`, è sufficiente digitare l'indirizzo IP del server nella barra degli indirizzi del browser:
`http://<INDIRIZZO_IP_DEL_SERVER>`



Il server leggerà automaticamente il file `index.html`. Qualsiasi modifica salvata in `/var/www/html/index.html` sarà visibile immediatamente ricaricando la pagina (tasto **F5**), senza la necessità di riavviare il servizio Apache.

---

### Installazione e Gestione di Nginx: Da Web Server a Reverse Proxy

Nginx (pronunciato "Engine-X") è un server web ad alte prestazioni progettato per gestire un numero elevato di connessioni simultanee (il problema C10K). Oltre a servire pagine statiche, è ampiamente utilizzato come **Reverse Proxy** e **Load Balancer**.

---

#### 1. Cos'è un Reverse Proxy?
Un reverse proxy funge da intermediario tra il client (utente) e il server web reale. Le richieste arrivano a Nginx, che le inoltra al server di backend appropriato. Questo permette di distribuire il carico, aumentare la sicurezza e gestire meglio il traffico.



---

#### 2. Installazione e Avvio
Nginx può essere installato tramite il gestore pacchetti `dnf`.

```bash
# Installazione
dnf install nginx -y

# Avvio e abilitazione al boot
systemctl start nginx
systemctl enable nginx

# Verifica dello stato
systemctl status nginx
```

> **Nota di Troubleshooting:** Se Nginx fallisce l'avvio, è molto probabile che la porta **80** sia già occupata da un altro servizio (come Apache/httpd). È possibile identificare il processo colpevole e liberare la porta con i seguenti comandi:

```bash
# Identificare il processo che occupa la porta 80
lsof -i :80

# Se il colpevole è httpd, fermarlo e disabilitarlo
systemctl stop httpd
systemctl disable httpd

# Riprovare ad avviare Nginx
systemctl start nginx
```

#### 3. Configurazione di un Sito Personalizzato
Anziché modificare il file principale `/etc/nginx/nginx.conf`, la **best practice** prevede la creazione di file di configurazione modulari nella directory `/etc/nginx/conf.d/`. Questo approccio mantiene l'installazione pulita e isola eventuali errori di sintassi.

**Esempio di configurazione (`/etc/nginx/conf.d/mio_sito.conf`):**
```nginx
server {
    listen 80;
    server_name 192.168.100.161; # Sostituire con l'IP del proprio server

    location / {
        root /var/www/mio_sito/html;
        index index.html;
    }
}
```

**Procedura per rendere effettive le modifiche:**
1.  **Creazione directory**: Eseguire `mkdir -p /var/www/mio_sito/html` per preparare lo spazio che ospiterà i file del sito.
2.  **Creazione pagina**: Creare il file `index.html` (ad esempio con `vi` o `nano`) inserendo un messaggio personalizzato, come "Benvenuti su MyFirstLinuxOS".
3.  **Test della sintassi**: È fondamentale eseguire `nginx -t`. Questo comando controlla che non ci siano errori di battitura nel file di configurazione prima di applicarlo.
4.  **Ricaricamento del servizio**: Se il test è positivo, eseguire `systemctl restart nginx` per rendere attive le nuove impostazioni.



---

#### 4. Nginx come Reverse Proxy
Configurare Nginx come **Reverse Proxy** significa istruirlo a ricevere le richieste dai client e inoltrarle a un altro server di backend. In questo scenario, Nginx funge da "receptionist" che smista il traffico verso il server corretto senza che l'utente finale conosca l'indirizzo reale del backend.



**Configurazione del Proxy:**
All'interno del blocco `location` del file di configurazione, si utilizza la direttiva `proxy_pass` per indicare la destinazione del traffico:

```nginx
server {
    listen 80;
    server_name 192.168.100.161;

    location / {
        proxy_pass [http://192.168.100.162](http://192.168.100.162); # Indirizzo IP del server di backend
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

#### 5. Risoluzione Errori Comuni (SELinux)
In distribuzioni come CentOS, RHEL o Rocky Linux, **SELinux** (Security-Enhanced Linux) è spesso la causa di errori che impediscono il corretto funzionamento di Nginx, anche se i permessi dei file sembrano corretti.

I due problemi più frequenti sono:

* **Errore 403 (Forbidden)**: Nginx non ha il permesso di leggere i file nella Document Root perché non possiedono l'etichetta di sicurezza appropriata.
    * **Soluzione**: È necessario cambiare il contesto di sicurezza dei file affinché SELinux li riconosca come contenuti web legittimi.
    ```bash
    chcon -t httpd_sys_content_t /var/www/mio_sito/html -R
    ```



* **Errore 502 (Bad Gateway)**: Quando Nginx funge da Reverse Proxy, SELinux impedisce al processo di effettuare connessioni di rete verso altri server (l'upstream). Nei log si troverà l'errore `Permission denied while connecting to upstream`.
    * **Soluzione**: Bisogna abilitare la "booleana" di SELinux che permette al server web di connettersi alla rete.
    ```bash
    # Il flag -P rende la modifica persistente dopo il riavvio
    setsebool -P httpd_can_network_connect 1
    ```



---

#### 6. Troubleshooting Rapido
Se il sito continua a non caricarsi, ecco una check-list veloce:
1.  **Stato del servizio**: `systemctl status nginx` (deve essere *active*).
2.  **Sintassi**: `nginx -t` (deve restituire *syntax is ok*).
3.  **Firewall**: `systemctl stop firewalld` (per testare se il blocco è dovuto alle porte chiuse).
4.  **Log degli errori**: `tail -f /var/log/nginx/error.log` (per vedere cosa succede in tempo reale).

---

### Gestione Centralizzata dei Log con Rsyslog

In un ambiente con centinaia o migliaia di server, è impensabile collegarsi a ogni singola macchina per controllare i log in caso di problemi. La soluzione è un **Central Logger**: un server dedicato che riceve e organizza i log provenienti da tutti i client della rete (server Linux, Windows, stampanti, router, ecc.).



---

#### 1. Il Servizio Rsyslog
Il cuore della gestione dei log su Linux è **Rsyslog**. È un servizio potente e flessibile che permette non solo di gestire i log locali in `/var/log`, ma anche di inoltrarli o riceverli via rete.

* **Pacchetto**: `rsyslog`
* **File di Configurazione**: `/etc/rsyslog.conf`
* **Porta di Default**: **514** (utilizzata sia per UDP che per TCP)

---

#### 2. Comandi di Gestione
Per operare su Rsyslog, è necessario avere i privilegi di **root**.

```bash
# Verificare se il pacchetto è installato
rpm -qa | grep rsyslog

# Se non presente, installarlo
yum install rsyslog -y

# Gestione del demone con systemd
systemctl start rsyslog    # Avvia il servizio
systemctl enable rsyslog   # Lo abilita al boot
systemctl status rsyslog   # Verifica se è 'active (running)'
systemctl restart rsyslog  # Applica le modifiche alla configurazione
```

#### 3. Configurazione del Client (Inoltro Log)
Per fare in modo che un server (client) invii i propri log a un **Central Logger**, è necessario modificare il file di configurazione principale `/etc/rsyslog.conf`. 

In fondo al file, si specifica a quale server inviare le informazioni e tramite quale protocollo:

* **UDP (@)**: Più veloce ma meno affidabile (non garantisce la ricezione).
* **TCP (@@)**: Più affidabile, garantisce che il log arrivi a destinazione.

```bash
# Esempio di configurazione per inoltro via UDP
# Sintassi: *.* @INDIRIZZO_IP:PORTA
*.* @192.168.100.160:514

# Esempio di configurazione per inoltro via TCP
*.* @@192.168.100.160:514
```

> **Nota sui moduli**: Se il server deve agire come **Central Logger** (ovvero deve ricevere log da altri), è necessario abilitare i moduli di ricezione all'interno di `/etc/rsyslog.conf`. Di default, queste righe sono commentate con un `#`. Rimuovi il commento per attivare l'ascolto sulla porta 514:

```nginx
# Per ricevere log via UDP
module(load="imudp")
input(type="imudp" port="514")

# Per ricevere log via TCP
module(load="imtcp")
input(type="imtcp" port="514")
```

#### 4. Perché usare un Central Logger?
Immagina di lavorare in un ambiente con **1.000 server**. Se si verificano problemi su 10 o 100 macchine contemporaneamente, collegarsi a ogni singolo server per controllare i log sarebbe un compito estenuante e inefficiente. Un Central Logger trasforma questo caos in un processo snello:

* **Efficienza e Produttività**: Non perdi tempo a saltare da un server all'altro. Hai una "visione d'insieme" di tutta l'infrastruttura da un'unica console.
* **Sicurezza Forense**: Se un hacker riesce a compromettere un server, la prima cosa che farà sarà cancellare i log locali per eliminare le proprie tracce. Con un Central Logger, i log vengono inviati istantaneamente a un server esterno sicuro, rendendo impossibile per l'attaccante cancellare le prove dell'intrusione.
* **Gestione degli Errori**: Permette di correlare eventi. Se un database fallisce, puoi vedere istantaneamente se nello stesso secondo ci sono stati errori di rete o di archiviazione su altri dispositivi.



---

#### 5. Verifica del Servizio e Pacchetti
Assicurati che il servizio sia attivo e configurato per avviarsi automaticamente al boot del sistema. Ricorda che per modificare questi parametri devi avere i privilegi di **root**.

```bash
# Verifica l'installazione del pacchetto
rpm -qa | grep rsyslog

# Se il pacchetto manca, installalo con:
yum install rsyslog -y

# Controlla se il servizio è 'active (running)'
systemctl status rsyslog

# Avvia e abilita il servizio per il prossimo riavvio
systemctl start rsyslog
systemctl enable rsyslog
```

---

# Guida all'Installazione e Configurazione di Nagios Core

## 1. Introduzione a Nagios
**Nagios** è un potente sistema di monitoraggio eventi che permette di supervisionare l'intera infrastruttura IT, inclusi server, switch, reti e applicazioni. Il suo scopo principale è garantire che tutti i servizi funzionino correttamente, inviando notifiche tempestive in caso di anomalie.

### Caratteristiche Principali:
* **Monitoraggio Attivo**: Controlla costantemente lo stato di salute di server e computer.
* **Sistema di Alerting**: Notifica gli utenti tramite SMS, email o chiamate quando si verifica un problema.
* **Escalation dei Log**: Permette di scalare gli avvisi se il primo responsabile non risponde.
* **Storico degli Eventi**: Mantiene un registro dettagliato di quando un problema è stato segnalato e come è stato risolto.

> **Nota Storica**: Creato nel 1999 da Ethan Galstad con il nome di *NetSaint*, è diventato ufficialmente *Nagios* nel 2002 per motivi di marchio. La versione open-source è nota come **Nagios Core**.



---

## 2. Requisiti e Preparazione (Step 1-2)

### Step 1: Installazione dei Pacchetti Necessari
Nagios richiede diversi strumenti di sviluppo e un server web per l'interfaccia grafica.
```bash
dnf install httpd php gcc glibc glibc-common gd gd-devel make net-snmp unzip wget -y
```

* **gcc/glibc**: Indispensabili per compilare Nagios partendo dal codice sorgente.
* **httpd/php**: Necessari per gestire e visualizzare l'interfaccia web di amministrazione.
* **gd/gd-devel**: Utilizzati per generare mappe grafiche e grafici sullo stato dei servizi.
* **Utilities (unzip, wget, make)**: Strumenti fondamentali per scaricare, estrarre e gestire l'installazione del software.

### Step 2: Creazione di Utente e Gruppo
Per ragioni di sicurezza e gestione dei permessi, è necessario creare un utente dedicato e un gruppo specifico per l'esecuzione dei comandi esterni.

```bash
# Creazione dell'utente nagios e del gruppo nagcmd
useradd nagios
groupadd nagcmd

# Aggiunta degli utenti nagios e apache al gruppo nagcmd
usermod -a -G nagcmd nagios
usermod -a -G nagcmd apache
```

## 3. Installazione di Nagios Core (Step 3-5)

### Step 3: Download ed Estrazione
Si utilizza la directory `/tmp` per scaricare i sorgenti ufficiali. In questo esempio viene utilizzata la versione 4.5.4 (verificare sempre l'ultima disponibile su nagios.org).

```bash
cd /tmp
# Download del pacchetto sorgente
wget -O nagios-4.5.4.tar.gz [https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.4.tar.gz](https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.4.tar.gz)

# Estrazione dell'archivio
tar xzf nagios-4.5.4.tar.gz
cd nagios-4.5.4
```

### Step 4: Configurazione del Build
In questa fase si prepara il codice sorgente per la compilazione, istruendo Nagios a utilizzare il gruppo `nagcmd` per l'esecuzione dei comandi esterni. Questo passaggio garantisce che solo gli utenti autorizzati possano impartire comandi critici al sistema di monitoraggio.

```bash
./configure --with-command-group=nagcmd
```

> **Nota di Troubleshooting:** Se il processo di configurazione fallisce con l'errore **"Cannot find SSL header"**, significa che mancano le librerie di sviluppo di OpenSSL. Per risolvere il problema, è necessario installare il pacchetto richiesto e rieseguire la configurazione:
>
> ```bash
> dnf install openssl-devel -y
> ./configure --with-command-group=nagcmd
> ```

---

### 5. Compilazione e Installazione di Nagios Core
Dopo la configurazione, si procede alla compilazione del codice sorgente e all'installazione dei componenti del sistema tramite una serie di comandi `make`.



* **`make all`**: Compila i file sorgente trasformandoli in programmi eseguibili dal sistema.
* **`make install`**: Installa il programma principale e i file di supporto nelle directory di sistema.
* **`make install-commandmode`**: Imposta i permessi corretti per l'interfaccia dei comandi esterni.
* **`make install-init`**: Installa gli script di avvio per gestire Nagios come servizio di sistema.
* **`make install-config`**: Installa i file di configurazione di esempio, necessari per il primo avvio.
* **`make install-webconf`**: Configura il server Apache per permettere l'accesso all'interfaccia web di Nagios.

---

### 6. Installazione dei Plugin e Gestione Firewall
Nagios richiede i **Plugin** per eseguire i controlli effettivi (come PING o il monitoraggio dei servizi). Senza di essi, il server non sarebbe in grado di raccogliere dati sullo stato degli host.

```bash
cd /tmp
# Scaricare l'ultima versione dei plugin (es. 2.4.11)
wget [https://nagios-plugins.org/download/nagios-plugins-2.4.11.tar.gz](https://nagios-plugins.org/download/nagios-plugins-2.4.11.tar.gz)
tar xzf nagios-plugins-2.4.11.tar.gz
cd nagios-plugins-2.4.11

# Configurazione e installazione dei plugin per l'utente nagios
./configure --with-nagios-user=nagios --with-nagios-group=nagios
make install
```

**Importante:** In ambiente di laboratorio, viene spesso disabilitato il firewall per semplificare i test di connettività iniziale. Tuttavia, in contesti di produzione, è fondamentale configurare correttamente le regole di accesso anziché disattivare completamente il servizio, garantendo così la sicurezza della rete.

```bash
# Disabilitazione del firewall (solo per ambiente lab)
systemctl stop firewalld
systemctl disable firewalld
```

### 7. Configurazione Password Web e Avvio Servizi
Per garantire la sicurezza dell'interfaccia di gestione, è necessario impostare una password per l'utente amministratore predefinito, **nagiosadmin**. Successivamente, si procede all'avvio del server web Apache e del servizio Nagios, abilitandone l'esecuzione automatica all'avvio del sistema.

```bash
# Impostazione della password per l'utente nagiosadmin
htpasswd -c /usr/local/nagios/etc/htpassword.users nagiosadmin

# Avvio e abilitazione dei servizi
systemctl start httpd
systemctl enable httpd
systemctl start nagios
systemctl enable nagios

# Verifica dello stato di esecuzione
systemctl status nagios
```

### 8. Configurazione degli Host
Le definizioni degli host e dei servizi che Nagios deve monitorare sono contenute nella directory `/usr/local/nagios/etc/objects/`. Una gestione ordinata di questi file è fondamentale per la scalabilità del sistema.

* **`localhost.cfg`**: Questo file viene utilizzato per configurare il monitoraggio del server locale (la macchina su cui è installato Nagios).
* **`host.cfg`**: È prassi consigliata creare file di configurazione separati per i nuovi host remoti. Questo permette di mantenere i parametri organizzati e facilita la manutenzione in infrastrutture IT complesse.

All'interno di questi file è possibile definire nuovi host specificando l'indirizzo IP, il nome simbolico e i servizi di controllo, come il comando `check_ping` per verificare la disponibilità della rete o la latenza dei pacchetti.



---

### 9. Verifica della Configurazione e Accesso Web
Prima di applicare qualsiasi modifica, è indispensabile convalidare la configurazione. Nagios fornisce uno strumento di verifica che analizza i file `.cfg` alla ricerca di errori di sintassi o riferimenti mancanti.

```bash
# Comando per verificare la correttezza della configurazione
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

# Se il controllo non rileva errori, riavviare il servizio per rendere effettive le modifiche
systemctl restart nagios
```

Dopo il riavvio, è possibile accedere alla console di monitoraggio tramite browser digitando l'indirizzo IP del server seguito dal percorso `/nagios`:

URL di esempio: `http://192.168.100.161/nagios`

---

# Hardening del Sistema Operativo Linux

L'**hardening** (irrobustimento) è il processo di messa in sicurezza di un sistema informatico attraverso la riduzione della sua superficie di attacco. In ambito professionale, la capacità di rendere un sistema Linux meno vulnerabile ad attacchi e virus è una delle competenze più richieste.

Di seguito vengono analizzate le strategie fondamentali per rendere sicuro un sistema operativo Linux.

---

## 1. Gestione degli Account Utente
La sicurezza inizia dal controllo degli accessi e dall'identità degli utenti.

* **Convenzione sui nomi (Naming Convention):** Si raccomanda di evitare nomi utente standard o facilmente prevedibili (come `admin`, `oracle` o `root` per servizi specifici). L'uso di nomi personalizzati o meno comuni riduce l'efficacia degli attacchi brute-force.
* **Gestione degli UID:** Per standard industriale, è preferibile assegnare UID (User ID) superiori a **10.000** per gli utenti fisici, rendendo più difficile indovinare gli identificativi di sistema.
* **Analisi degli utenti:** È possibile visualizzare tutti gli account di sistema consultando il file:
    ```bash
    cat /etc/passwd
    ```

### Politiche delle Password
È fondamentale definire criteri di scadenza e complessità delle password.
* **Comando `chage`:** Permette di visualizzare e modificare la "vecchiaia" della password di un utente.
    ```bash
    chage -l <username>   # Visualizza le informazioni di scadenza
    ```
* **File di configurazione:**
    * `/etc/shadow`: Contiene le password cifrate e i parametri di scadenza.
    * `/etc/login.defs`: Definisce i valori predefiniti per l'intero sistema (es. lunghezza minima della password, si raccomanda almeno **12-13 caratteri**).
    * `/etc/pam.d/system-auth`: Configurazione avanzata dei moduli di autenticazione (PAM).

---

## 2. Rimozione di Pacchetti e Servizi Superflui
Un sistema con molti software installati è intrinsecamente più vulnerabile.

* **Rimozione pacchetti:** Ogni pacchetto non necessario deve essere rimosso. 
    ```bash
    rpm -qa | wc -l         # Conta i pacchetti installati
    rpm -e <nome_pacchetto> # Rimuove un pacchetto (attenzione alle dipendenze)
    ```
* **Stop ai servizi inutilizzati:** Se un server non deve erogare un servizio specifico (es. NFS su un web server), tale servizio va interrotto e disabilitato.
    ```bash
    systemctl -a            # Elenca tutti i servizi
    systemctl stop <servizio>
    systemctl disable <servizio>
    ```

---

## 3. Monitoraggio delle Porte in Ascolto
Occorre verificare regolarmente quali porte sono aperte per accettare traffico in entrata.

```bash
netstat -tunlp
```

Se si rileva un servizio attivo su una porta non necessaria (es. porta 53/DNS su un sistema che non è un server DNS), il relativo processo deve essere terminato.

---

## 4. Sicurezza di SSH (Secure Shell)
Il servizio SSH rappresenta il principale vettore di accesso remoto e deve essere protetto modificando il file di configurazione `/etc/ssh/sshd_config`.

* **Cambio della porta:** Si raccomanda di spostare il servizio dalla porta standard **22** a una porta personalizzata (es. **1022**). Questo riduce drasticamente il numero di attacchi automatizzati.
* **Disabilitazione dell'accesso Root:** È fondamentale impostare il parametro `PermitRootLogin no`. In questo modo, l'accesso diretto come superutente viene inibito; gli amministratori dovranno autenticarsi con il proprio account personale e successivamente acquisire i privilegi necessari. Questa pratica garantisce la tracciabilità delle azioni nei log di sistema, associando ogni operazione a un utente specifico.

---

## 5. Firewall e Sicurezza di Rete
Il firewall agisce come un guardiano di sistema, filtrando il traffico in entrata e in uscita in base a regole prestabilite.

* **Versioni e strumenti:** Nei sistemi più recenti (RHEL 7 e successivi) si utilizza **Firewalld**, gestibile tramite il comando `firewall-cmd` o l'interfaccia grafica `firewall-config`. Nelle versioni precedenti si utilizzava **iptables**.
* **Politica restrittiva:** Un firewall correttamente configurato dovrebbe bloccare tutto il traffico in ingresso per impostazione predefinita, autorizzando esclusivamente le porte e i protocolli necessari per le funzioni specifiche del server.

---

## 6. SELinux (Security-Enhanced Linux)
Sviluppato dalla **NSA**, SELinux implementa un sistema di controllo degli accessi obbligatorio (**MAC**) che agisce a livello di kernel. A differenza dei permessi standard, SELinux limita le capacità dei processi e delle applicazioni basandosi su contesti di sicurezza predefiniti.

### Stati operativi di SELinux:
1.  **Enforcing:** Le policy di sicurezza sono attive e ogni violazione viene bloccata e registrata.
2.  **Permissive:** Il sistema non blocca le azioni, ma registra ogni violazione della policy (stato utile per il debug).
3.  **Disabled:** Il meccanismo di sicurezza è completamente disattivato.

**Verifica dello stato e dei contesti:**
```bash
# Verifica lo stato attuale di SELinux
sestatus

# Visualizza il contesto di sicurezza di un file specifico
stat <nome_file>
```

### 7. Aggiornamenti e Patch di Sicurezza
Il mantenimento del sistema operativo costantemente aggiornato è un requisito imprescindibile per proteggersi dalle vulnerabilità scoperte di recente. Non è sempre necessario aggiornare ogni singolo pacchetto applicativo, ma le patch di sicurezza devono essere applicate con la massima priorità.



* **Patch di sicurezza:** È prioritario applicare gli aggiornamenti classificati come "security patches" non appena vengono rilasciati dai vendor (Red Hat, CentOS, ecc.).
* **Monitoraggio attivo:** Si raccomanda l'iscrizione ai canali ufficiali di notifica e alle mailing list di sicurezza dei vendor. Rimanere informati sui nuovi bug e sulle vulnerabilità **CVE** (Common Vulnerabilities and Exposures) permette di intervenire prima che queste possano essere sfruttate.
* **Automazione:** Ove possibile, si consiglia di automatizzare il controllo degli aggiornamenti critici, mantenendo però un controllo manuale sulla fase di applicazione in ambienti di produzione per evitare regressioni.

```bash
# Esempio di comando per verificare e installare i soli aggiornamenti di sicurezza
yum update --security
```

Conclusioni sull'Hardening

L'applicazione sistematica di questi punti permette di trasformare una configurazione Linux standard in un sistema "indurito" e pronto per l'esposizione in reti aziendali o pubbliche. Un buon amministratore di sistema non si limita a installare il software, ma ne riduce proattivamente la superficie di attacco.

---

# Installazione di OpenLDAP su Linux

Questa lezione approfondisce l'implementazione di **OpenLDAP**, un servizio fondamentale per la gestione centralizzata degli utenti, simile ad Active Directory di Microsoft ma basato su software open source.

---

## 1. Cos'è OpenLDAP?
**OpenLDAP** è un'implementazione open source del protocollo **LDAP** (*Lightweight Directory Access Protocol*). Viene sviluppato dal progetto OpenLDAP ed è ampiamente utilizzato per gestire cataloghi di informazioni, come dati di contatto e credenziali di accesso degli utenti, in modo centralizzato.



### Caratteristiche principali:
* **Protocollo Internet:** Permette a programmi di posta e altre applicazioni di consultare informazioni sugli utenti da un server remoto.
* **Licenza:** Rilasciato sotto la *OpenLDAP Public License*.
* **Compatibilità:** Disponibile per le principali distribuzioni Linux, ma anche per AIX, Android, HP-UX, macOS, Solaris e Windows.
* **Vantaggi:** Essendo gratuito, rappresenta l'alternativa ideale ad Active Directory in ambienti con migliaia di utenti distribuiti su numerosi server.

---

## 2. Gestione del Servizio: SLAPD
Una volta completata l'installazione, il demone (servizio) che gestisce le richieste LDAP è denominato **slapd**.

### Comandi principali per la gestione del servizio:
* **Avvio:** `systemctl start slapd`
* **Abilitazione al boot:** `systemctl enable slapd`
* **Riavvio:** `systemctl restart slapd`
* **Verifica dello stato:** `systemctl status slapd`

---

## 3. Directory delle Configurazioni
I file di configurazione principali, dove è possibile modificare il comportamento del server, si trovano nel seguente percorso:
` /etc/openldap/slapd.d/`

---

## 4. Procedura di Installazione

### Verifica della connettività
Prima di procedere, è indispensabile verificare che la macchina Linux abbia accesso a Internet per scaricare i pacchetti necessari. Si può utilizzare il comando `ping`:
```bash
ping google.com
```

Se il sistema riceve risposta (es. **64 bytes from...**), la connettività è garantita.

### Installazione dei pacchetti
Per installare tutti i pacchetti relativi a OpenLDAP, si utilizza il gestore di pacchetti `yum` (o `dnf`). Si consiglia l'uso dei caratteri jolly per assicurarsi di includere tutti i client, le librerie e il server necessari:

```bash
# Installazione di tutti i pacchetti relativi a OpenLDAP
yum install *openldap* -y
```

Il sistema mostrerà un riepilogo della transazione, indicando la dimensione del download e i pacchetti coinvolti. Dopo aver confermato l'operazione, il messaggio **"Complete!"** certificherà la corretta installazione dei componenti sul sistema.

---

## 5. Verifica del Processo e Integrazione
Dopo l'installazione, è necessario verificare che il servizio sia attivo e comprendere come il sistema operativo Linux interagisca con esso per l'autenticazione degli utenti.

### Verifica del processo
Oltre alla gestione tramite `systemctl`, è possibile confermare che il demone sia effettivamente in esecuzione consultando la tabella dei processi di sistema:
```bash
# Verifica del processo slapd tramite grep
ps -ef | grep slapd
```

### Configurazione dell'ordine di ricerca (NSS)
Il file `/etc/nsswitch.conf` (Name Service Switch) è il componente critico che determina l'ordine con cui il sistema operativo consulta i diversi database per recuperare informazioni su utenti, gruppi e host.

Nel contesto di OpenLDAP, è necessario intervenire sulla riga relativa a `passwd` (e opzionalmente `group` e `shadow`) per istruire il sistema a interrogare il server di directory:

* **files:** Rappresenta il database locale tradizionale (es. `/etc/passwd`). È prioritario per garantire l'accesso agli utenti di sistema anche in caso di problemi di rete.
* **ldap:** Indica al sistema di interrogare il servizio OpenLDAP qualora l'utente non sia presente nei file locali.



> **Nota di architettura:** In un'infrastruttura di produzione, OpenLDAP viene installato su un server dedicato che funge da Directory Server centrale. I server client vengono configurati per puntare a questo nodo centrale, permettendo agli amministratori di gestire migliaia di account da un unico punto, anziché operare localmente su ogni singola macchina.

---

# Risoluzione dei Problemi di Rete: Il Comando Traceroute

Questa lezione approfondisce l'utilizzo di uno degli strumenti più importanti per il monitoraggio e la risoluzione dei problemi di rete (*troubleshooting*): **traceroute**.

---

### Cos'è Traceroute?
Il comando **traceroute** viene utilizzato in ambiente Linux per mappare il percorso che un pacchetto di informazioni compie dalla sorgente (il proprio computer) alla destinazione finale (un server o un sito web). 

Ogni tappa intermedia di questo viaggio è chiamata **hop** (salto) e rappresenta un router o un server che processa il traffico durante il transito.



---

### Funzioni Principali
L'analisi del tracciamento del traffico permette di:
* **Individuare perdite di dati:** Identificare esattamente in quale nodo la comunicazione si interrompe, segnalando potenzialmente un server o un router guasto.
* **Identificare rallentamenti:** Misurare i tempi di risposta per ogni singolo salto, permettendo di localizzare i colli di bottiglia che influenzano negativamente le prestazioni della rete.
* **Mappare il percorso:** Visualizzare attraverso quali gateway e DNS passa il traffico prima di raggiungere la destinazione.

> **Nota:** Questo comando è presente in quasi tutti i sistemi operativi. In **Windows**, il comando equivalente è `tracert`.

---

### Sintassi e Utilizzo
La sintassi di base è molto semplice:
```bash
traceroute [destinazione]
```

La destinazione può essere un **nome host** (es. `google.com`), un **indirizzo IP** o una **URL**. 

*Se il sistema non riesce a risolvere il nome host tramite DNS, è necessario utilizzare direttamente l'indirizzo IP della macchina di destinazione.*

---

### Esempio Pratico
Per testare il comando in un ambiente reale, è possibile seguire questa procedura:

1.  **Verifica connettività:** Prima di iniziare, si esegue un `ping` per assicurarsi che l'interfaccia di rete sia attiva.
2.  **Esecuzione:**
    ```bash
    traceroute google.com
    ```

**Analisi dell'output:**
* **Primo Hop:** Corrisponde solitamente al router locale o al gateway aziendale (es. `192.168.1.1`). Rappresenta il "punto di uscita" della propria rete privata.
* **Hop intermedi:** Mostrano i router del fornitore di servizi (ISP) e i vari nodi di transito internazionali. Se si notano tre asterischi (`* * *`), significa che il nodo non ha risposto entro il tempo limite (spesso per motivi di sicurezza/firewall).
* **Destinazione finale:** L'ultimo salto indica il raggiungimento dell'IP risolto per l'host richiesto.



---

### Strumenti Correlati: Il Gateway
Il motivo per cui il traffico passa per determinati nodi dipende dalla tabella di instradamento del sistema. Per identificare il proprio gateway predefinito (il primo salto), si utilizza:
```bash
netstat -rnv
```

Oppure il comando più moderno:
```bash
ip route show
```

Questi comandi mostrano la rotta `default`, ovvero l'indirizzo IP (es. `192.168.1.1`) a cui vengono inviati tutti i pacchetti destinati all'esterno della rete locale.

---

# Come visualizzare file immagine da riga di comando in Linux

È comune saper gestire file di testo tramite terminale utilizzando strumenti come `vi`, `cat`, `more` o `head`. Tuttavia, sorge spesso il dubbio su come sia possibile visualizzare un **file immagine** senza ricorrere esclusivamente all'interfaccia grafica (GUI).

---

## 1. Cos'è un file immagine?
I formati di file immagine sono standard per organizzare e memorizzare immagini digitali. I dati possono essere salvati in formato:
* **Raster:** Dati digitali che possono essere visualizzati su monitor o stampati.
* **Compresso/Non compresso:** Per ottimizzare lo spazio o mantenere la qualità.
* **Vettoriale:** Basato su equazioni matematiche per la scalabilità.

---

## 2. Requisiti e Limitazioni
Sebbene in un ambiente desktop sia sufficiente un doppio clic sull'icona del file, l'apertura tramite comando richiede alcune precisazioni:
* **Ambiente grafico attivo:** Il comando funzionerà solo se si è loggati direttamente nell'interfaccia grafica di Linux. 
* **SSH/PuTTY:** Se si accede al server in remoto tramite terminale testuale (SSH), il comando non potrà mostrare l'immagine poiché i terminali testuali non supportano nativamente il rendering grafico.

---

## 3. Installazione di ImageMagick
Per visualizzare immagini da riga di comando, è necessario installare un pacchetto software specifico chiamato **ImageMagick**.

### Procedura di installazione:
1.  **Ottenere i privilegi di Root:**
    ```bash
    su - 
    # oppure
    sudo -i
    ```
2.  **Verificare la connettività Internet:**
    ```bash
    ping google.com
    ```
3.  **Installare il pacchetto:**
    ```bash
    yum install ImageMagick -y
    ```
    *(Nota: su distribuzioni più recenti si può usare `dnf` invece di `yum`)*.

---

## 4. Visualizzazione dell'immagine
Una volta installato il software, il comando da utilizzare è `display`.

### Esempio pratico:
Supponendo di avere un'immagine scaricata nella cartella `Desktop`, i passaggi sono i seguenti:

1.  **Navigare nella cartella corretta:**
    ```bash
    cd ~/Desktop
    ```
2.  **Eseguire il comando di visualizzazione:**
    ```bash
    display nome_file_immagine.jpg
    ```



Il comando `display` aprirà una finestra dedicata all'interno dell'interfaccia grafica mostrando il contenuto del file.

---

## 5. Considerazioni finali
Sebbene nell'amministrazione di sistema aziendale (dove prevalgono i server *headless* senza monitor) l'apertura di immagini da terminale sia un'operazione poco frequente, conoscere questo strumento è utile per completare la propria padronanza della riga di comando Linux e per gestire workstation grafiche.

---

# Configurazione e Sicurezza di SSH (Secure Shell)

Questa lezione esplora il funzionamento di **SSH** e le migliori pratiche per renderlo sicuro. SSH è lo standard per l'accesso remoto ai sistemi Linux e, data la sua importanza, deve essere configurato con attenzione.

---

## 1. Cos'è SSH?
**SSH** sta per **Secure Shell**. 

* **Shell:** Fornisce l'interfaccia verso il sistema Linux. Traduce i comandi dell'utente per il **Kernel**, che a sua volta gestisce l'hardware. Esistono diversi tipi di shell (Bash, Csh, Ksh).
* **Ambiente di lavoro:** Quando si effettua l'accesso, il sistema fornisce un prompt. Solitamente, il simbolo `$` indica un utente regolare, mentre il simbolo `#` indica l'utente root.
* **OpenSSH:** È il pacchetto software che implementa il protocollo SSH. Il demone (il servizio che gira in background) si chiama **sshd**.
* **Porta predefinita:** SSH utilizza normalmente la porta **22**.



---

## 2. Pratiche di Sicurezza Essenziali
Sebbene SSH sia crittografato e sicuro per natura, un amministratore di sistema deve applicare configurazioni aggiuntive per ridurre la superficie di attacco.

> **Raccomandazione:** Prima di modificare i file di configurazione, creare sempre una copia di backup:
> `cp /etc/ssh/sshd_config /etc/ssh/sshd_config.orig`

### A. Configurazione del Timeout di Inattività
Per evitare che sessioni SSH lasciate incustodite rimangano aperte (e quindi vulnerabili), è possibile impostare un timeout automatico.

1.  Modificare il file `/etc/ssh/sshd_config`.
2.  Aggiungere o modificare le seguenti righe:
    * `ClientAliveInterval 600` (imposta 600 secondi, ovvero 10 minuti).
    * `ClientAliveCountMax 0` (chiude la sessione se il client non risponde dopo l'intervallo).
3.  Riavviare il servizio: `systemctl restart sshd`.

### B. Disabilitare il Login dell'Utente Root
È una pratica fondamentale impedire l'accesso diretto come utente root per prevenire attacchi di forza bruta sull'account amministrativo principale.

* Nel file `sshd_config`, cercare il parametro `PermitRootLogin` e impostarlo su **no**.
* In questo modo, gli utenti dovranno collegarsi con un account standard e poi acquisire privilegi superiori (tramite `sudo` o `su`).

### C. Disabilitare le Password Vuote
Per una maggiore sicurezza, è necessario impedire l'accesso remoto ad account che non hanno una password impostata.

* Cercare il parametro `PermitEmptyPasswords` e assicurarsi che sia impostato su **no** (e non sia commentato).

### D. Limitare l'Accesso agli Utenti
Anziché permettere l'accesso SSH a chiunque, è possibile specificare una lista esclusiva di utenti autorizzati.

* Aggiungere in fondo al file di configurazione: `AllowUsers utente1 utente2`.
* Tutti gli utenti non presenti in questa lista verranno automaticamente respinti.

### E. Cambiare la Porta Predefinita
Poiché la maggior parte degli attacchi automatizzati prende di mira la porta 22, spostare il servizio su una porta diversa (es. **2224**) può rendere il sistema meno visibile.

* Modificare il parametro `Port 22` con la porta desiderata.
* **Attenzione:** Una volta cambiata la porta, sarà necessario specificarla nel client SSH (es. PuTTY) per potersi connettere.

---

## 3. Riepilogo Comandi Utili

| Azione | Comando |
| :--- | :--- |
| **Riavvio del servizio** | `systemctl restart sshd` |
| **Modifica configurazione** | `vi /etc/ssh/sshd_config` |
| **Verifica porta attiva** | `netstat -tulpn \| grep sshd` |

---

# Accesso Remoto tramite Chiavi SSH (Senza Password)

Questa lezione illustra come configurare l'accesso tra due macchine Linux utilizzando le **chiavi SSH**, eliminando la necessità di inserire manualmente le credenziali a ogni connessione.

---

## 1. Perché utilizzare le chiavi SSH?

L'accesso senza password è fondamentale in due scenari principali:
* **Accessi ripetitivi:** Se un amministratore deve connettersi a un server remoto molte volte al giorno, l'inserimento continuo di utente e password risulta inefficiente.
* **Automazione tramite Script:** Questa è la ragione più importante. Gli script eseguiti su un "Server A" che devono compiere operazioni su un "Server B" richiedono un'autenticazione non interattiva (ovvero che non richieda l'intervento umano per digitare la password).

---

## 2. Concetti Fondamentali e Architettura

L'autenticazione avviene tramite una coppia di chiavi generate a livello di utente (utente standard o root).

* **Macchina Client:** Il computer da cui parte la connessione.
* **Macchina Server (Remota):** Il computer a cui ci si vuole connettere.

Il processo prevede la generazione di una "serratura" (chiave pubblica) da inviare al server e di una "chiave fisica" (chiave privata) da conservare sul client.



---

## 3. Procedura Operativa

La configurazione si articola in tre passaggi principali eseguiti sulla macchina **Client**:

### Passaggio 1: Generazione delle chiavi
Si utilizza il comando `ssh-keygen` per creare la coppia di chiavi RSA.
```bash
ssh-keygen
```

* **Locazione:** Il sistema suggerirà di salvare la chiave in `~/.ssh/id_rsa`. Si consiglia di premere **Invio** per accettare il percorso predefinito.
* **Passphrase:** Per scopi di automazione o test in laboratorio, è possibile lasciare il campo vuoto premendo **Invio**. In ambienti di produzione aziendale, è invece caldamente raccomandato impostarne una per aggiungere un ulteriore livello di protezione.

### Passaggio 2: Copia della chiave sul Server
Una volta generate le chiavi, la chiave pubblica deve essere trasferita sul server remoto affinché quest'ultimo possa riconoscere e autorizzare il client.
```bash
ssh-copy-id root@<IP_DEL_SERVER>
```

* **Durante questa operazione**, verrà richiesta la password dell'utente root del server per l'ultima volta. Il sistema provvederà a inserire automaticamente la chiave nel file `/root/.ssh/authorized_keys` della macchina remota.



### Passaggio 3: Verifica dell'accesso
Completata la copia, è possibile testare la connessione. Se la configurazione è corretta, l'accesso avverrà senza alcuna richiesta di password:

```bash
ssh root@<IP_DEL_SERVER>
# Oppure utilizzando il flag -l
ssh -l root <IP_DEL_SERVER>
```

## 4. Riepilogo Comandi e File Chiave

| Strumento / File | Descrizione |
| :--- | :--- |
| `ssh-keygen` | Genera la coppia di chiavi (pubblica e privata) sul client. |
| `ssh-copy-id` | Trasferisce la chiave pubblica sul server remoto per autorizzare l'accesso. |
| `~/.ssh/id_rsa` | **Chiave Privata**: Da custodire sul client (non deve essere mai condivisa). |
| `~/.ssh/id_rsa.pub` | **Chiave Pubblica**: Da copiare sui server a cui si desidera accedere. |
| `authorized_keys` | File sul server che elenca le chiavi pubbliche autorizzate all'accesso. |



> **Nota di sicurezza**: La chiave privata rappresenta l'identità digitale dell'utente. In caso di smarrimento o compromissione, chiunque ne entri in possesso potrà accedere ai server autorizzati senza dover conoscere la password dell'account. È fondamentale che i permessi del file siano impostati correttamente (tipicamente `600`).

---

# Amministrazione di Server Linux con Cockpit

**Cockpit** è uno strumento di amministrazione server sponsorizzato da Red Hat, progettato per offrire un'interfaccia moderna, intuitiva e basata sul web per la gestione dei sistemi Linux.

---

## 1. Caratteristiche Principali
Cockpit permette di amministrare i server in modo visivo e integrato. Tra le sue funzionalità principali si annoverano:
* **Monitoraggio delle risorse:** Visualizzazione in tempo reale dell'uso di CPU, memoria e disco.
* **Gestione account:** Creazione, rimozione e modifica degli utenti.
* **Gestione di rete:** Configurazione di interfacce, bond, team, bridge e VLAN.
* **Manutenzione:** Arresto o riavvio del sistema, installazione di aggiornamenti software e gestione dei servizi (`systemd`).
* **Terminale integrato:** Accesso alla riga di comando direttamente dal browser.



---

## 2. Installazione e Configurazione

### Verifica della connettività
Prima di procedere, è necessario assicurarsi che il server possa raggiungere i repository ufficiali tramite una connessione Internet. È possibile verificare la connettività con il comando:
```bash
ping -c 4 [www.google.com](https://www.google.com)
```

### Installazione del pacchetto
L'installazione deve essere eseguita con privilegi di amministratore (root) o utilizzando il comando `sudo`.

* **Sistemi Red Hat 8 / CentOS 8 (e versioni successive):** Cockpit è spesso installato per impostazione predefinita. Qualora non lo fosse, è possibile procedere con il comando:
    ```bash
    dnf install cockpit -y
    ```
* **Sistemi CentOS 7 / Red Hat 7:** Il pacchetto è disponibile opzionalmente e può essere installato dai repository ufficiali:
    ```bash
    yum install cockpit -y
    ```
* **Sistemi Ubuntu / Debian:** Si utilizza il gestore di pacchetti `apt`:
    ```bash
    apt-get install cockpit
    ```

Prima di avviare il servizio, è possibile verificare la corretta installazione del pacchetto tramite il comando `rpm -qa | grep cockpit` (per sistemi basati su RPM).

### Gestione del servizio
Una volta installato il pacchetto, il servizio associato deve essere avviato e configurato per l'esecuzione automatica al boot del sistema:

```bash
# Avvio del servizio
systemctl start cockpit

# Abilitazione all'avvio automatico
systemctl enable cockpit

# Verifica dello stato operativo
systemctl status cockpit
```

## 3. Accesso all'Interfaccia Web

L'applicazione Cockpit è accessibile tramite protocollo HTTPS sulla porta **9090**. Per collegarsi, è necessario utilizzare un browser web e inserire l'indirizzo IP del server seguito dal numero della porta:

`https://<IP_DEL_SERVER>:9090`

> **Configurazione del Firewall:** Se la connessione viene rifiutata, è probabile che il firewall del server stia bloccando il traffico sulla porta 9090. Per risolvere il problema, è possibile autorizzare il servizio in modo permanente:
> ```bash
> firewall-cmd --permanent --add-service=cockpit
> firewall-cmd --reload
> ```
> In alternativa, solo per test temporanei, si può arrestare il firewall con il comando `systemctl stop firewalld`.

Al primo accesso, il browser visualizzerà un avviso di sicurezza poiché il certificato SSL è autofirmato dal server. È necessario cliccare su **Avanzate** e selezionare **Accetta il rischio e continua**.

---

## 4. Panoramica delle Funzionalità

Una volta effettuato il login con le credenziali di sistema (utente root o utente con privilegi sudo), l'interfaccia mette a disposizione diversi strumenti organizzati in un menu laterale:

* **Sistema:** Consente di monitorare lo stato di salute generale, l'utilizzo di CPU e memoria, e di eseguire operazioni di spegnimento o riavvio.
* **Log:** Offre una visualizzazione dettagliata dei messaggi di sistema, con filtri basati sulla gravità degli eventi (errori, emergenze, informazioni).
* **Archiviazione:** Mostra i dischi installati, le partizioni e i punti di montaggio. Permette inoltre di monitorare le prestazioni di lettura e scrittura.
* **Rete:** Permette di configurare le interfacce, creare bond, team, bridge o aggiungere VLAN senza dover agire sui file di configurazione manuali.
* **Account:** Facilita la gestione degli utenti locali, permettendo la creazione o la rimozione di account in pochi clic.
* **Servizi:** Elenca tutti i servizi `systemd`, consentendo di avviarne o arrestarne l'esecuzione e di modificarne l'abilitazione al boot.
* **Aggiornamenti Software:** Permette di ricercare e installare aggiornamenti o singole patch di sicurezza, in modo simile ai comandi `yum update` o `dnf upgrade`.
* **Diagnostica:** Include strumenti per la creazione di report SOS (SOS Report) e la gestione del Kernel Dump (kdump).
* **Terminale:** Fornisce una console shell interattiva all'interno del browser, identica a una sessione SSH tradizionale.

---

### Conclusione
Cockpit rappresenta una soluzione ideale per chi desidera gestire server Linux in modo moderno e visivo, mantenendo al contempo la potenza della riga di comando sempre a disposizione grazie al terminale integrato.

---

# Introduzione ai Firewall in Ambiente Linux

Il **firewall** rappresenta la prima linea di difesa per la sicurezza di un sistema. Può essere immaginato come un muro tagliafuoco, una guardia o uno scudo che, basandosi su un insieme di regole predefinite, decide quale traffico dati può entrare o uscire da un server.



---

## 1. Tipologie di Firewall
Esistono due categorie principali di firewall in ambito IT:
* **Firewall Hardware:** Dispositivi dedicati (appliance) solitamente gestiti dai team di rete per proteggere l'intero perimetro aziendale.
* **Firewall Software:** Applicazioni che girano direttamente sul sistema operativo (Linux, Windows, ecc.). Questo modulo si focalizza sulla gestione del firewall software in ambiente Linux.

---

## 2. Il Modello di Funzionamento
Il firewall analizza i pacchetti di dati in transito. Ad esempio, se il **Server A** tenta di connettersi al **Server B** tramite il protocollo **SSH (porta 22)**:
1.  Il firewall intercetta la richiesta.
2.  Verifica se esiste una regola che autorizza il Server A sulla porta 22.
3.  In caso positivo, la connessione viene stabilita; in caso negativo, il pacchetto viene rifiutato o scartato.



---

## 3. Strumenti di Gestione: Iptables e Firewalld
In Linux esistono principalmente due strumenti per gestire le regole del firewall. Non è raccomandato eseguirli contemporaneamente.

### A. Iptables
È lo strumento storico e più diffuso. Si basa su una struttura gerarchica composta da:
* **Tabelle:** (es. *filter*, *nat*) per processare i pacchetti in modi specifici.
* **Catene (Chains):** Punti di ispezione del traffico. Le principali sono:
    * **INPUT:** Traffico in entrata verso il server.
    * **OUTPUT:** Traffico in uscita dal server.
    * **FORWARD:** Traffico instradato verso un altro dispositivo.
* **Target:** L'azione da compiere se la regola viene soddisfatta:
    * **ACCEPT:** Accetta la connessione.
    * **REJECT:** Rifiuta la connessione inviando una risposta di errore.
    * **DROP:** Scarta il pacchetto senza fornire alcuna risposta al mittente.



#### Operazioni con Iptables
Per utilizzare `iptables`, è necessario disattivare `firewalld`:
```bash
systemctl stop firewalld
systemctl disable firewalld
systemctl mask firewalld  # Impedisce l'avvio accidentale
```

#### Installazione e gestione del servizio:
Per utilizzare `iptables`, è necessario disattivare preventivamente `firewalld` per evitare conflitti tra i due strumenti:
```bash
systemctl stop firewalld
systemctl disable firewalld
systemctl mask firewalld  # Impedisce l'avvio accidentale da parte di altri programmi
```

Qualora il pacchetto non fosse presente nel sistema, si procede all'installazione dei componenti necessari tramite il gestore di pacchetti `yum`. È fondamentale assicurarsi che la macchina disponga di connettività internet per raggiungere i repository ufficiali.

```bash
# Verifica della presenza del pacchetto
rpm -qa | grep iptables-services

# Installazione del pacchetto dei servizi iptables
yum install iptables-services -y
```

Dopo aver completato l'installazione, il servizio deve essere avviato e configurato per l'attivazione automatica al boot del sistema:

```bash
# Avvio del servizio iptables
systemctl start iptables

# Abilitazione all'avvio automatico
systemctl enable iptables

# Verifica che il servizio sia attivo e privo di errori
systemctl status iptables
```

Per l'analisi delle regole predefinite o per la gestione della configurazione iniziale, si utilizzano i seguenti comandi:

* **Verifica delle regole:** Tramite il comando `iptables -L` è possibile visualizzare le regole caricate per impostazione predefinita. Generalmente, il sistema include autorizzazioni per il traffico SSH e per l'interfaccia di loopback.
* **Svuotamento della tabella (Flush):** Qualora si renda necessario eliminare ogni filtro esistente per definire una configurazione personalizzata da zero, si utilizza il comando `iptables -F`.

Una volta eseguito il comando di flush, l'output mostrerà le tre catene fondamentali del filtraggio pacchetti (**INPUT**, **FORWARD** e **OUTPUT**) prive di qualsiasi istruzione associata.



---

### B. Gestione tramite Firewalld

`Firewalld` rappresenta lo strumento di gestione predefinito per le distribuzioni Linux più moderne. Pur operando sui medesimi principi di filtraggio, introduce un'astrazione basata sulle **Zone**, che permette di raggruppare regole diverse in base al livello di fiducia della rete connessa.

#### Configurazione e Avvio
Poiché la gestione simultanea di più interfacce firewall è sconsigliata per evitare conflitti logici, è necessario assicurarsi che `iptables` sia arrestato e mascherato prima di procedere con `firewalld`:

```bash
systemctl stop iptables
systemctl disable iptables
systemctl mask iptables
```

Dopo aver neutralizzato altri servizi concorrenti, si procede all'attivazione di firewalld. Nel caso in cui il servizio risulti mascherato, occorre preventivamente sbloccarlo:

```bash
# Rimozione del mascheramento e avvio del servizio
systemctl unmask firewalld
systemctl start firewalld
systemctl enable firewalld

# Verifica dello stato del processo
systemctl status firewalld
```

#### Analisi dei Servizi e delle Zone
L'interazione con questo strumento avviene principalmente tramite l'utility `firewall-cmd`. Le operazioni fondamentali per il monitoraggio e la gestione includono:

* **Visualizzazione della configurazione attiva:** Il comando `firewall-cmd --list-all` permette di consultare i dettagli della zona predefinita (solitamente denominata *public*), inclusi i servizi già autorizzati (come `ssh` e `dhcpv6-client`) e le eventuali porte aperte.
* **Consultazione dei servizi predefiniti:** Tramite `firewall-cmd --get-services` è possibile elencare tutti i servizi che il firewall è in grado di riconoscere nativamente. Questi servizi sono definiti tramite file in formato XML che contengono le informazioni relative alle porte e ai protocolli necessari (es. HTTP sulla porta 80, FTP sulla porta 21).
* **Aggiornamento del sistema:** Ogni volta che viene apportata una modifica alla configurazione, è necessario eseguire il comando `firewall-cmd --reload`. Questa operazione permette al processo del firewall di caricare le nuove informazioni senza interrompere le connessioni già stabilite.

---

### C. Esempi Pratici di Amministrazione
Per acquisire dimestichezza con la gestione del traffico, è utile analizzare il comportamento delle **Zone**. In `firewalld`, ogni interfaccia di rete è assegnata a una zona specifica che ne determina il livello di sicurezza.

* **Elenco delle zone disponibili:** Per visualizzare tutte le zone configurate nel sistema, si utilizza il comando:
    ```bash
    firewall-cmd --get-zones
    ```
* **Verifica della zona predefinita:** Per conoscere quale profilo viene applicato automaticamente alle nuove interfacce:
    ```bash
    firewall-cmd --get-default-zone
    ```

### Conclusione
Sia `iptables` che `firewalld` operano sui medesimi principi di filtraggio pacchetti (tabelle, catene, regole e target), ma offrono interfacce di gestione differenti. Mentre `iptables` garantisce un controllo granulare e diretto, `firewalld` semplifica l'amministrazione quotidiana attraverso l'uso di astrazioni logiche come zone e servizi.

---

## Ottimizzazione delle Prestazioni del Sistema Linux

Sebbene i sistemi Linux siano generalmente ottimizzati per impostazione predefinita, è possibile apportare ulteriori regolazioni basate sulle prestazioni del sistema o sui requisiti specifici delle applicazioni. In questa sezione viene analizzato come ottimizzare il sistema selezionando profili gestiti dal demone **Tuned** e come gestire la priorità dei processi tramite i comandi **nice** e **renice**.

---

### 1. Il Demone Tuned
**Tuned** è un servizio di sistema (`systemd`) utilizzato per monitorare e adattare le impostazioni dei componenti hardware e software per migliorare le prestazioni o il risparmio energetico.

* **Definizione di Demone:** Un processo che viene eseguito continuamente in background.
* **Funzionamento:** Tuned applica impostazioni di sistema all'avvio del servizio o alla selezione di un nuovo profilo. Adatta automaticamente parametri di networking (es. durante il download di file ISO) o impostazioni di I/O (es. in caso di alta attività di lettura/scrittura su disco).
* **Disponibilità:** Installato di default su CentOS/Red Hat versioni 7, 8 e successive.

#### Profili Predefiniti comuni:
| Profilo | Scopo |
| :--- | :--- |
| **balanced** | Compromesso tra risparmio energetico e prestazioni. |
| **desktop** | Basato sul profilo *balanced*, ottimizza la risposta delle applicazioni interattive. |
| **throughput-performance** | Ottimizzato per il massimo throughput (volume di dati elaborati). |
| **latency-performance** | Ottimizzato per la bassa latenza (velocità di risposta). |
| **virtual-guest** | Ottimizzato per macchine eseguite come guest in ambienti virtuali. |
| **virtual-host** | Ottimizzato per sistemi che ospitano macchine virtuali. |

---

### 2. Gestione del Servizio Tuned

Per verificare la presenza del pacchetto e lo stato del servizio, si utilizzano i seguenti comandi:

```bash
# Verificare l'installazione del pacchetto
rpm -qa | grep tuned

# Installare il pacchetto (se assente)
yum install tuned -y  # o 'dnf install tuned' su sistemi recenti

# Gestione del servizio tramite systemctl
systemctl status tuned
systemctl start tuned
systemctl enable tuned
```

#### Amministrazione dei profili con `tuned-adm`:
Il comando principale per interagire con il demone e gestire le impostazioni di ottimizzazione è `tuned-adm`. Di seguito vengono elencate le operazioni più comuni:

* **Verifica del profilo attivo:** Per identificare quale profilo sia attualmente utilizzato dal sistema, si esegue:
    ```bash
    tuned-adm active
    ```
* **Elenco dei profili disponibili:** Per visualizzare tutti i profili predefiniti installati e pronti all'uso:
    ```bash
    tuned-adm list
    ```
* **Cambio del profilo:** Per passare a un profilo differente (ad esempio per ottimizzare un sistema desktop o un server dedicato al throughput), si utilizza la sintassi:
    ```bash
    tuned-adm profile <nome_profilo>
    # Esempio per impostare il profilo bilanciato:
    tuned-adm profile balanced
    ```
* **Suggerimento automatico:** Il demone è in grado di analizzare le caratteristiche del sistema (come la presenza di virtualizzazione) e consigliare il profilo più adatto tramite il comando:
    ```bash
    tuned-adm recommend
    ```
* **Disattivazione del servizio:** Qualora si preferisca gestire manualmente ogni parametro senza l'intervento del demone, è possibile disattivare ogni profilo attivo:
    ```bash
    tuned-adm off
    ```

---

### 3. Gestione della Priorità dei Processi: Nice e Renice

Un altro metodo per ottimizzare le prestazioni del sistema consiste nel dare priorità a determinati processi rispetto ad altri. Se un server dispone di una sola CPU, questa può eseguire un'unica computazione alla volta; gli altri processi devono attendere il proprio turno secondo una logica di pianificazione (spesso di tipo *round-robin*).

Attraverso i comandi **nice** e **renice**, è possibile istruire il sistema affinché conceda maggiore tempo di calcolo a processi critici (come un database Oracle) a scapito di quelli meno importanti.

#### Livelli di Priorità e Valori di Nice
La priorità può essere regolata su 40 livelli distinti:
* **Valore di Nice (NI):** Spazia da **-20** (massima priorità) a **19** (minima priorità).
* **Comportamento predefinito:** I processi ereditano normalmente un valore di nice pari a **0**.
* **Priorità Kernel (PR):** Il kernel Linux mappa i valori di nice dell'utente in un range di priorità interna che va da 0 a 139 (dove 0-99 è riservato ai processi real-time e 100-139 ai processi utente).

[Image: Diagramma di allineamento tra User Nice Level (-20 a 19) e Kernel Priority (0 a 139)]

#### Monitoraggio e Modifica
Per verificare la priorità dei processi in esecuzione, si può utilizzare il comando `top`, osservando le colonne **PR** e **NI**, oppure utilizzare una variante specifica del comando `ps`:
```bash
ps axo pid,comm,nice,cls --sort=-nice
```

#### Per intervenire direttamente sulle priorità:
La gestione dei livelli di priorità può essere effettuata sia al momento del lancio di un nuovo processo, sia su un'attività già in esecuzione nel sistema.

1.  **Avvio di un nuovo processo con priorità specifica (`nice`):**
    Per assegnare una priorità personalizzata a un comando prima della sua esecuzione, si utilizza il comando `nice` seguito dal valore desiderato. Ad esempio, per avviare il monitor di sistema `top` con una priorità molto alta (valore -15), si esegue:
    ```bash
    # Sintassi: nice -n <valore> <comando>
    nice -n -15 top
    ```
    In questo modo, il kernel assegnerà al processo una priorità di sistema (PR) più bassa, garantendogli un accesso preferenziale alle risorse della CPU.

2.  **Modifica della priorità di un processo in esecuzione (`renice`):**
    Qualora sia necessario variare la priorità di un'operazione già avviata senza interromperla, si ricorre al comando `renice`. In questo caso, è indispensabile identificare preventivamente il **PID** (Process ID) del target.
    ```bash
    # Esempio: cambiare la priorità del processo con PID 9360 impostando un valore di nice pari a 12
    renice -n 12 9360
    ```
    Dopo l'esecuzione, il sistema confermerà l'avvenuta modifica mostrando il vecchio valore di priorità e quello appena impostato.

### Sintesi e finalità
L'utilizzo combinato del demone **Tuned** per l'ottimizzazione dei profili hardware/software e dei comandi **nice/renice** per la gestione granulare dei processi, permette di massimizzare le prestazioni del sistema Linux. Queste operazioni assicurano che le applicazioni critiche ricevano le risorse necessarie, migliorando la reattività complessiva dell'infrastruttura.

---

## Guida Completa ai Container in Ambiente Linux

Questa lezione analizza il concetto di container, il loro funzionamento in ambito IT e l'utilizzo degli strumenti nativi di Red Hat, con particolare attenzione a **Podman**.

---

### 1. Cos'è un Container?

Il termine e il concetto derivano dai container per le spedizioni fisiche. Proprio come un container marittimo ha dimensioni standardizzate per essere trasportato ovunque (navi, camion, treni) senza modificare l'infrastruttura, un container IT permette di trasportare software in modo uniforme.

* **Standardizzazione:** Ogni container ha la stessa struttura logica, facilitando lo stoccaggio e il trasporto tra diversi ambienti.
* **Portabilità:** Funziona su qualsiasi "porto" (server o computer) che supporti la tecnologia dei container.



---

### 2. I Container nel mondo IT

In passato, uno sviluppatore scriveva codice sul proprio laptop e lo testava con successo. Tuttavia, una volta spostato sul server di produzione, il software spesso smetteva di funzionare a causa di differenze nelle librerie o nelle configurazioni del sistema operativo.

I container risolvono questo problema ("*funziona sulla mia macchina*") impacchettando:
1.  **Codice sorgente del software.**
2.  **Librerie necessarie.**
3.  **File di configurazione.**

**Nota bene:** Un container **non include il sistema operativo**. Esso condivide il kernel della macchina ospite, rendendolo estremamente leggero rispetto a una macchina virtuale. Un singolo sistema operativo può eseguire più container contemporaneamente in modo isolato.

---

### 3. Software per la Gestione dei Container

Esistono diverse soluzioni software per creare e gestire questi pacchetti:

* **Docker:** Lanciato nel 2013, è lo standard più noto. È disponibile sia per Linux che per Windows.
* **Podman:** Sviluppato da Red Hat (rilasciato nel 2018) come alternativa a Docker. È uno strumento **daemon-less** (senza un processo centrale sempre attivo) e open source.

> **Importante:** In Red Hat Enterprise Linux 8 (RHEL 8), Docker non è supportato ufficialmente; si utilizza preferibilmente Podman.



---

### 4. Ecosistema Tecnologico Red Hat

Red Hat fornisce un set di strumenti a riga di comando per operare senza la necessità di un motore di container pesante:
* **Podman:** Gestione diretta di pod, container e immagini (avvio, arresto, stato).
* **Buildah:** Utilizzato per costruire e firmare nuove immagini di container.
* **Skopeo:** Strumento per ispezionare, copiare ed eliminare immagini tra diversi registri.
* **RunC / CRun:** Runtime che forniscono le funzionalità di base per l'esecuzione dei container.

#### Terminologia Chiave
* **Immagini:** Sono i "template" o i modelli da cui vengono creati i container. Un container può anche essere convertito nuovamente in un'immagine.
* **Pod:** Gruppi di container distribuiti insieme sullo stesso host. Il logo di Podman (tre foche) rappresenta proprio un gruppo di container che lavorano all'unisono.

---

### 5. Guida Pratica: Installazione e Utilizzo

#### Installazione
Su sistemi Red Hat 8 o CentOS 7, il pacchetto si installa tramite i gestori di pacchetti standard:

```bash
# Su Red Hat 8/9
dnf install podman -y

# Su CentOS 7
yum install podman -y
```

Markdown

#### Comandi di Base
Dopo l'installazione, è possibile interagire con lo strumento tramite l'interfaccia a riga di comando. Di seguito sono riportate le operazioni fondamentali per la gestione dell'ambiente:

* **Verifica della versione:** Per confermare la corretta installazione e conoscere la versione del software in uso:
    ```bash
    podman -v
    ```
* **Accesso alla documentazione:** È possibile consultare l'aiuto rapido o le pagine del manuale per visualizzare tutte le opzioni disponibili (come `attach`, `build`, `commit`, `cp`, `create`):
    ```bash
    podman --help
    # Oppure per la documentazione estesa:
    man podman
    ```
* **Informazioni sull'ambiente e sui registri:** Il comando `info` permette di visualizzare i dettagli tecnici e l'ordine dei registri (registry) utilizzati per la ricerca delle immagini (ad esempio *registry.access.redhat.com*, *registry.redhat.io* e *docker.io*):
    ```bash
    podman info
    ```

---

#### Gestione delle Immagini e dei Container
Il flusso di lavoro standard prevede la ricerca di un'immagine preconfigurata, il suo scaricamento e la successiva attivazione come container.

1.  **Ricerca di un'immagine:**
    Per individuare un'immagine specifica nel repository (ad esempio un server Apache `httpd`), si utilizza il comando di ricerca. I risultati mostreranno l'indice, il nome, la descrizione e il numero di "stelle" (valutazione degli utenti):
    ```bash
    podman search httpd
    ```

2.  **Download (Pull) dell'immagine:**
    Una volta identificata la sorgente (ad esempio da *docker.io*), si procede al download locale:
    ```bash
    podman pull docker.io/library/httpd
    ```

3.  **Verifica delle immagini scaricate:**
    Per elencare le immagini presenti sul sistema, complete di ID, tag e dimensione:
    ```bash
    podman images
    ```

4.  **Esecuzione di un container:**
    Per avviare un container basato sull'immagine scaricata, si utilizza il comando `run`. Nell'esempio seguente vengono usate le opzioni `-d` (esecuzione in background), `-t` (allocazione di una pseudo-TTY) e `-p` (mappatura della porta 8080 dell'host sulla porta 80 del container):
    ```bash
    podman run -dt -p 8080:80 docker.io/library/httpd
    ```

5.  **Monitoraggio dei container attivi:**
    Per verificare lo stato dei container in esecuzione, identificare il loro ID univoco e controllare le porte assegnate:
    ```bash
    podman ps
    ```

---

## Installazione e Gestione di Docker su Linux

Questa lezione approfondisce l'installazione, la configurazione e la gestione di **Docker**, una piattaforma open-source fondamentale per la containerizzazione, ampiamente utilizzata in ogni ambiente Linux.

---

### 1. Il Concetto di Container
Il termine "container" deriva dal settore dei trasporti marittimi. Proprio come i container fisici hanno dimensioni standard per adattarsi a navi, camion e magazzini in tutto il mondo, i container IT standardizzano il software.

* **Efficienza:** Non sono necessarie attrezzature speciali per spostarli; si adattano a qualsiasi infrastruttura che supporti lo standard.
* **Risoluzione del conflitto "funziona sulla mia macchina":** In passato, il codice funzionante sul laptop di uno sviluppatore spesso falliva in produzione. I container risolvono il problema impacchettando codice, librerie e configurazioni in un'unica unità portatile, eliminando i problemi di compatibilità tra ambienti diversi.

> **Nota per l'Amministratore di Sistema:** Il compito principale dell'amministratore è installare, configurare e gestire l'infrastruttura dei container. La scrittura del codice e il packaging dell'applicazione sono responsabilità degli sviluppatori.

---

### 2. Cos'è Docker?
Docker è una piattaforma aperta per lo sviluppo, la spedizione e l'esecuzione di applicazioni. Permette di separare l'applicazione dall'infrastruttura sottostante, accelerando la distribuzione del software.

* **Docker Engine:** Il cuore del sistema; un demone che gestisce i container tramite lo strumento `systemctl`.
* **Docker Hub:** Un registro pubblico (simile a un grande "frigo" per pasti pronti) dove è possibile trovare, condividere e archiviare immagini Docker.
* **Docker Image:** Un "blueprint" o un'istantanea che contiene tutto il necessario per eseguire un'applicazione (codice, runtime, librerie di sistema).

---

### 3. Preparazione all'Installazione (CentOS Stream)
Prima di procedere, si raccomanda di consultare la documentazione ufficiale su *docs.docker.com*. Esistono due versioni principali:
1.  **Docker CE (Community Edition):** Gratuita e open-source (scelta per questa lezione).
2.  **Docker EE (Enterprise Edition):** A pagamento, con funzionalità avanzate per il business.

#### Passaggi Preliminari:
* **Rimozione versioni precedenti:** È essenziale rimuovere eventuali vecchie installazioni per evitare conflitti.
* **Verifica privilegi:** Le operazioni devono essere eseguite come utente `root`.

---

### 4. Procedura di Installazione e Configurazione

#### Step 1: Configurazione del Repository
È necessario installare le utility di gestione e aggiungere il repository ufficiale per garantire l'accesso all'ultima versione stabile.

```bash
# Installazione delle utility di sistema
dnf install -y yum-utils

# Aggiunta del repository ufficiale di Docker
dnf config-manager --add-repo [https://download.docker.com/linux/centos/docker-ce.repo](https://download.docker.com/linux/centos/docker-ce.repo)
```

#### Step 2: Installazione dei Componenti Core
Una volta configurato il repository ufficiale, si procede all'installazione dei pacchetti fondamentali che compongono l'ecosistema Docker. Questa operazione scarica il motore di runtime, gli strumenti di gestione e i plugin necessari per le operazioni avanzate.

* **Esecuzione dell'installazione:**
    Viene utilizzato il gestore di pacchetti `dnf` per installare simultaneamente i seguenti componenti:
    * **docker-ce:** Il motore Docker vero e proprio (Community Edition).
    * **docker-ce-cli:** L'interfaccia a riga di comando per interagire con il demone.
    * **containerd.io:** Il runtime che gestisce il ciclo di vita dei container.
    * **docker-buildx-plugin:** Plugin per funzionalità di build avanzate.
    * **docker-compose-plugin:** Strumento per la gestione di applicazioni multi-container.

    ```bash
    dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

---

#### Step 3: Gestione del Servizio
Al termine dell'installazione, il servizio Docker non risulta attivo di default. È necessario intervenire manualmente per avviarlo e garantirne la persistenza.

1.  **Avvio del servizio:** Per attivare immediatamente il motore Docker, si esegue:
    ```bash
    systemctl start docker
    ```
2.  **Abilitazione all'avvio (Boot):** Per far sì che il servizio venga caricato automaticamente ad ogni riavvio del sistema:
    ```bash
    systemctl enable docker
    ```
3.  **Verifica dello stato:** Per confermare che il processo sia in esecuzione (stato *active/running*):
    ```bash
    systemctl status docker
    ```

---

#### Step 4: Test di Funzionamento
La verifica finale dell'ambiente avviene tramite l'esecuzione di un container di prova. Il comando `docker run` automatizza l'intero processo di reperimento ed esecuzione.

* **Esecuzione del test:**
    ```bash
    docker run hello-world
    ```

**Analisi del processo di esecuzione:**
* **Ricerca locale:** Docker verifica la presenza dell'immagine `hello-world` sul disco locale.
* **Download (Pulling):** Se l'immagine non è presente, viene scaricata automaticamente dai repository ufficiali di **Docker Hub**.
* **Istanziazione:** Viene creato ed eseguito un nuovo container basato sull'immagine ottenuta.
* **Output di successo:** La visualizzazione del messaggio *"Hello from Docker"* conferma la corretta installazione e configurazione di tutti i componenti.

---

# Automazione dell'Installazione Linux con Kickstart

**Kickstart** rappresenta una metodologia di automazione per l'installazione di sistemi operativi Linux (Red Hat, CentOS, Fedora). Tale sistema consente di eseguire il deployment del sistema operativo senza intervento umano, pre-configurando tutte le risposte che normalmente verrebbero richieste durante la procedura guidata (Wizard).

---

## 1. Architettura e Funzionamento
Durante un'installazione standard, l'utente deve definire manualmente parametri quali lingua, fuso orario, partizionamento del disco e selezione del software. Kickstart automatizza queste scelte attraverso un file di configurazione denominato **ks.cfg**.

### Componenti necessari:
* **Kickstart Server:** Un server (spesso HTTP, FTP o NFS) che ospita il file di configurazione.
* **File Kickstart:** Documento di testo contenente i parametri di installazione.
* **Sorgente di Installazione:** Repository dei pacchetti o immagine ISO del sistema operativo.
* **Mezzo di Avvio:** Supporto per il boot iniziale del client (ISO, USB o rete PXE).

---

## 2. Metodi di Generazione del File Kickstart
Esistono due approcci principali per la creazione del file di configurazione:

1.  **system-config-kickstart:** Uno strumento grafico (GUI) disponibile su CentOS/RHEL 7 che facilita la selezione dei parametri.
2.  **File Anaconda (anaconda-ks.cfg):** In ogni sistema Red Hat/CentOS, viene generato automaticamente un file in `/root/anaconda-ks.cfg` al termine di un'installazione manuale. Questo file rispecchia le scelte effettuate e può essere utilizzato come modello.

> **Nota:** Nelle versioni 8 e successive, lo strumento grafico è stato rimosso in favore di sistemi di automazione come **Ansible** o l'uso diretto del file di Anaconda.

---

## 3. Configurazione del Server di Laboratorio
Per distribuire il file Kickstart via rete, viene tipicamente utilizzato il protocollo HTTP.

### Configurazione del servizio Web:
```bash
# Installazione del server Apache
yum install httpd -y

# Avvio e abilitazione del servizio
systemctl start httpd
systemctl enable httpd

# Disabilitazione del firewall per il test
systemctl stop firewalld
systemctl disable firewalld
```

**Pubblicazione del file:**
Il file di configurazione deve essere reso accessibile nella directory pubblica del server web:
```bash
# Copia del file nella Document Root
cp /root/anaconda-ks.cfg /var/www/html/anaconda-ks.cfg

# Assegnazione dei permessi di lettura
chmod a+r /var/www/html/anaconda-ks.cfg
```
La verifica della corretta pubblicazione avviene accedendo via browser all'indirizzo: `http://<IP_SERVER>/anaconda-ks.cfg`.

#### 4. Esecuzione dell'Installazione Automatizzata
Per avviare la procedura su un nuovo client, è necessario interrompere il boot dell'immagine ISO (premendo `ESC` nella schermata iniziale) e inserire la stringa di configurazione al prompt di avvio. Questo indirizza l'installer verso il file di configurazione remoto.



**Analisi del processo:**
Una volta inviato il comando, l'installer **Anaconda** esegue le seguenti operazioni in autonomia:
* **Recupero del file:** Il sistema scarica il file `.cfg` via rete utilizzando il protocollo specificato.
* **Configurazione automatica:** Vengono impostati tastiera, lingua e fuso orario senza prompt per l'utente.
* **Gestione dello storage:** Viene eseguito il partizionamento del disco rigido secondo i parametri definiti nel file.
* **Installazione software:** Si procede all'installazione dei pacchetti (solitamente circa 1.300-1.400 per sistemi con interfaccia grafica).
* **Setup degli account:** Vengono impostate le password di root e creati gli utenti locali specificati.

---

#### 5. Configurazione avanzata: Ambienti senza DHCP
In assenza di un server DHCP, il client non è in grado di ottenere un indirizzo IP per comunicare con il server Kickstart. In questo scenario, è necessario dichiarare i parametri di rete statici direttamente nella riga di comando di boot.

**Sintassi per IP statico:**
```text
linux ks=http://<IP_SERVER>/anaconda-ks.cfg ksdevice=<INTERFACCIA> ip=<IP_CLIENT> netmask=<MASK> gateway=<GW>
```

**Dettaglio dei parametri:**
* **ksdevice:** Indica l'interfaccia di rete fisica o virtuale da attivare per la comunicazione iniziale (es. `eth0` per sistemi fisici o `enp0s3` per ambienti virtuali VirtualBox).
* **ip:** L'indirizzo IP statico da assegnare temporaneamente al client affinché possa raggiungere il server e scaricare il file di configurazione.
* **netmask:** La maschera di sottorete necessaria per definire l'estensione della rete locale (es. `255.255.255.0`).
* **gateway:** L'indirizzo del router o del nodo di uscita necessario per l'instradamento dei pacchetti verso il server Kickstart, qualora si trovi in una sottorete differente.

---

#### 6. Scalabilità e Conclusioni
L'adozione di Kickstart consente una scalabilità elevata: lo stesso file di configurazione può servire simultaneamente un numero illimitato di client. Poiché l'immagine ISO e il file `.cfg` sono accessibili in modalità di sola lettura, è possibile gestire il deployment massivo di server garantendo uniformità di configurazione e una drastica riduzione dei tempi di messa in produzione.

---

# Installazione, Configurazione e Gestione di Ansible in Linux

**Ansible** è una suite di strumenti software per l'implementazione dell'**Infrastructure as Code (IaC)**. Si tratta di una piattaforma open source dedicata al provisioning software, alla gestione delle configurazioni e al deployment di applicazioni.

---

## 1. Concetti Fondamentali

Ansible si distingue per alcune caratteristiche chiave che ne facilitano l'adozione in ambienti IT complessi:

* **Agentless:** Non richiede l'installazione di software aggiuntivo (agenti) sui nodi gestiti. La comunicazione avviene tramite protocolli standard (solitamente SSH).
* **Basato su YAML:** Utilizza il linguaggio YAML (*Yet Another Markup Language*), un formato leggibile dall'uomo basato su coppie chiave-valore e indentazione, per definire le configurazioni.
* **Playbook:** Sono file YAML che elencano i task da automatizzare. Descrivono lo stato desiderato del sistema (es. "il software X deve essere presente").
* **Inventory:** Un file (solitamente `/etc/ansible/hosts`) che elenca gli indirizzi IP o i nomi host dei server da gestire, organizzati eventualmente in gruppi.



---

## 2. Cronologia Essenziale
* **2012:** Creazione da parte di Michael DeHaan.
* **2013:** Rilascio ufficiale come progetto open source.
* **2015:** Acquisizione da parte di **Red Hat**.
* **2021:** Rilascio di Ansible Automation Platform 2.

---

## 3. Procedura Operativa (Step-by-Step)

### Step 1: Installazione del Control Node
Sebbene Ansible sia *agentless* sui nodi remoti, deve essere installato sul nodo di controllo.

```bash
# Installazione tramite package manager DNF
dnf install ansible -y

# Verifica dell'installazione
ansible --version
```
*Nota: L'installazione include Ansible Core e Python 3, linguaggio su cui si basa l'esecuzione dei moduli.*

### Step 2: Configurazione dell'Inventory
È necessario istruire Ansible su quali macchine operare modificando il file host, che funge da inventario dei nodi gestiti.

```bash
vi /etc/ansible/hosts
```

Esempio di configurazione per gestire la macchina locale:

```Ini, TOML
[webservers]
localhost ansible_connection=local
```

### Step 3: Verifica della Connessione
Il modulo `ping` permette di testare la raggiungibilità dei nodi definiti nell'inventory. A differenza del comando di rete standard, questo modulo verifica la capacità di Ansible di connettersi via SSH ed eseguire codice Python sul target.

```bash
ansible -m ping webservers
```

*Un output con stato "success" e risposta "pong" in formato JSON conferma la corretta configurazione della comunicazione tra il nodo di controllo e gli host.*

---

### Step 4: Configurazione dell'Accesso Remoto (SSH)
Per la gestione di server remoti, è necessaria l'autenticazione tramite chiavi SSH. Questo metodo abilita Ansible a operare in modalità non presidiata, evitando la richiesta manuale di password per ogni operazione e garantendo un canale di comunicazione sicuro.



1.  **Generazione della coppia di chiavi:** `ssh-keygen -t rsa -b 2048`
2.  **Trasferimento della chiave pubblica sul server remoto:** `ssh-copy-id utente@IP_REMOTO`
3.  **Test di accesso:** `ssh utente@IP_REMOTO` (l'accesso deve avvenire senza inserimento di credenziali).

---

### Step 5: Creazione ed Esecuzione di un Playbook
L'automazione vera e propria viene implementata tramite i Playbook. Si ipotizzi di dover installare il server web Apache (`httpd`) simultaneamente su tutti i nodi appartenenti al gruppo `webservers`.

1.  **Aggiornamento dell'Inventory:**
    Nel file `/etc/ansible/hosts`, si definiscono i parametri di connessione specifici per ogni host remoto, inclusi indirizzi IP e utenti:
    ```ini
    [webservers]
    localhost ansible_connection=local
    centos_server ansible_host=192.168.100.169 ansible_user=iafzal
    ```

2.  **Scrittura del Playbook (`install_httpd.yaml`):**
    Il file deve seguire rigorosamente la sintassi YAML, dove l'indentazione definisce la gerarchia dei comandi.



```yaml
---
- name: Installazione Apache su Webservers
  hosts: webservers
  become: yes
  tasks:
    - name: Installazione pacchetto httpd
      dnf:
        name: httpd
        state: present
```

3. **Esecuzione del Playbook:**
L'esecuzione del Playbook avviene tramite il comando `ansible-playbook`. Durante questa fase, Ansible legge l'inventario, si connette agli host specificati e applica i task in ordine sequenziale.

```bash
ansible-playbook install_httpd.yaml --ask-become-pass
```

* **--ask-become-pass (o -K):** Questo parametro istruisce Ansible a richiedere la password di root/sudo, indispensabile per eseguire operazioni che richiedono privilegi elevati, come l'installazione di pacchetti di sistema tramite `dnf`.
* **Esecuzione:** Ansible si connette via SSH a ciascun host remoto definito nel gruppo `webservers`, verifica lo stato del pacchetto `httpd` e, se non presente, procede all'installazione.



---

### Step 6: Verifica Finale e Conclusioni
Al termine del processo, Ansible genera un riepilogo dettagliato dello stato di ogni host, denominato **Play Recap**. Questo sommario permette di monitorare l'esito delle operazioni su tutta l'infrastruttura in un'unica schermata.

**Interpretazione dei risultati:**
* **ok:** Indica che il task è stato completato con successo o che il sistema si trovava già nello stato desiderato (principio di idempotenza).
* **changed:** Segnala che è stata apportata una modifica effettiva al sistema (es. il pacchetto è stato installato ex novo).
* **unreachable:** Il nodo non è stato raggiunto per problemi di rete o credenziali SSH errate.
* **failed:** Si è verificato un errore logico durante l'esecuzione del task.



Per confermare manualmente l'avvenuta installazione su tutti i nodi gestiti, è possibile eseguire il seguente comando di verifica:
```bash
rpm -qa | grep httpd
```

L'implementazione di Ansible consente di gestire infrastrutture scalabili con estrema precisione, garantendo l'uniformità delle configurazioni e riducendo sensibilmente i tempi operativi necessari per la manutenzione e il deployment.

---

# Installazione, Configurazione e Gestione di OpenVPN in ambiente Linux

**OpenVPN** è un protocollo VPN open-source che permette di creare connessioni sicure e private punto-punto. A differenza dei protocolli tradizionali, OpenVPN è ampiamente apprezzato per la sua affidabilità, sicurezza e compatibilità multipiattaforma.

---

## 1. Concetti Fondamentali e Funzionamento

Una **VPN (Virtual Private Network)** stabilisce una connessione digitale tra un dispositivo locale e un server remoto. Questo processo crea un **tunnel criptato** che:
* Protegge i dati personali da intercettazioni.
* Maschera l'indirizzo IP reale dell'utente.
* Permette di superare blocchi geografici o firewall.



### OpenVPN vs VPN Tradizionali
Mentre il termine VPN è un concetto generale, OpenVPN è un'implementazione specifica. Rispetto al protocollo PPTP (sviluppato da Microsoft negli anni '90), OpenVPN offre una sicurezza superiore grazie all'uso di librerie OpenSSL e alla possibilità per la community di revisionare e migliorare costantemente il codice sorgente.

---

## 2. Architettura del Laboratorio
Per questa procedura vengono utilizzate due macchine virtuali:
1.  **Server:** Nodo centrale che gestisce le connessioni (IP finale `.167`).
2.  **Client:** Nodo che si connette al tunnel (IP finale `.178`).

> **Nota:** Si raccomanda di eseguire uno snapshot delle macchine virtuali prima di procedere, in modo da poter ripristinare lo stato originale in caso di errori di configurazione.

---

## 3. Procedura Operativa: Configurazione Server

### Step 1: Installazione dei pacchetti
È necessario installare il software OpenVPN e l'utility `easy-rsa`, fondamentale per la gestione della **PKI (Public Key Infrastructure)** e dei certificati.

```bash
dnf install openvpn easy-rsa -y
```

### Step 2: Configurazione della PKI con Easy-RSA
I certificati agiscono come documenti di identità digitali, garantendo che solo i server e i client autorizzati possano accedere alla rete. Per la loro gestione si utilizza l'infrastruttura a chiave pubblica (PKI).

1.  **Inizializzazione della PKI:**
    Dopo aver copiato i file di `easy-rsa` nella directory `/etc/openvpn`, è necessario preparare l'ambiente per la creazione dei file di sicurezza.
    ```bash
    ./easy-rsa init-pki
    ```
    Questo comando crea la struttura delle cartelle necessaria per ospitare chiavi e certificati.

2.  **Creazione della Certification Authority (CA):**
    Viene generato il certificato radice che servirà a firmare e validare tutte le richieste di connessione.
    ```bash
    ./easy-rsa build-ca no-pass
    ```
    L'opzione `no-pass` viene utilizzata per semplificare le operazioni di laboratorio; in ambienti di produzione è consigliabile impostare una password.

3.  **Generazione dei Certificati per Server e Client:**
    Vengono create le chiavi private e le relative richieste di firma (CSR).
    * **Per il server:** `./easy-rsa gen-req server no-pass`
    * **Per il client:** `./easy-rsa gen-req client no-pass`

4.  **Firma dei Certificati:**
    La CA valida ufficialmente l'identità del server e del client firmando i file generati al punto precedente.
    * **Firma server:** `./easy-rsa sign-req server server`
    * **Firma client:** `./easy-rsa sign-req client client`

5.  **Generazione parametri Diffie-Hellman e Chiave HMAC:**
    Questi componenti aggiungono livelli di sicurezza per lo scambio delle chiavi e proteggono contro attacchi DoS.
    ```bash
    ./easy-rsa gen-dh
    openvpn --genkey secret ta.key
    ```



---

### Step 3: Configurazione del Servizio Server
Una volta pronti i file di sicurezza, questi devono essere spostati nella directory di configurazione di OpenVPN per essere utilizzati dal servizio.

1.  **Trasferimento file:** I file `ca.crt`, `dh.pem`, `server.crt`, `server.key` e `ta.key` vengono posizionati in `/etc/openvpn`.
2.  **Configurazione del file server.conf:** È necessario modificare il file di esempio per puntare ai corretti percorsi dei certificati e definire i parametri di rete (es. porta 1194).
3.  **Abilitazione del routing (IP Forwarding):** Per consentire al server di instradare il traffico tra i client e le reti esterne, deve essere attivato l'inoltro dei pacchetti IP a livello di kernel.
    ```bash
    echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
    sysctl -p
    ```

# Configurazione e Gestione di un Server DHCP in ambiente Linux

Il **DHCP (Dynamic Host Configuration Protocol)** è un protocollo di rete che permette l'assegnazione automatica di indirizzi IP e altri parametri di configurazione ai dispositivi presenti su una rete.

---

## 1. Considerazioni Preliminari

In contesti aziendali, il servizio DHCP è solitamente gestito da amministratori di rete attraverso router o dispositivi hardware dedicati. Raramente un amministratore di sistema Linux si occupa della sua gestione diretta.

> **Importante:** Questa guida illustra la configurazione del server DHCP a scopo puramente concettuale e didattico. L'implementazione in una rete domestica reale richiede la riconfigurazione del modem/router per instradare il traffico verso il nuovo server Linux; operazioni errate in questa fase possono causare la perdita di connettività per tutti i dispositivi della casa.

---

## 2. Funzionamento del DHCP

Per comunicare in rete, ogni computer necessita di un indirizzo IP unico.
* **Assegnazione Automatica:** Il server DHCP assegna dinamicamente un IP a ogni nuovo dispositivo (tablet, laptop, server) che si connette alla rete.
* **Natura Dinamica:** Gli indirizzi assegnati tramite DHCP possono variare nel tempo (es. dopo un riavvio del dispositivo).
* **IP Statico:** In ambito aziendale, circa il 95-98% dei server utilizza IP statici (configurati manualmente) per garantire che l'indirizzo rimanga invariato, facilitando il puntamento DNS e la gestione dei servizi.

---

## 3. Procedura di Configurazione (Server Linux)

### Step 1: Preparazione e Snapshot
Prima di procedere su una macchina virtuale, è fondamentale eseguire uno **snapshot**. In caso di errori, sarà possibile ripristinare lo stato precedente in pochi secondi.

### Step 2: Assegnazione di un IP Statico al Server DHCP
Il server che distribuisce gli indirizzi IP non può dipendere a sua volta da un altro server DHCP; deve possedere un indirizzo IP statico e "cablato" (hard-coded).

Per configurare l'IP in modo semplice su distribuzioni come CentOS/RHEL, si raccomanda l'uso dello strumento grafico testuale:
```bash
nmtui
```

1.  **Selezione dell'interfaccia di rete:** Attraverso il menu, viene individuata l'interfaccia corretta (es. `enp0s3`).
2.  **Configurazione dell'indirizzo:** Viene impostato un indirizzo statico (es. `192.168.15.1/24`). La notazione `/24` (corrispondente alla maschera `255.255.255.0`) consente la gestione di una sottorete fino a 256 indirizzi.

---

### Step 3: Installazione del Pacchetto
In base alla versione della distribuzione in uso, l'installazione del software avviene tramite il gestore di pacchetti predefinito:
* **RHEL/CentOS 7:** `yum install dhcp`
* **RHEL/CentOS 8+:** `dnf install dhcp-server`

### Step 4: Configurazione del file dhcp.conf
Il comportamento del server viene regolato modificando il file `/etc/dhcp/dhcpd.conf`. Qualora il file risultasse vuoto o non presente, è possibile utilizzare un modello di esempio disponibile nel sistema:
```bash
cp /usr/share/doc/dhcp*/dhcpd.conf.example /etc/dhcp/dhcpd.conf
```

**Definizione dei parametri operativi:**
All'interno del file di configurazione vengono stabiliti i criteri per il rilascio degli indirizzi IP:

* **Tempo di lease (Default/Max):** Viene definita la durata della prenotazione di un indirizzo per un client. Ad esempio, è possibile impostare una durata minima di 10 minuti (600 secondi) e una massima di 2 ore (7200 secondi).
* **Subnet e Range:** Viene specificata la sottorete di appartenenza e l'intervallo esatto di indirizzi che il server è autorizzato a distribuire. Anche se la sottorete (es. `/24`) permette fino a 256 indirizzi, il range può essere limitato (ad esempio da `.50` a `.200`) per riservare gli altri IP a utilizzi statici.
* **Opzioni di rete (Gateway e DNS):** Vengono definiti i parametri che verranno inviati automaticamente a ogni host, come l'indirizzo del router (gateway predefinito), la maschera di sottorete e i server DNS per la risoluzione dei nomi.

---

### Step 5: Gestione del Servizio e Sicurezza
Completata la redazione del file di configurazione, si procede all'attivazione del demone e alla gestione delle eccezioni nel sistema di sicurezza.

1.  **Avvio e persistenza:** Il servizio viene avviato e configurato per l'esecuzione automatica al boot del sistema.
    ```bash
    systemctl start dhcpd
    systemctl enable dhcpd
    ```

2.  **Configurazione del Firewall:** Per consentire ai client di comunicare con il server, è necessario autorizzare il servizio DHCP nelle impostazioni di firewalld.
    ```bash
    firewall-cmd --add-service=dhcp --permanent
    firewall-cmd --reload
    ```
    *In alternativa, in ambienti di test controllati, è possibile procedere all'arresto temporaneo del firewall tramite `systemctl stop firewalld`.*

---

### Step 6: Integrazione con l'Infrastruttura di Rete
Perché il server Linux diventi l'autorità DHCP effettiva, è necessario intervenire sul router o sul modem fornito dall'ISP:

1.  **Disabilitazione DHCP:** Viene disattivata la funzione DHCP nativa del router per evitare conflitti di indirizzamento sulla stessa rete.
2.  **Configurazione Forwarding:** Viene impostato il router affinché inoltri le richieste DHCP verso l'indirizzo IP statico del server Linux.

*Si ricorda che la procedura di configurazione del router varia sensibilmente a seconda del produttore e del modello in uso.*

---

# Configurazione e Gestione di Squid Proxy Server in Linux

Un **Proxy Server** è un'applicazione che funge da intermediario tra un client che richiede una risorsa e il server che la fornisce. In termini semplici, agisce come un "uomo di mezzo" tra il dispositivo dell'utente e Internet.



### Vantaggi dell'utilizzo di un Proxy
* **Privacy:** Nasconde l'indirizzo IP reale e la posizione del client.
* **Sicurezza:** Aggiunge uno strato di protezione all'ambiente Linux.
* **Velocità:** Grazie al caching, memorizza copie di siti web frequentati per servirli più rapidamente.
* **Controllo:** Permette di monitorare il traffico e bloccare l'accesso a determinati contenuti.

---

## 1. Caratteristiche di Squid Proxy
Squid è un server proxy ad alte prestazioni specializzato nella gestione del traffico web.
* **Porta predefinita:** Utilizza la porta **3128**.
* **Configurazione:** Il file principale risiede in `/etc/squid/squid.conf`.
* **Caching:** Ottimizza la banda memorizzando i file scaricati di frequente.

---

## 2. Installazione e Configurazione Iniziale

### Preparazione
Prima di procedere, è consigliabile eseguire uno **snapshot** della macchina virtuale per poter ripristinare il sistema in caso di errori. La procedura deve essere eseguita come utente **root**.

### Installazione del pacchetto
Per installare Squid e tutti i relativi pacchetti su distribuzioni basate su RHEL/CentOS:
```bash
dnf install squid* -y
```

### Gestione del servizio
Una volta completata l'installazione, è necessario avviare il demone di Squid e configurarlo affinché rimanga attivo anche dopo il riavvio del sistema.

```bash
systemctl start squid
systemctl enable squid
systemctl status squid
```

## 3. Configurazione delle Regole di Accesso (ACL)

Le **Access Control Lists (ACL)** sono fondamentali per stabilire i criteri di sicurezza e i permessi di navigazione. Esse permettono di definire con precisione quali host possono accedere al servizio e quali destinazioni devono essere filtrate.

### Abilitazione della rete locale
Per autorizzare i dispositivi della propria sottorete all'utilizzo del proxy, è necessario intervenire sul file `/etc/squid/squid.conf`. La configurazione prevede la definizione di una sorgente (src) seguita dall'istruzione di accesso:

```text
acl local_net src 192.168.100.0/24
http_access allow local_net
```

### Filtraggio dei siti web (Blocklist)
Il controllo dell'accesso a domini specifici viene gestito attraverso l'integrazione di liste di blocco personalizzate. Questa procedura permette di limitare la navigazione verso categorie di siti non autorizzate, come i social media o i portali di e-commerce.

1.  **Creazione del file di restrizione:** Viene generato un file di testo dedicato (ad esempio `/etc/squid/block_sites.txt`) all'interno del quale vengono elencati i domini da inibire.
    * Esempio di contenuto: `.facebook.com`
    * L'utilizzo del punto antecedente al nome del dominio assicura che il blocco venga esteso non solo al dominio principale, ma anche a tutti i relativi sottodomini.

2.  **Integrazione delle direttive in Squid:** All'interno del file principale `squid.conf`, viene dichiarata una ACL (Access Control List) che punta al file appena creato. Successivamente, viene applicata una regola di diniego per tale lista.

```text
# Definizione della lista basata sui domini contenuti nel file
acl Block_Sites_RegX dstdomain "/etc/squid/block_sites.txt"

# Applicazione del blocco (da inserire prima delle regole di accesso consentito)
http_access deny Block_Sites_RegX
```

## 4. Configurazione del Firewall e Riavvio del Sistema

Affinché le richieste provenienti dai client non vengano intercettate e bloccate dalle barriere di sicurezza del server, è indispensabile autorizzare il traffico sulla porta TCP **3128**.

```bash
# Apertura permanente della porta nel firewall
firewall-cmd --add-port=3128/tcp --permanent
firewall-cmd --reload
```

Al termine di ogni modifica strutturale ai file di configurazione, il servizio deve essere riavviato per rendere operative le nuove istruzioni e ricaricare correttamente le liste di blocco e le ACL.

```bash
systemctl restart squid
```

## 5. Configurazione del Client e Verifica Operativa

Per instradare correttamente il traffico attraverso il proxy, è necessaria una configurazione manuale sul dispositivo dell'utente finale. All'interno delle impostazioni di rete del browser (ad esempio Firefox), deve essere inserito l'indirizzo IP del server Squid unitamente alla porta **3128** per tutti i protocolli supportati.



### Test di funzionamento
La verifica della corretta configurazione avviene testando due scenari distinti:

* **Navigazione consentita:** L'accesso a portali non inclusi nelle liste di restrizione (es. `google.com`) deve risultare regolare. In questa modalità, il proxy funge da intermediario e gestore della cache, ottimizzando potenzialmente i tempi di caricamento.
* **Verifica del blocco:** Tentando di raggiungere un sito interdetto (es. `facebook.com`), il browser deve mostrare una pagina di errore o un messaggio di "Accesso negato" (Access Denied) generato direttamente da Squid.


---


