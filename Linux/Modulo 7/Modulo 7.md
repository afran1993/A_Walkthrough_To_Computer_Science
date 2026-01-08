
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

