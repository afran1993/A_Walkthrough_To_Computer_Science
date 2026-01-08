
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

