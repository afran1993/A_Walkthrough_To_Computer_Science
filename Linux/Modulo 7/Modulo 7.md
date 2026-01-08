
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
