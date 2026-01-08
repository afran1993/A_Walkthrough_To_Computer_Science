# I Run Level del Sistema in Linux

I **Run Level** rappresentano le diverse modalità operative in cui può trovarsi un sistema operativo Linux. Ogni livello determina quali servizi e funzionalità sono attivi all'avvio o durante l'esecuzione del sistema. Analogamente alla "Modalità Provvisoria" (Safe Mode) di Windows, Linux utilizza questi livelli per scopi di manutenzione, risoluzione dei problemi o operatività standard.



---

### Panoramica dei Run Level principali

In Linux esistono sette tipi principali di Run Level (da 0 a 6). Di seguito vengono descritti i più significativi per un amministratore di sistema:

| Livello | Descrizione | Effetto del comando |
| :--- | :--- | :--- |
| **0** | **Halt / Shut down** | Spegne completamente il sistema (non riavvia). |
| **1** | **Single-user mode** | Modalità utente singolo per manutenzione (alias: `s` o `S`). |
| **2** | **Multi-user mode (No Network)** | Modalità multi-utente locale senza supporto di rete. |
| **3** | **Multi-user mode (Networking, No GUI)** | Modalità multi-utente con rete ma solo interfaccia testuale. |
| **4** | **Unused** | Livello non assegnato, personalizzabile dall'utente. |
| **5** | **Multi-user mode (Networking + GUI)** | Modalità standard con rete e interfaccia grafica. |
| **6** | **Reboot** | Riavvia completamente il sistema. |

---

### Comandi per la gestione dei Run Level

Per eseguire il passaggio da un livello all'altro è necessario possedere i privilegi di **root**. È possibile verificare l'identità dell'utente corrente tramite il comando `whoami`.

#### Cambiare Run Level
Per cambiare la modalità del sistema si utilizza il comando `init` seguito dal numero del livello desiderato:
* `init 0`: Per spegnere il computer.
* `init 6`: Per riavviare il sistema.
* `init 3`: Per passare dalla modalità grafica alla console testuale.
* `init 5`: Per avviare l'interfaccia grafica.

#### Verificare il Run Level corrente
Per conoscere in quale modalità si trova attualmente il sistema, viene utilizzato il comando:
```bash
who -r
```

L'output mostrerà il livello attuale (ad esempio, "run-level 5") e la data/ora dell'ultimo cambio di stato.



---

### Casi d'uso e Pratica

* **Risoluzione dei problemi:** Il livello **1** (Single-user mode) risulta fondamentale per il ripristino di password dimenticate o per la correzione di errori critici nel file system, in quanto vengono avviati esclusivamente i servizi minimi essenziali.
* **Server di produzione:** Molti server aziendali operano stabilmente nel livello **3** per ottimizzare le risorse di sistema, evitando il consumo di memoria e CPU associato all'interfaccia grafica (GUI).
* **Ambienti Desktop:** La maggior parte delle postazioni di lavoro Linux è configurata per l'avvio automatico nel livello **5**.



> **Nota operativa:** L'esecuzione di `init 0` comporta lo spegnimento fisico della macchina. In ambienti virtualizzati (come Oracle VirtualBox) è necessario riavviare la macchina virtuale manualmente dall'interfaccia dell'hypervisor; su server fisici, l'operazione richiede l'accesso diretto all'hardware o a sistemi di gestione remota.

---

# Il Processo di Boot del Computer (Bootstrap)

Il processo di avvio di un computer, noto anche come **bootstrap**, è la sequenza di operazioni che permette al sistema di accendersi e caricare il sistema operativo. Sebbene esistano diverse piattaforme hardware e vari sistemi operativi (come Windows o Linux), le fasi fondamentali del processo rimangono simili.

---

### 1. Alimentazione e Inizializzazione della CPU
Quando il computer viene collegato alla rete elettrica e acceso, l'elettricità fluisce attraverso la scheda madre. La **CPU** (Central Processing Unit) è il primo componente a ricevere energia. In questa fase iniziale, la CPU non possiede istruzioni proprie su cosa eseguire e si rivolge immediatamente al firmware del sistema.

### 2. BIOS e ROM
La CPU recupera le prime istruzioni dal **BIOS** (*Basic Input and Output System*). Il BIOS è un software di basso livello preinstallato dal produttore dell'hardware su un chip di memoria denominato **ROM** (*Read-Only Memory*). Essendo una memoria a sola lettura, le istruzioni contenute al suo interno sono permanenti e non possono essere modificate o cancellate accidentalmente.



[Image of BIOS chip on a motherboard]


### 3. CMOS e Batteria Tampone
Per funzionare correttamente, il BIOS consulta un altro chip chiamato **CMOS** (*Complementary Metal-Oxide Semiconductor*). Questo chip memorizza le impostazioni personalizzate del BIOS, come l'ordine di avvio dei dischi, l'ora e la data del sistema.
* **Alimentazione:** A differenza della ROM, il CMOS necessita di una piccola quantità di energia costante per non perdere i dati. Questa è fornita da una piccola batteria al litio situata sulla scheda madre.



---

### 4. Il Test POST (Power-On Self-Test)
Una delle istruzioni principali del BIOS è l'esecuzione del **POST**. Durante questo test, la CPU verifica l'integrità e la presenza dell'hardware collegato (RAM, tastiera, dischi rigidi, ecc.).
* **Esito positivo:** Il processo prosegue verso il caricamento del sistema operativo.
* **Esito negativo:** Se viene rilevato un componente difettoso, il processo di avvio si interrompe, spesso emettendo dei segnali acustici (*beep codes*) per indicare il tipo di errore.

### 5. MBR e Caricamento del Sistema Operativo
Una volta superato il POST, il BIOS cerca le istruzioni di avvio nel primo settore del disco rigido, chiamato **MBR** (*Master Boot Record*).
* L'MBR contiene le informazioni necessarie per individuare e avviare il sistema operativo (Windows, Linux, ecc.).
* Il sistema operativo viene quindi caricato dal disco alla **RAM** (Memoria ad accesso casuale).
* Da questo momento, il sistema operativo assume il controllo, avviando i propri programmi e applicazioni e interagendo con la CPU per l'elaborazione dei dati.



---

### Riepilogo del Processo (Bootstrap)
L'intera sequenza che va dall'accensione della CPU al caricamento finale del sistema operativo è definita **Bootstrap**. È un termine tecnico ampiamente utilizzato nel mondo IT per descrivere come il computer "si tiri su dai lacci dei propri stivali" (*pulling oneself up by one's bootstraps*), partendo da zero fino a diventare pienamente operativo.

# Il Processo di Avvio di Linux (Boot Process)

Comprendere cosa accade dal momento in cui si preme il pulsante di accensione fino alla comparsa del prompt di login è fondamentale per ogni amministratore di sistema. Questo processo, composto da sei fasi sequenziali, è spesso oggetto di domande durante i colloqui di lavoro tecnici.

---

### Le 6 Fasi del Boot Process

#### 1. BIOS (Basic Input/Output System)
All'accensione, il sistema esegue il BIOS, un firmware che risiede sulla scheda madre.
* **Ruolo:** Esegue i controlli di integrità del sistema (POST).
* **Compito:** Individua, carica ed esegue il programma di boot loader cercandolo nei supporti di memoria disponibili (Hard Drive, CD-ROM, USB).

#### 2. MBR (Master Boot Record)
L'MBR è situato nel primo settore del disco avviabile (es. `/dev/sda`).
* **Caratteristiche:** Ha una dimensione ridotta (512 byte).
* **Compito:** Contiene le informazioni primarie per caricare il boot loader vero e proprio (GRUB).

#### 3. GRUB (Grand Unified Bootloader)
Il GRUB permette all'utente di scegliere quale kernel eseguire, opzione utile se nel sistema sono presenti versioni multiple.
* **Configurazione:** Il file principale risiede solitamente in `/boot/grub/grub.cfg`.
* **Compito:** Carica l'immagine del Kernel selezionata nella memoria RAM.

#### 4. Kernel
Una volta caricato, il Kernel ha il compito di inizializzare l'hardware e montare il file system radice (*root file system*).
* **Inizializzazione:** Utilizza il file `initrd` (Initial RAM Disk) come file system temporaneo finché non viene montato quello reale.
* **Compito:** Esegue il primo processo del sistema, situato in `/sbin/init`.

#### 5. Init
Il processo `init` legge il file `/etc/inittab` per determinare il livello di esecuzione (**Run Level**) del sistema.
* **Livelli principali:**
    * `0`: Arresto del sistema (Halt).
    * `1`: Modalità utente singolo (Single-user mode).
    * `3`: Modalità multi-utente testuale (Full multi-user mode).
    * `5`: Modalità multi-utente con interfaccia grafica (X11).
    * `6`: Riavvio (Reboot).

#### 6. Run Level (Programmi rc.d)
In base al Run Level scelto, il sistema esegue gli script di avvio contenuti nelle directory corrispondenti (es. `/etc/rc.d/rc5.d/` per il livello 5).
* **Esempio:** Nel livello 5, vengono avviati i servizi di rete e l'interfaccia grafica (GUI).

---

### Verifica del Run Level da Terminale

È possibile verificare in tempo reale il livello operativo del sistema tramite la riga di comando.

1.  **Verifica livello corrente:**
    ```bash
    who -r
    ```
    *Se il sistema restituisce "run-level 5", significa che l'ambiente grafico è attivo.*

2.  **Esplorazione degli script di avvio:**
    Navigando nella directory `/etc/rc.d/rc5.d/`, è possibile visualizzare l'ordine con cui i servizi (come il network o la console grafica) vengono avviati.



---

# Il Processo di Boot nelle Versioni Moderne di Linux (systemd)

Nelle versioni più recenti delle distribuzioni Linux, come **CentOS 7+** e **Red Hat Enterprise Linux (RHEL) 7+**, il processo di avvio ha subito cambiamenti significativi rispetto alle versioni precedenti (5 e 6). Comprendere questa sequenza è fondamentale per ogni amministratore di sistema, poiché permette di diagnosticare e risolvere efficacemente i problemi quando il sistema non riesce ad avviarsi correttamente.

Il protagonista di questa evoluzione è **systemd**, il nuovo gestore di servizi che si occupa della sequenza di boot, pur mantenendo la compatibilità con i vecchi script di System V.

---

### Le 5 Fasi del Processo di Avvio

#### 1. Firmware (BIOS/UEFI)
All'accensione della macchina, viene eseguito il firmware preinstallato dal produttore (BIOS o UEFI). Questo software è indipendente dal sistema operativo.
* **POST (Power-On Self-Test):** Il firmware esegue un controllo automatico di tutto l'hardware collegato (CPU, RAM, dischi, ecc.) per assicurarne il corretto funzionamento.

#### 2. MBR (Master Boot Record)
Una volta superato il test POST, il firmware legge il primo settore del disco fisso, noto come **MBR**.
* **Ruolo:** L'MBR contiene le istruzioni che indicano al sistema dove si trova il boot loader (**GRUB2**) affinché possa essere caricato nella memoria RAM.

#### 3. GRUB2 (Grand Unified Bootloader version 2)
GRUB2 è il boot loader predefinito. Il suo compito principale è caricare il Kernel Linux.
* **Configurazione:** Legge il file `/boot/grub2/grub.cfg`.
* **Interazione:** Durante questa fase, è possibile visualizzare una schermata nera con l'elenco dei kernel disponibili. L'utente può selezionare manualmente quale versione avviare o lasciare che il sistema proceda automaticamente secondo la configurazione di GRUB.

#### 4. Kernel
Il Kernel è il cuore del sistema operativo. Una volta avviato da GRUB2, esegue le seguenti operazioni:
* **Caricamento Driver:** Estrae i driver necessari dal file `initrd.img` (Initial RAM Disk). Questi driver permettono al Kernel di comunicare con l'hardware (dischi, controller, ecc.).
* **Avvio di systemd:** Una volta inizializzato l'hardware, il Kernel avvia il primo processo del sistema operativo: **systemd**.

#### 5. systemd
**systemd** è il primo processo (Process ID 1) e il "padre" di tutti gli altri processi che verranno avviati in seguito.
* **Esecuzione processi:** Avvia tutti i servizi e i demoni abilitati al boot.
* **Target e Run Level:** Legge il file `/etc/systemd/system/default.target` per determinare lo stato finale del sistema (Run Level). Esistono 7 livelli (da 0 a 6) che definiscono se il sistema debba avviarsi in modalità utente singolo, multi-utente con rete o interfaccia grafica.

---

### Riepilogo per la Risoluzione dei Problemi

Conoscere questa sequenza permette di isolare il problema:
1.  **Schermo nero/nessun segno:** Possibile problema di Firmware o hardware.
2.  **Errore "Boot device not found":** Problema relativo all'MBR o alla tabella delle partizioni.
3.  **Menu di GRUB mancante o errori di caricamento kernel:** Problema nel file `grub.cfg`.
4.  **Kernel Panic/Blocco caricamento driver:** Problema legato al Kernel o a `initrd`.
5.  **Servizi che non partono/Login non disponibile:** Problema legato a `systemd` o ai target di avvio.



---

# Analisi delle Prestazioni di Avvio con `systemd-analyze`

Il comando **systemd-analyze** è uno strumento integrato nel pacchetto `systemd` utilizzato per esaminare le statistiche sulle prestazioni all'avvio del sistema. Questo tool permette di recuperare informazioni sullo stato del sistema, tracciare i tempi di esecuzione del gestore dei servizi e verificare la correttezza dei file di unità (*unit files*).

Per un amministratore di sistema, l'uso di `systemd-analyze` è fondamentale per identificare i colli di bottiglia che rallentano il boot e ottimizzare l'efficienza operativa.

---

### Funzionalità Principali

Lo strumento permette di scomporre il tempo di avvio in diverse sezioni chiave:
* **Kernel:** Il tempo impiegato dal nucleo del sistema operativo per inizializzarsi.
* **initrd (Initial RAM Disk):** Il tempo speso nel file system temporaneo utilizzato per preparare l'hardware.
* **User space:** Il tempo impiegato per caricare le applicazioni e i servizi utente dopo l'avvio del kernel.

Inoltre, il comando è in grado di identificare la **"critical chain"** (catena critica), ovvero la sequenza di servizi che influisce maggiormente sul tempo totale di avvio.

---

### Comandi e Parametri Comuni

Per esplorare tutte le potenzialità dello strumento, si consiglia la consultazione del manuale:
```bash
man systemd-analyze
```

#### 1. Analisi del tempo totale
Per ottenere un riepilogo immediato del tempo impiegato dal sistema per raggiungere la piena operatività, viene utilizzato il parametro `time`. Questo comando fornisce una panoramica chiara della distribuzione del carico temporale tra i diversi stadi del boot:

```bash
systemd-analyze time
```

* **Analisi dei risultati:** L'output potrebbe riportare, ad esempio, un tempo totale di 25 secondi, dettagliando la frazione temporale trascorsa nel **kernel** (es. 2s), nell'**initrd** (es. 5s) e nello **user space** (es. 18s).
* **Significato dello User Space:** Tale termine indica l'area di memoria e l'ambiente operativo in cui vengono eseguiti i processi e le applicazioni dell'utente, mantenuti logicamente separati dal kernel per garantire la stabilità del sistema.

#### 2. Identificazione dei servizi critici (Blame)
Attraverso il parametro `blame`, viene generata una lista dei servizi avviati durante il boot, ordinati in modo decrescente in base al tempo richiesto per l'inizializzazione. Questo strumento risulta essenziale per individuare i colli di bottiglia che rallentano l'avvio.

```bash
systemd-analyze blame
```

* **Valutazione dei servizi:** Qualora un servizio, come ad esempio `firewalld.service`, richiedesse 860ms per completare l'avvio, esso verrebbe elencato tra le voci che contribuiscono al tempo totale dello user space. L'analisi può estendersi a decine di pagine, permettendo una revisione meticolosa di ogni singola unità.

#### 3. Filtro per istanza utente
In scenari in cui occorra analizzare esclusivamente i processi avviati per l'utente corrente, si ricorre all'opzione `--user` posizionata al termine del comando.

```bash
systemd-analyze blame --user
```

Questa modalità permette di filtrare i processi specifici della sessione utente, mostrandone i rispettivi tempi di avvio e isolandoli dai servizi globali di sistema.

---

### Breve Storia e Diffusione
* **2010:** Lo strumento viene sviluppato come parte del progetto `systemd`, avviato da Lennart Poettering.
* **2014:** Il comando viene incluso ufficialmente in **Red Hat Enterprise Linux 7** e **CentOS 7**.
* **Stato attuale:** `systemd-analyze` rimane una componente integrante della suite `systemd`, ampiamente utilizzata per l'analisi e l'ottimizzazione del processo di boot nelle moderne distribuzioni Linux.

---

### Conclusioni e Diagnostica
L'utilizzo di `systemd-analyze` rappresenta una competenza tecnica fondamentale per la risoluzione dei problemi (*troubleshooting*). La capacità di identificare e gestire i servizi che ritardano l'avvio permette di ottimizzare le prestazioni complessive del sistema e di ridurre i tempi di inattività. Durante un colloquio tecnico, la menzione di questo strumento è considerata un indicatore di competenza avanzata nella gestione dei sistemi Linux.

---

# Il Messaggio del Giorno (MOTD) in Linux

Il **Message of the Day (MOTD)** è un messaggio testuale che viene visualizzato sul terminale ogni volta che un utente effettua il login in una macchina Linux. Viene comunemente utilizzato per fornire messaggi di benvenuto, indicare il nome dell'host o mostrare avvisi di sicurezza importanti.

In contesti aziendali o su server remoti, il MOTD è spesso impiegato per informare gli amministratori che il sistema è critico e che ogni comando eseguito potrebbe essere registrato, scoraggiando al contempo gli accessi non autorizzati.

---

### Localizzazione e Configurazione

Il file responsabile di questa funzione si trova nel percorso:
`cat /etc/motd`

Il nome del file è l'acronimo di **Message Of The Day**. Di default, questo file è spesso vuoto o contiene informazioni standard della distribuzione.

---

### Procedura di Modifica

Per personalizzare il messaggio, è necessario intervenire sul file con privilegi di amministrazione. Di seguito i passaggi operativi:

1.  **Apertura del file:** Utilizzare un editor di testo (ad esempio `vi` o `nano`):
    ```bash
    vi /etc/motd
    ```

2.  **Inserimento del testo:** È possibile digitare qualunque avviso o informazione utile. Un esempio tipico di messaggio di sicurezza è il seguente:
    > Questa è la prima macchina Linux di test.
    > Utilizzare il sistema solo per scopi autorizzati.
    > Se non si dispone dell'autorizzazione, si prega di uscire immediatamente.

3.  **Salvataggio e chiusura:** In `vi`, l'operazione si completa premendo `ESC`, digitando `:wq!` e premendo `Invio`.

---

### Verifica del Funzionamento

Per verificare che il messaggio venga visualizzato correttamente, occorre simulare un nuovo accesso al sistema, ad esempio tramite una sessione SSH (utilizzando client come PuTTY).

1.  **Individuazione dell'IP:** Verificare l'indirizzo IP del server tramite il comando `ifconfig` o `ip addr`.
2.  **Connessione remota:** Avviare una nuova sessione verso l'indirizzo IP identificato.
3.  **Login:** Dopo aver inserito le credenziali (utente e password), il testo precedentemente salvato in `/etc/motd` apparirà immediatamente a schermo prima del prompt dei comandi.



---

# Personalizzazione Avanzata del Messaggio del Giorno (MOTD)

Oltre alla semplice modifica del file di testo statico `/etc/motd`, è possibile personalizzare il messaggio visualizzato all'accesso in modo che esegua comandi e mostri informazioni dinamiche (come il nome dell'host, la versione del kernel o l'utente corrente). Questo approccio è utile per gestire più macchine contemporaneamente utilizzando un unico script standardizzato.

---

### Procedura Operativa in 4 Fasi

#### 1. Creazione dello Script di Messaggio
Invece di modificare il file statico, viene creato uno script shell nella directory dei profili di sistema.
* **Percorso:** `/etc/profile.d/motd.sh`
* **Contenuto dello script:**
  ```bash
  #!/bin/bash
  echo -e "
  #######################################################
  # Benvenuto su: `hostname`
  # Sistema Operativo: `cat /etc/redhat-release`
  # Versione Kernel: `uname -r`
  # Accesso effettuato come: `whoami`
  #######################################################
  "
  ```

> **Nota tecnica:** I comandi inseriti tra i simboli di backtick ( \` ) vengono eseguiti dal sistema e il loro output viene integrato direttamente nel messaggio. Questo processo, noto come sostituzione di comando, permette di rendere il testo informativo e dinamico.

#### 2. Backup e Modifica della Configurazione SSH
Per evitare la duplicazione dei messaggi o conflitti tra il vecchio contenuto statico e il nuovo script dinamico, è necessario istruire il demone SSH affinché non mostri il file MOTD predefinito.

* **Creazione di un backup di sicurezza:** Prima di procedere alla modifica di file di configurazione critici, si consiglia vivamente di crearne una copia di riserva per consentire un eventuale ripristino:
    ```bash
    cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
    ```
* **Modifica del file di configurazione:** Tramite un editor di testo (come `vi`), occorre accedere al file `/etc/ssh/sshd_config`. Si deve individuare la direttiva `PrintMotd` (solitamente commentata con `#`) e impostarla esplicitamente su `no`:
    ```text
    PrintMotd no
    ```

#### 3. Riavvio del Servizio SSH
Affinché le nuove impostazioni vengano caricate dal sistema, si deve procedere al riavvio del demone SSH:
```bash
systemctl restart sshd.service
```

#### 4. Verifica dell'Accesso
Per confermare la corretta esecuzione della procedura, è necessario avviare una nuova sessione SSH (ad esempio tramite PuTTY o terminale remoto). Al momento del login, il sistema eseguirà lo script contenuto in `/etc/profile.d/motd.sh`, visualizzando dinamicamente le informazioni aggiornate (hostname, versione del kernel, utente corrente) invece del semplice testo statico.



---

### Vantaggi della Personalizzazione Dinamica
* **Scalabilità:** Lo stesso script può essere distribuito su un intero parco macchine; ogni server visualizzerà automaticamente i propri parametri univoci (come il proprio hostname) senza necessità di interventi manuali sui singoli file.
* **Monitoraggio immediato:** È possibile integrare nello script comandi aggiuntivi per visualizzare all'accesso informazioni critiche, come lo spazio residuo sui dischi (`df -h`) o l'utilizzo della memoria RAM (`free -m`).
* **Standardizzazione:** Permette di mantenere un'immagine professionale e coerente su tutti i terminali aziendali, includendo avvisi legali o istruzioni operative uniformi per tutti gli utenti.

---

# Tipologie di Archiviazione Informatica

L'archiviazione informatica è fondamentale per la conservazione di dati e informazioni. Nel settore IT, si distinguono principalmente quattro categorie di storage, differenziate per modalità di connessione e ambito di utilizzo.

---

### 1. Archiviazione Locale (Local Storage)
Si definisce archiviazione locale tutto ciò che risiede all'interno del computer o del PC ed è collegato direttamente alla scheda madre.
* **Esempi:** RAM (memoria volatile), Hard Disk Drive (HDD), Solid State Drive (SSD) e memoria Cache.
* **Caratteristica:** Questi componenti sono integrati o collegati fisicamente ai connettori interni della scheda madre.

### 2. DAS (Direct Attached Storage)
Il DAS identifica dispositivi di archiviazione collegati esternamente e direttamente al computer, senza l'intermediazione di una rete.
* **Esempi:** Unità CD/DVD, chiavette USB (Flash Drive) e dischi rigidi esterni.
* **Connessione:** Avviene solitamente tramite interfacce standard come USB o cavi SCSI.
* **Vantaggio:** Il sistema rileva immediatamente l'unità non appena viene inserita o collegata.

---

### 3. SAN (Storage Area Network)
La SAN è una soluzione di archiviazione di livello aziendale (*corporate*) che permette di collegare storage remoti tramite tecnologie ad alta velocità.
* **Tecnologia:** Viene utilizzata la fibra ottica o il protocollo iSCSI.
* **Hardware richiesto:** Il computer deve essere dotato di schede specifiche chiamate **HBA** (Host Bus Adapter), installate sulla scheda madre.
* **Funzionamento:** Una volta collegati i cavi in fibra e installati i driver, lo storage viene "scoperto" dal sistema e identificato tramite un numero univoco denominato **WWN** (World Wide Name).



### 4. NAS (Network-Attached Storage)
Il NAS è un dispositivo di archiviazione collegato alla rete locale che comunica tipicamente attraverso il protocollo **TCP/IP**.
* **Caratteristica principale:** Lo storage è accessibile tramite un **indirizzo IP**, esattamente come se fosse un computer indipendente all'interno della rete.
* **Protocolli comuni:** Viene utilizzato principalmente tramite Samba (per ambienti Windows) o NFS (Network File System per ambienti Unix/Linux).
* **Configurazione:** Che l'unità contenga un singolo disco o dieci, essa risulterà sempre raggiungibile puntando al suo indirizzo IP specifico.



---

### Riepilogo Comparativo

| Tipo | Connessione | Ambito | Identificativo |
| :--- | :--- | :--- | :--- |
| **Locale** | Interna (SATA/NVMe) | Personale | Device Path |
| **DAS** | Diretta Esterna (USB) | Personale/Ufficio | Device Path |
| **SAN** | Fibra / iSCSI | Aziendale (Data Center) | WWN / LUN |
| **NAS** | Rete (Ethernet) | Aziendale / Domestico | Indirizzo IP |

# Visualizzazione e Gestione delle Partizioni del Disco in Linux

L'analisi del file system e delle partizioni è fondamentale per monitorare l'occupazione dello storage e gestire i volumi locali. In questa lezione vengono approfonditi due comandi essenziali: `df` e `fdisk`.

---

### 1. Il comando `df` (Disk Free)
Il comando `df` viene utilizzato per visualizzare le informazioni relative allo spazio su disco dei file system montati.

* **Utilizzo base:** Digitando semplicemente `df`, vengono mostrate diverse colonne:
    * **File system:** L'origine del file system.
    * **Size/Used/Available:** Dimensioni totali, spazio utilizzato e spazio disponibile.
    * **Percentage:** Percentuale di utilizzo.
    * **Mounted on:** Il punto di montaggio nel sistema.
* **Modalità leggibile:** L'opzione `-h` (**human-readable**) converte i valori in formati facilmente comprensibili come Gigabyte (G) o Megabyte (M).
    ```bash
    df -h
    ```



---

### 2. Il comando `fdisk`
A differenza di `df`, che analizza l'utilizzo del file system, `fdisk` permette di visualizzare la struttura fisica del disco e delle partizioni create.

* **Visualizzazione delle partizioni:** Per elencare i dischi e le relative tabelle delle partizioni, si utilizza l'opzione `-l`.
    ```bash
    fdisk -l
    ```
* **Analisi dell'output:**
    * Viene mostrata la dimensione totale del disco (ad esempio, `/dev/sda` da 10 GB).
    * Vengono elencate le singole partizioni, come ad esempio `/dev/sda1` o volumi logici gestiti tramite LVM.
    * È possibile identificare partizioni specifiche come la **swap** (utilizzata per la memoria virtuale) e la partizione **root** (dove risiede il sistema operativo).



---

### Applicazioni Pratiche
Questi comandi risultano estremamente utili nelle seguenti situazioni:
1.  **Aggiunta di nuovi dischi:** Quando si inserisce un nuovo supporto fisico nel sistema.
2.  **Inizializzazione:** Per preparare il disco all'uso.
3.  **Partizionamento:** Per suddividere un disco fisico in più unità logiche.
4.  **Montaggio:** Per collegare le partizioni a punti di montaggio specifici nella gerarchia delle directory.

---

# Aggiunta di un Disco e Creazione di una Nuova Partizione in Linux

In questa lezione viene illustrato come aggiungere un nuovo disco rigido a un sistema Linux esistente, creare una partizione, formattarla con un file system e montarla correttamente.

---

### 1. Preparazione dell'Ambiente Virtuale (VirtualBox)

Prima di operare sul sistema operativo, è necessario aggiungere l'hardware virtuale. 
> **Importante:** Si consiglia vivamente di creare uno **Snapshot** della macchina virtuale prima di procedere. Errori nella configurazione del disco (specialmente nel file `fstab`) possono impedire il corretto avvio del sistema.

**Passaggi in VirtualBox:**
1.  Spegnere la macchina virtuale.
2.  Accedere a **Impostazioni** -> **Archiviazione**.
3.  Selezionare il **Controller SATA** e cliccare sull'icona "Aggiungi Disco".
4.  Scegliere **Crea nuovo disco**, tipo **VDI**, **Allocato dinamicamente**.
5.  Impostare la dimensione (es. **2.00 GB**) e assegnare un nome (es. `datadisk`).
6.  Salvare e avviare la macchina virtuale.



---

### 2. Identificazione e Partizionamento del Nuovo Disco

Una volta avviato il sistema e ottenuto l'accesso come `root`, è necessario identificare il nuovo dispositivo.

* **Identificazione:** Eseguire `fdisk -l`. Il nuovo disco apparirà solitamente come `/dev/sdb` (se il disco principale è `/dev/sda`).
* **Creazione della partizione:** Utilizzare l'utility `fdisk` sul nuovo dispositivo:
    ```bash
    fdisk /dev/sdb
    ```
    All'interno del menu interattivo di `fdisk`, seguire questa sequenza di comandi:
    1.  **n**: Crea una nuova partizione.
    2.  **p**: Seleziona partizione primaria.
    3.  **1**: Assegna il numero di partizione.
    4.  **Invio** (due volte): Accetta i valori predefiniti per il primo e l'ultimo settore (utilizza l'intero disco).
    5.  **w**: Scrive le modifiche sul disco ed esce (fino a questo momento le modifiche sono solo in memoria).

---

### 3. Formattazione e Montaggio

Una partizione non è utilizzabile finché non riceve un file system.

* **Formattazione (XFS):** Creare il file system sulla nuova partizione `/dev/sdb1`:
    ```bash
    mkfs.xfs /dev/sdb1
    ```
* **Creazione del Punto di Montaggio:** Creare una directory che ospiterà i dati:
    ```bash
    mkdir /data
    ```
* **Montaggio temporaneo:** Collegare la partizione alla directory:
    ```bash
    mount /dev/sdb1 /data
    ```
    Verificare con `df -h` che la nuova unità sia visibile e montata correttamente.

---

### 4. Configurazione Persistente (`/etc/fstab`)

Per far sì che il disco venga montato automaticamente a ogni riavvio, è necessario modificare il file `/etc/fstab`.

1.  Aprire il file: `vi /etc/fstab`
2.  Aggiungere una riga in fondo al file seguendo questa sintassi:
    ```text
    /dev/sdb1    /data    xfs    defaults    0 0
    ```
3.  Salvare e uscire (`:wq`).



**Verifica della persistenza:**
* Per testare la configurazione senza riavviare, è possibile smontare il disco con `umount /data` e poi eseguire `mount -a`. Se il comando non restituisce errori e il disco viene montato, la configurazione è corretta.

---

### Conclusioni e Troubleshooting
Se dopo il riavvio il sistema non dovesse caricarsi, significa che è presente un errore di battitura in `/etc/fstab`. In tal caso, è possibile ripristinare il sistema tramite lo snapshot creato precedentemente in VirtualBox.

---

# Introduzione a LVM (Logical Volume Management)

Il **Logical Volume Management (LVM)** è un sistema di gestione del disco basato su software che offre una flessibilità notevolmente superiore rispetto al partizionamento tradizionale. LVM funge da strato di astrazione tra l'hardware fisico del disco e il sistema operativo.

---

### Architettura e Funzionamento

LVM permette di combinare più dischi fisici in un unico pool di archiviazione. Per comprendere il funzionamento, si può analizzare la gerarchia strutturale:

1.  **Volume Fisico (PV - Physical Volume):** Rappresenta il disco rigido o la partizione fisica (es. un disco da 100 GB).
2.  **Gruppo di Volumi (VG - Volume Group):** È un contenitore creato unendo uno o più Volumi Fisici. Agisce come un "disco virtuale" unico.
3.  **Volume Logico (LV - Logical Volume):** Sono le "partizioni virtuali" create all'interno del Gruppo di Volumi. Queste vengono poi formattate e montate nel sistema (es. `/`, `/home`, `swap`).



---

### Esempio di Configurazione Multidisco

Si consideri uno scenario con tre dischi fisici da 100 GB ciascuno:
* Invece di gestire tre unità separate, i tre dischi vengono combinati in un unico **Volume Group** (ad esempio denominato `datavg`).
* La dimensione totale di questo gruppo diventa **300 GB**.
* Da questo pool di 300 GB, si possono creare diversi **Volumi Logici** (es. `data1`, `data2`, `data3`, `data4`) con dimensioni arbitrarie.

---

### Vantaggi Principali di LVM

Il beneficio principale risiede nella **scalabilità dinamica**:

* **Estensione a caldo:** Qualora lo spazio nel Volume Group (300 GB) dovesse esaurirsi, è possibile aggiungere un quarto disco (es. altri 100 GB) al pool senza interrompere il funzionamento del sistema o smontare i file system. Il gruppo passerà istantaneamente a 400 GB.
* **Ridimensionamento dei volumi:** Se un particolare punto di montaggio (come la partizione di root `/`) raggiunge il 99% della capacità, è possibile espandere quel Volume Logico specifico attingendo allo spazio libero disponibile nel Gruppo di Volumi.
* **Gestione in Ambienti Corporate:** In contesti aziendali o server, dove le applicazioni e i database possono saturare lo spazio rapidamente, LVM evita la necessità di migrazioni complesse o reinstallazioni del sistema dovute a limiti di partizionamento fisico.



---

### LVM in Ambienti Virtuali
In ambiente virtuale, la flessibilità è ancora maggiore: è possibile espandere la dimensione del disco virtuale direttamente dall'hypervisor e, successivamente, utilizzare LVM per estendere il gruppo di volumi e le partizioni logiche lato sistema operativo, portando ad esempio una partizione da 100 GB a 200 GB in pochi passaggi.

---

# Configurazione di LVM durante l'Installazione di Linux

In questa lezione viene illustrata la procedura guidata per configurare il **Logical Volume Management (LVM)** direttamente durante l'installazione del sistema operativo CentOS 7. Questa modalità permette di strutturare lo storage in modo flessibile sin dal primo avvio.

---

### 1. Preparazione della Macchina Virtuale
Per procedere all'installazione, è necessario creare un nuovo ambiente virtuale:
* **Creazione VM:** Creare una nuova macchina virtuale (es. denominata `LVM_Linux_OS`).
* **Specifiche:** Assegnare 1 GB di RAM e un disco virtuale da 8 GB.
* **ISO:** Collegare l'immagine ISO di CentOS 7 e avviare l'installazione selezionando la voce "Install CentOS 7".

### 2. Configurazione del Partizionamento Personalizzato
Dopo aver impostato lingua, tastiera e fuso orario, occorre accedere alla sezione **Installation Destination** per definire la struttura del disco.

* **Selezione del Disco:** Selezionare il disco virtuale da 8 GB.
* **Opzione Partizionamento:** Invece della configurazione automatica, selezionare la voce **"I will configure partitioning myself"** (Configurerò il partizionamento manualmente).
* **Schema di Partizionamento:** Assicurarsi che nel menu a tendina sia selezionato **LVM**.



### 3. Creazione dei Mount Point LVM
Utilizzando il tasto **"+"**, è possibile aggiungere i vari punti di montaggio. In un ambiente professionale, è prassi comune separare le directory principali in volumi logici distinti:

| Mount Point | Capacità | Tipo di Device | Note |
| :--- | :--- | :--- | :--- |
| `/home` | 2 GB | LVM | Destinato ai dati degli utenti. |
| `/var` | 2 GB | LVM | Destinato a log e file variabili. |
| `swap` | ~1 GB | LVM | Memoria virtuale utilizzata in caso di saturazione RAM. |
| `/boot` | 500 MB | **Standard Partition** | **Importante:** La partizione di boot non può essere di tipo LVM. |
| `/` (root) | Restante | LVM | Sistema operativo e programmi. |

> **Nota Tecnica:** Per assegnare tutto lo spazio rimanente a un volume (come la root), è possibile digitare la parola "all" nel campo della capacità.

### 4. Verifica e Completamento
Una volta definiti i volumi, cliccare su "Done" e accettare le modifiche per scrivere la tabella delle partizioni. Proseguire con l'impostazione della password di root e attendere il completamento dell'installazione.

---

### Analisi Post-Installazione
Dopo il riavvio, è possibile verificare la struttura tramite il comando:
```bash
df -h
```

# Analisi dei Comandi di Amministrazione LVM

Una volta completata l'installazione del sistema con il supporto **LVM (Logical Volume Management)**, è fondamentale conoscere i comandi che permettono di visualizzare e gestire la gerarchia dello storage direttamente dalla riga di comando.

---

### Comandi di Visualizzazione Rapida

Esistono tre comandi principali che forniscono una panoramica immediata dei tre livelli di astrazione di LVM. Questi comandi terminano tutti con la lettera "s" (che sta per *scan* o *summary*):

* **`pvs` (Physical Volume Scan):** Mostra i volumi fisici (dischi o partizioni reali) che sono stati inizializzati per LVM. Permette di vedere la dimensione totale del disco fisico e lo spazio libero ancora disponibile per il Gruppo di Volumi.
* **`vgs` (Volume Group Scan):** Fornisce informazioni sui gruppi di volumi. È utile per identificare quanti dischi fisici compongono un gruppo e quanti volumi logici sono stati creati al suo interno.
* **`lvs` (Logical Volume Scan):** Elenca tutti i volumi logici creati (come `root`, `home`, `var`). Mostra la loro dimensione attuale e il gruppo di appartenenza.



---

### Differenza tra i Percorsi dei Dispositivi

Quando si utilizza LVM, l'accesso ai dati avviene tramite percorsi specifici che differiscono dalle partizioni standard:

1.  **Standard:** Una partizione classica viene identificata come `/dev/sda1`.
2.  **LVM:** Un volume logico è generalmente accessibile tramite il mappatore dei dispositivi, con percorsi simili a `/dev/mapper/centos-root` o `/dev/vg_name/lv_name`.

Questa struttura è ciò che permette al sistema operativo di ridimensionare il "disco" virtuale senza dover modificare la tabella delle partizioni fisica sottostante.

---

### Vantaggi Operativi

L'uso di questi comandi di scansione è il primo passo per operazioni avanzate come:
* **Monitoraggio:** Controllare in tempo reale se è rimasto spazio libero nel **Volume Group** per future espansioni.
* **Manutenzione:** Identificare rapidamente quale disco fisico (`PV`) sta ospitando un determinato volume logico.
* **Flessibilità:** Verificare la configurazione prima di procedere all'estensione di un file system con il comando `lvextend`.

---

# Configurazione Pratica di LVM: Aggiunta Disco e Creazione Partizioni

In questa lezione viene illustrato il processo completo per aggiungere un nuovo disco fisico e configurare una partizione tramite **LVM (Logical Volume Manager)**. Il flusso di lavoro segue una gerarchia precisa che trasforma l'hardware grezzo in uno spazio di archiviazione flessibile e utilizzabile dal sistema.

---

### La Gerarchia LVM
Per configurare correttamente LVM, è necessario seguire questi passaggi in ordine sequenziale:

1.  **Disco Fisico (Hard Disk):** L'unità hardware aggiunta al sistema (es. `/dev/sdc`).
2.  **Partizione:** Creazione di una partizione fisica sul disco con identificativo specifico per LVM.
3.  **Volume Fisico (PV):** Inizializzazione della partizione per l'utilizzo in LVM.
4.  **Gruppo di Volumi (VG):** Unione di uno o più volumi fisici in un pool di archiviazione.
5.  **Volume Logico (LV):** Creazione della partizione "virtuale" all'interno del gruppo.
6.  **File System e Montaggio:** Formattazione del volume logico e collegamento a una directory.



---

### Fase 1: Aggiunta del Disco (VirtualBox)
Prima di operare su Linux, occorre aggiungere il disco tramite l'hypervisor:
1. Spegnere la VM e andare in **Impostazioni > Archiviazione**.
2. Selezionare il **Controller SATA** e cliccare su **Aggiungi Disco Fisico**.
3. Creare un nuovo disco (es. `discoracle.vdi`) di **1 GB**.
4. Avviare la macchina virtuale.

---

### Fase 2: Configurazione della Partizione Fisica
Una volta loggati come root, verificare la presenza del disco con `fdisk -l` (sarà identificato come `/dev/sdc`).

```bash
fdisk /dev/sdc
```

### Procedura Dettagliata per la Creazione della Partizione LVM

Dopo aver avviato l'utility `fdisk` sul disco desiderato (es. `/dev/sdc`), è necessario seguire i passaggi operativi riportati di seguito per preparare la struttura fisica all'integrazione con LVM:

1.  **Creazione:** Digitare **n** per generare una nuova partizione.
2.  **Selezione Tipo:** Digitare **p** per impostare la partizione come primaria.
3.  **Numerazione:** Digitare **1** (o premere Invio) per assegnare il primo numero di partizione disponibile.
4.  **Dimensionamento:** Premere **Invio** per accettare i valori predefiniti relativi al primo e all'ultimo settore; in questo modo verrà allocato l'intero spazio disponibile sul disco.
5.  **Identificazione del Tipo (Passaggio Critico):**
    * Digitare **t** per modificare l'ID del sistema della partizione.
    * Inserire il codice esadecimale **8e**. Questo codice identifica specificamente la partizione come **Linux LVM**, permettendo al software di gestione dei volumi di riconoscerla correttamente.
6.  **Verifica:** Digitare **p** per visualizzare la tabella delle partizioni creata e confermare che sotto la colonna "Id" compaia "8e" e sotto "System" la dicitura "Linux LVM".
7.  **Scrittura:** Digitare **w** per salvare permanentemente le modifiche sulla tabella delle partizioni del disco e uscire.

---

### Inizializzazione dei Componenti LVM

Una volta preparata la partizione fisica, si procede alla creazione della struttura logica tramite i seguenti comandi:

* **Creazione del Volume Fisico (PV):** Viene inizializzata la partizione per essere utilizzata da LVM.
    ```bash
    pvcreate /dev/sdc1
    ```
* **Creazione del Gruppo di Volumi (VG):** Si aggrega il volume fisico in un gruppo denominato `oracle_vg`.
    ```bash
    vgcreate oracle_vg /dev/sdc1
    ```
* **Creazione del Volume Logico (LV):** Si definisce il volume finale `oracle_lv`. È consigliabile specificare una dimensione leggermente inferiore alla capacità totale del VG (ad esempio **1000M** invece di 1024M) per garantire che vi sia spazio sufficiente per i metadati LVM.
    ```bash
    lvcreate -n oracle_lv -L 1000M oracle_vg
    ```

---

### Formattazione e Accesso ai Dati

Per rendere il volume logico utilizzabile dal sistema operativo, è necessario applicare un file system e definire un punto di accesso:

1.  **Formattazione XFS:**
    ```bash
    mkfs.xfs /dev/oracle_vg/oracle_lv
    ```
2.  **Montaggio:**
    Creare la directory di destinazione e montare il volume:
    ```bash
    mkdir /oracle
    mount /dev/oracle_vg/oracle_lv /oracle
    ```

# Estensione Dinamica del Disco con LVM (Logical Volume Management)

Questa lezione illustra come estendere la capacità di archiviazione di un punto di montaggio esistente (es. `/oracle`) senza dover creare nuove partizioni separate o interrompere il servizio, sfruttando la flessibilità di LVM.

---

### Perché estendere un disco?
In un ambiente di produzione, i dati (come i database Oracle) crescono costantemente. Quando un disco raggiunge il **100% della capacità**, le opzioni principali sono:
1. **Eliminare file obsoleti:** Spesso non è possibile in ambienti critici.
2. **Aggiungere un nuovo disco fisico:** In sistemi fisici richiede slot liberi e configurazioni RAID.
3. **Estensione tramite LVM:** È la soluzione ideale, specialmente in ambienti virtuali, poiché permette di unire il nuovo spazio a quello esistente in modo trasparente per l'utente.



---

### 1. Aggiunta di un Nuovo Disco Virtuale
In un ambiente virtuale (es. VirtualBox), l'aggiunta di risorse è immediata:
* Spegnere la macchina virtuale.
* Accedere a **Impostazioni > Archiviazione**.
* Aggiungere un nuovo disco rigido (es. `dataoracle2.vdi`) da **1 GB**.
* Avviare il sistema.

### 2. Preparazione della Nuova Partizione
Una volta avviato Linux, identificare il nuovo disco (solitamente `/dev/sdd`) e prepararlo:
1. Eseguire `fdisk /dev/sdd`.
2. Creare una nuova partizione (**n**, **p**, **1**, valori predefiniti).
3. Cambiare il tipo di partizione in **8e** (Linux LVM) usando il comando **t**.
4. Salvare con **w**.

### 3. Procedura di Estensione LVM
Per raddoppiare lo spazio da 1 GB a 2 GB, occorre integrare il nuovo disco (`/dev/sdd1`) nel sistema LVM esistente.

#### A. Inizializzazione del Volume Fisico (PV)
```bash
pvcreate /dev/sdd1
```

#### B. Estensione del Gruppo di Volumi (VG)

Una volta inizializzato il nuovo Volume Fisico, è necessario aggiungerlo al Gruppo di Volumi esistente (ad esempio `oracle_vg`) per espandere il pool di archiviazione complessivo. Questa operazione "unisce" la capacità del nuovo disco a quella del precedente.



* **Comando:**
    ```bash
    vgextend oracle_vg /dev/sdd1
    ```
* **Verifica:** L'avvenuta estensione può essere confermata tramite il comando `vgdisplay oracle_vg`, controllando che il valore "VG Size" sia aumentato coerentemente con la dimensione del nuovo disco aggiunto.

---

#### C. Estensione del Volume Logico (LV)

Con lo spazio aggiuntivo ora disponibile nel Gruppo di Volumi, si procede all'espansione del Volume Logico specifico. In questa fase si decide quanto dello spazio libero del pool debba essere assegnato alla partizione logica.

* **Comando per aggiungere una dimensione specifica (es. 1000MB):**
    ```bash
    lvextend -L +1000M /dev/mapper/oracle_vg-oracle_lv
    ```
* **Comando per utilizzare tutto lo spazio libero disponibile nel VG:**
    ```bash
    lvextend -l +100%FREE /dev/mapper/oracle_vg-oracle_lv
    ```



---

#### D. Ridimensionamento del File System (XFS Grow)

L'ultimo passaggio fondamentale consiste nell'estendere il file system affinché possa riconoscere e utilizzare il nuovo spazio allocato al Volume Logico. Poiché CentOS 7 utilizza tipicamente **XFS**, non è necessario smontare la partizione (l'operazione avviene "a caldo").

* **Comando:**
    ```bash
    xfs_growfs /dev/mapper/oracle_vg-oracle_lv
    ```

Dopo l'esecuzione, il comando `df -h` mostrerà la nuova capacità aggiornata (ad esempio, il passaggio da 1 GB a 2 GB totali per il punto di montaggio `/oracle`).

---

# Gestione dello Spazio di Swap in Linux

Questa lezione approfondisce il concetto di **spazio di swap**, la sua utilità nel sistema operativo Linux e le procedure operative per estenderlo tramite la creazione di un file di swap dedicato.

---

### Che cos'è lo Spazio di Swap?
Secondo la definizione ufficiale di CentOS.org, lo swap viene utilizzato quando la memoria fisica (**RAM**) è esaurita. Se il sistema richiede ulteriori risorse di memoria ma la RAM è piena, le "pagine" di memoria inattive vengono spostate nello spazio di swap.

* **Natura:** Lo swap risiede sul disco rigido, che ha tempi di accesso decisamente più lenti rispetto alla RAM fisica.
* **Funzione:** È una "memoria virtuale" che funge da rete di sicurezza, ma non deve essere considerata un sostituto permanente della RAM.
* **Terminologia:** In ambiente Windows è nota come "memoria virtuale", mentre in Linux è definita "swap".



---

### Regole per il Dimensionamento
Esiste una regola generale (rule of thumb) per determinare quanto spazio di swap allocare in base alla RAM fisica ($M$):

* Se $M < 2$ GB, lo swap ($S$) dovrebbe essere $M \times 2$.
* Se $M > 2$ GB, lo swap ($S$) dovrebbe essere $M + 2$ GB.

---

### Monitoraggio della Memoria
Prima di procedere, è necessario verificare lo stato attuale della memoria con il comando:
```bash
free -m
```

Questo comando mostra la RAM fisica totale e lo spazio di swap configurato (spesso impostato automaticamente a 1 GB durante l'installazione). Se l'utilizzo di entrambi si avvicina al 100%, è necessario intervenire per evitare il blocco del sistema o rallentamenti critici.



---

### Procedura Operativa per l'Estensione dello Swap

Qualora non sia possibile aggiungere RAM fisica o incrementare le risorse tramite hypervisor, è possibile "ritagliare" una porzione dal disco fisso per dedicarla allo swap tramite la creazione di un file specifico.

#### 1. Creazione del file con il comando `dd`
Per generare un file da 1 GB (composto da 1024 blocchi da 1 MB ciascuno), si utilizza l'utility `dd`. L'operazione deve essere eseguita con privilegi di **root**:

```bash
dd if=/dev/zero of=/newswap bs=1M count=1024
```

* **`if=/dev/zero`**: Legge flussi di caratteri nulli dal dispositivo virtuale di sistema per riempire il file in fase di creazione.
* **`of=/newswap`**: Definisce il percorso e il nome del file che fungerà da nuova area di swap.
* **`bs=1M`**: Imposta la dimensione del singolo blocco di lettura e scrittura a 1 Megabyte.
* **`count=1024`**: Determina il numero totale di blocchi da scrivere, definendo così la dimensione finale del file (in questo caso, 1 GB).



---

#### 2. Configurazione dei Permessi di Sicurezza
Per garantire la stabilità del sistema, è fondamentale che solo l'utente root possa accedere a tale file. Si procede limitando i permessi di lettura e scrittura per evitare accessi non autorizzati:

```bash
chmod 600 /newswap
```

#### 3. Inizializzazione e Attivazione dello Swap

Il file generato tramite il comando `dd` non è immediatamente utilizzabile dal sistema; esso deve essere trasformato in un'area di swap valida e successivamente attivato affinché il kernel possa allocarvi le pagine di memoria inattive.

1.  **Formattazione del file (mkswap):** Si procede alla creazione della struttura necessaria per lo swap. Questo comando scrive i metadati specifici all'inizio del file.
    ```bash
    mkswap /newswap
    ```
2.  **Abilitazione del file (swapon):** Una volta formattato, il file deve essere integrato nel pool di memoria virtuale attiva del sistema.
    ```bash
    swapon /newswap
    ```



Dopo l'attivazione, è possibile verificare l'incremento dello spazio disponibile utilizzando i comandi `free -m` o `swapon -s`. Quest'ultimo mostrerà l'elenco di tutti i dispositivi e i file attualmente utilizzati come swap, inclusa la priorità e il percorso del nuovo file `/newswap`.

---

### Configurazione Permanente (Persistenza al Riavvio)

Di default, le modifiche effettuate con `swapon` vengono perse al riavvio del sistema. Per rendere l'aggiunta permanente, è necessario istruire il sistema operativo affinché carichi il file automaticamente durante la fase di boot:

1.  Aprire il file di configurazione con privilegi amministrativi: `vi /etc/fstab`.
2.  Inserire la seguente riga alla fine del file, assicurandosi di rispettare la sintassi corretta (percorso, punto di montaggio, tipo, opzioni, dump, pass):
    ```text
    /newswap    swap    swap    defaults    0 0
    ```



> **Avvertenza:** Si raccomanda di prestare massima attenzione durante la modifica di `/etc/fstab`. Errori di digitazione in questo file possono causare il fallimento del processo di avvio del sistema, richiedendo interventi in modalità di ripristino.

---

# Il Comando xfs_info in Linux

Questa lezione approfondisce l'utilizzo dell'utility **xfs_info**, uno strumento fondamentale per l'amministrazione di sistemi Linux che utilizzano il file system XFS.

---

### Ripasso del File System XFS
Prima di analizzare il comando, è opportuno ricordare che **XFS** è un file system di tipo *journaling* ad alte prestazioni. 
* **Struttura:** Organizza i dati in una gerarchia di directory e file.
* **Integrità:** Garantisce la coerenza dei dati mantenendo un log (giornale) dei cambiamenti prima che vengano effettivamente applicati al disco.
* **Efficienza:** Eccelle nella gestione di file di grandi dimensioni e nell'allocazione dinamica dello spazio.



---

### Che cos'è xfs_info?
L'utility **xfs_info** viene utilizzata per visualizzare i dettagli della "geometria" di un file system XFS esistente. 
* **Scopo:** Fornisce una panoramica dettagliata di parametri quali dimensione del blocco, struttura degli inode e layout del disco.
* **Sicurezza:** È uno strumento di sola lettura; permette di ispezionare la configurazione senza apportare alcuna modifica.
* **Utilità:** Risulta essenziale per il monitoraggio del sistema, la risoluzione dei problemi (*troubleshooting*) e la pianificazione della capacità (*capacity planning*).

#### Sintassi di base
La sintassi è estremamente semplice:
```bash
xfs_info /punto/di/montaggio_o_dispositivo
```

Esempio: `xfs_info /dev/sda1` oppure `xfs_info /` (utilizzando il punto di montaggio).

---

### Storia ed Evoluzione
* **1994:** Introduzione originale da parte di Silicon Graphics (SGI) per il sistema IRIX.
* **2001:** Porting nel kernel Linux, rendendo disponibili utility come `xfs_info`.
* **2002:** Rilascio ufficiale del pacchetto **xfsprogs**, che include gli strumenti di gestione XFS.
* **2014:** Adozione di XFS come file system predefinito per **Red Hat Enterprise Linux 7**, consolidando l'importanza di queste utility.

---

### Analisi Tecnica dell'Output
L'esecuzione del comando restituisce una panoramica strutturata in cinque sezioni fondamentali:

| Sezione | Descrizione Tecnica |
| :--- | :--- |
| **Meta-data** | Visualizza le informazioni sulla struttura del file system, inclusa la dimensione degli **inode** (es. 512 byte) e il supporto a funzionalità come **CRC** (integrità dei dati) e **reflink**. |
| **Data** | Indica la dimensione dei blocchi di dati (tipicamente 4 KB) e il numero totale di blocchi disponibili nella partizione. |
| **Naming** | Riporta la versione della convenzione dei nomi e la dimensione dei blocchi utilizzati per gestire le directory. |
| **Log** | Descrive i parametri del **journal**, come il numero di blocchi dedicati al tracciamento delle modifiche per prevenire la corruzione dei dati. |
| **Realtime** | Mostra i dettagli relativi a un'area opzionale dedicata ai dati real-time; solitamente non è utilizzata (valore *none*). |



#### Concetti Chiave dell'Architettura XFS
* **Inode:** Struttura che memorizza i metadati dei file (permessi, proprietà, locazione fisica). In XFS, gli inode sono gestiti in gruppi per ottimizzare l'accesso.
* **Allocation Groups (AG):** Il file system viene diviso in diverse regioni indipendenti. Questo permette operazioni simultanee di lettura/scrittura, migliorando drasticamente le prestazioni su sistemi multi-processore.
* **Journaling (Log):** Un registro che tiene traccia delle transazioni non ancora completate, garantendo un ripristino rapido in caso di arresto anomalo.

---

### Esercitazione Pratica: Verifica della Versione
Per identificare la versione corrente dell'utility installata nel sistema, si utilizza l'opzione `-V`:
```bash
xfs_info -V
```

# Stratis: Gestione Avanzata dello Storage in Linux

In questa lezione viene analizzata l'implementazione di **Stratis**, la soluzione di gestione dei volumi di nuova generazione introdotta con Red Hat Enterprise Linux (RHEL) 8 e disponibile anche in CentOS 8 (nonché nelle versioni 7.5 e successive).

---

### Cos'è Stratis?
Stratis è un "local storage manager" che semplifica la configurazione dello storage unificando la gestione dei volumi logici e la creazione del file system in un unico processo.

* **Thin Provisioning di serie:** Stratis alloca lo spazio solo quando è effettivamente necessario.
* **Espansione Automatica:** A differenza di LVM, dove l'estensione di un file system saturo richiede interventi manuali (`lvextend` e ridimensionamento del FS), Stratis espande automaticamente il file system finché nel pool è presente spazio libero.
* **Integrazione:** Combina le funzionalità di LVM e XFS in un'unica interfaccia semplificata.



---

### Confronto: LVM vs Stratis

| Caratteristica | LVM (Logical Volume Manager) | Stratis |
| :--- | :--- | :--- |
| **Componenti** | Physical Volumes -> Volume Group -> Logical Volume | Dischi Fisici -> Pool -> File System |
| **Estensione FS** | Manuale (richiede comandi multipli) | Automatica (fino alla capacità del pool) |
| **Provisioning** | Solitamente Thick (spazio riservato subito) | Thin (spazio allocato dinamicamente) |
| **Configurazione** | Diversi livelli di astrazione manuale | Gestione unificata tramite CLI dedicata |



---

### Guida Operativa: Workshop Pratico

#### 1. Installazione dei pacchetti
Per utilizzare Stratis, è necessario installare la riga di comando (`stratis-cli`) e il demone (`stratisd`):
```bash
dnf install stratis-cli stratisd -y
```

#### 2. Abilitazione del Servizio
Il demone deve essere configurato per l'avvio automatico al boot del sistema e attivato immediatamente per la sessione corrente:

```bash
systemctl enable --now stratisd
```

È possibile verificare il corretto funzionamento del servizio consultandone lo stato:


```bash
systemctl status stratisd
```

#### 3. Creazione e Gestione del Pool
Dopo l'aggiunta dei dischi fisici al sistema (nell'esempio, due unità da 5 GB identificate come `/dev/sdb` e `/dev/sdc`), si procede alla configurazione del pool di archiviazione.

* **Creazione del pool iniziale:**
    ```bash
    stratis pool create pool1 /dev/sdb
    ```
* **Estensione del pool:** Per incrementare lo spazio disponibile, viene aggregato il secondo disco al pool esistente tramite il comando `add-data`.
    ```bash
    stratis pool add-data pool1 /dev/sdc
    ```
* **Verifica della configurazione:** L'esecuzione di `stratis pool list` permette di visualizzare la capacità totale aggregata (pari a 10 GB), lo spazio utilizzato e quello ancora disponibile nel pool.



---

#### 4. Gestione del File System
Una volta definito il pool, viene creato il file system. Stratis semplifica questa fase gestendo internamente la formattazione e l'allocazione dinamica.

* **Creazione del file system:**
    ```bash
    stratis filesystem create pool1 fs1
    ```
* **Montaggio del volume:** Si crea una directory di destinazione e si procede al montaggio del file system appena generato.
    ```bash
    mkdir /BigData
    mount /dev/stratis/pool1/fs1 /BigData
    ```



Si osserva che il comando `df -h` potrebbe riportare una dimensione virtuale fissa (solitamente 1 TB). Questo è dovuto all'architettura di Stratis: il file system appare molto grande per permettere l'espansione automatica, ma lo spazio fisico reale viene consumato solo quando i dati vengono effettivamente scritti. Per monitorare l'occupazione reale, è necessario utilizzare `stratis filesystem list`.

---

#### 5. Snapshot e Persistenza
Stratis offre strumenti integrati per la protezione dei dati e la persistenza della configurazione al riavvio del server.

* **Creazione di uno Snapshot:** Viene generata una copia istantanea del file system per finalità di backup o test.
    ```bash
    stratis filesystem snapshot pool1 fs1 fs1-snap
    ```
* **Configurazione in `/etc/fstab`:** Per garantire che il file system sia disponibile dopo un riavvio, è necessario inserire l'UUID del volume nel file di sistema. È fondamentale includere l'opzione di montaggio `x-systemd.requires=stratisd.service` per assicurarsi che il sistema attenda l'avvio del demone Stratis prima di tentare il mount:
    ```text
    UUID=valore-uuid-recuperato /BigData xfs defaults,x-systemd.requires=stratisd.service 0 0
    ```



---

# Introduzione alla Tecnologia RAID

Il termine **RAID** è l'acronimo di *Redundant Array of Independent Disks* (insieme ridondante di dischi indipendenti). Questa tecnologia viene utilizzata principalmente per garantire la **ridondanza dei dati**: in caso di guasto a un disco rigido, il sistema può continuare a funzionare senza perdita di informazioni.

---

### Tipologie di RAID Principali

Esistono diversi livelli di RAID, ognuno con caratteristiche specifiche in termini di prestazioni e sicurezza. In questa lezione vengono analizzati i tre tipi più comuni: RAID 0, RAID 1 e RAID 5.



#### RAID 0 (Striping)
In questa configurazione, i dati vengono suddivisi equamente tra due o più dischi.
* **Vantaggi:** Elevata velocità di lettura e scrittura; la capacità totale è la somma dei singoli dischi (es. due dischi da 5 GB creano un volume da 10 GB).
* **Svantaggi:** **Nessuna ridondanza**. Se un solo disco si guasta, tutti i dati dell'array vengono persi.
* **Utilizzo:** Ideale per dati temporanei, backup non critici o data warehousing dove la velocità è prioritaria rispetto alla sicurezza.

#### RAID 1 (Mirroring)
Questa configurazione crea una copia esatta (specchio) dei dati su due o più dischi.
* **Vantaggi:** Massima sicurezza; se un disco si guasta, i dati sono immediatamente disponibili sull'altro.
* **Svantaggi:** Costo elevato (la capacità utile è pari a quella del disco più piccolo, non alla somma); le prestazioni di scrittura possono essere ridotte poiché il sistema deve replicare i dati su ogni disco.
* **Utilizzo:** Sistemi operativi e database critici dove la perdita di dati non è ammissibile.



#### RAID 5 (Striping con Parità)
Il RAID 5 richiede almeno **tre dischi** e distribuisce sia i dati che le informazioni di parità (necessarie per la ricostruzione in caso di guasto) su tutti i dischi dell'array.
* **Funzionamento:** Utilizza un meccanismo di scrittura ciclica (*round-robin*). Se un disco si rompe, le informazioni di parità sugli altri dischi permettono di ricostruire i dati mancanti.
* **Capacità:** La capacità totale è data dalla somma dei dischi meno lo spazio di un disco riservato alla parità (es. tre dischi da 5 GB offrono circa 10 GB di spazio utile).
* **Vantaggi:** Ottimo equilibrio tra prestazioni, sicurezza e capacità di archiviazione.

---

### Considerazioni per l'Amministratore di Sistema

La comprensione del RAID è fondamentale sia per affrontare colloqui tecnici, sia per l'attività operativa quotidiana.

* **RAID Hardware vs Software:** Solitamente, in ambito aziendale, il RAID viene configurato a livello hardware tramite controller dedicati prima dell'installazione del sistema operativo.
* **RAID vs LVM:** È importante distinguere queste due tecnologie:
    * **RAID:** Si concentra sulla protezione fisica dei dati e sulla ridondanza dei dischi rigidi.
    * **LVM (Logical Volume Manager):** Si occupa della gestione logica dei volumi, permettendo di ridimensionare e gestire lo spazio in modo flessibile.



---

# Manutenzione del File System: `fsck` e `xfs_repair`

Per garantire l'integrità dei dati in un sistema Linux, è fondamentale conoscere gli strumenti di verifica e riparazione dei file system. Esistono due utility principali, la cui scelta dipende dal tipo di file system in uso.

---

### Utility di Controllo

Esistono due comandi principali per gestire la manutenzione dei file system:

1.  **`fsck` (File System Check):** È l'utility storica, utilizzata sin dall'era Unix e Solaris. È progettata specificamente per i file system della famiglia **EXT** (EXT2, EXT3, EXT4).
2.  **`xfs_repair`:** È lo strumento specifico per i file system di tipo **XFS**, introdotti per gestire partizioni di grandi dimensioni (terabyte e petabyte) che richiedono un'elevata scalabilità.



### Identificazione del File System
Prima di procedere, è necessario identificare il tipo di file system associato a ogni partizione. Il comando consigliato è:
```bash
df -Th
```

L'opzione `-T` visualizza il tipo di file system (ad esempio **xfs**, **ext4** o **nfs**), informazione fondamentale per determinare quale utility di manutenzione debba essere impiegata.



---

### Procedure di Riparazione e Manutenzione

#### Esecuzione al Boot
Per i file system di tipo EXT, il sistema operativo esegue automaticamente `fsck` durante la fase di avvio per verificare la coerenza dei metadati. Qualora venissero rilevate anomalie, l'utility tenterebbe la riparazione prima di completare il caricamento del sistema. Al contrario, **`xfs_repair` non viene eseguito automaticamente al boot**; data la natura dei volumi XFS, spesso molto capienti, un controllo automatico comporterebbe tempi di attesa eccessivi per l'operatività del server.

#### Intervento Manuale dell'Amministratore
In presenza di errori rilevati nei log di sistema (come `/var/log/messages`) o di anomalie nelle operazioni di Input/Output (I/O), si rende necessario un intervento manuale.

> **Avvertenza:** È imperativo non eseguire mai strumenti di riparazione su un file system montato. Il volume deve essere preventivamente smontato per evitare la corruzione irreversibile dei dati.

1.  **Smontaggio del volume:**
    ```bash
    umount /punto_di_montaggio
    ```
2.  **Esecuzione della riparazione:**
    * Per file system EXT: `fsck -y /dev/sdX` (l'opzione `-y` risponde affermativamente in automatico a ogni richiesta di correzione).
    * Per file system XFS: `xfs_repair /dev/sdX`.
3.  **Rimontaggio del volume:**
    ```bash
    mount /dev/sdX /punto_di_montaggio
    ```



#### Gestione della Partizione Root (`/`)
La partizione root non può essere smontata mentre il sistema è in esecuzione. Per sottoporre a manutenzione il file system radice, è necessario riavviare il sistema in **Single User Mode** o utilizzare una **Rescue Mode** (modalità di ripristino) tramite un supporto di installazione esterno, montando il disco originale come unità secondaria.

---

### Analisi dei Codici di Uscita (Exit Codes)
Al termine dell'esecuzione di `fsck`, l'esito dell'operazione può essere verificato interrogando la variabile di ambiente `$?`:
```bash
echo $?
```

I principali codici di ritorno (exit codes) sono i seguenti:

| Codice | Significato |
| :--- | :--- |
| **0** | Nessun errore riscontrato nel file system. |
| **1** | Errori del file system rilevati e corretti con successo. |
| **2** | Il sistema richiede un riavvio obbligatorio (file system corretto, ma richiede reboot). |
| **4** | Errori del file system rilevati ma lasciati non corretti. |
| **8** | Errore operativo durante l'esecuzione del comando. |
| **16** | Errore di utilizzo o di sintassi nel comando. |
| **32** | Operazione annullata su richiesta dell'utente. |
| **128** | Errore relativo a una libreria condivisa. |

L'analisi di tali codici può essere effettuata immediatamente dopo l'esecuzione del comando tramite l'istruzione `echo $?`.

---

### Raccomandazioni di Sicurezza
L'utilizzo di queste utility comporta azioni profonde sulla struttura dei dati. Prima di procedere su sistemi di produzione, è caldamente consigliata l'esecuzione di uno **snapshot** della macchina virtuale o un backup integrale dei volumi interessati.



Qualora un file system risultasse contrassegnato come "clean" (pulito) ma si sospettassero comunque anomalie latenti, è possibile forzare il controllo approfondito utilizzando l'opzione `-f`.

---

#### Esempio di riparazione manuale su XFS
Se l'utility rileva un file system montato, restituirà un errore fatale. La procedura corretta prevede:
1. Identificazione del dispositivo con `df -Th`.
2. Smontaggio del volume con `umount`.
3. Esecuzione di `xfs_repair`.
4. Verifica dell'exit code con `echo $?`.

---

# Strategie di Backup del Sistema

In ambito Linux, esistono diverse metodologie per eseguire il backup dei dati. Questa lezione si focalizza sull'utilizzo dell'utility `dd`, uno strumento storico utilizzato sin dai tempi dei backup su nastri magnetici.

---

### Le 5 Tipologie di Backup

Ogni sistema informativo può essere protetto attraverso cinque livelli di backup differenti:

1.  **Backup dell'intero sistema:** Consiste nella creazione di un'immagine completa del sistema (simile a un file ISO). Si utilizzano spesso software di terze parti come Acronis, Veeam, Commvault o NetBackup per garantire un ripristino totale in caso di crash hardware.
2.  **Backup delle applicazioni:** Si concentra sui software installati sul sistema operativo. Poiché lo scopo del sistema operativo è far girare le applicazioni, queste richiedono strategie di salvataggio dedicate, spesso fornite dai produttori stessi.
3.  **Backup dei database:** I database (Oracle, SQL Server, ecc.) generano dati critici che richiedono strumenti specifici (come Oracle DataGuard) per garantire la coerenza dei dati. Questo backup non riguarda il file system, ma solo le informazioni gestite dall'applicazione.
4.  **Backup del file system:** Riguarda il salvataggio di file e directory. In Linux, si utilizzano comunemente comandi come `tar` (per creare archivi) e `gzip` (per comprimerli). Gli archivi risultanti possono poi essere trasferiti su server remoti.
5.  **Backup del disco (Disk Cloning):** È l'obiettivo principale di questa lezione. Consiste nel clonare bit per bit un intero disco fisico o una partizione.



---

### L'Utility `dd`

Il comando **`dd`** (*dataset definition*) è una utility da riga di comando per sistemi Unix-like il cui scopo primario è convertire e copiare file. A differenza di altri strumenti, `dd` non interpreta il contenuto del file system: copia esattamente ciò che trova, inclusi i settori di avvio (MBR/GPT) e lo spazio vuoto.

#### Sintassi di base
La struttura fondamentale del comando prevede l'indicazione del file di input (`if`) e del file di output (`of`):
```bash
dd if=<sorgente> of=<destinazione>
```

* **Clonazione di un intero disco:**
    ```bash
    dd if=/dev/sda of=/dev/sdb
    ```
    *Attenzione:* È necessario che il disco di destinazione abbia una capacità uguale o superiore a quella del disco sorgente.



* **Creazione di un'immagine di una partizione:**
    L'utility permette di salvare il contenuto di una singola partizione all'interno di un file immagine, utile per scopi di archiviazione o trasferimento.
    ```bash
    dd if=/dev/sda1 of=/root/partition_image.img
    ```

---

### Esempio Pratico: Backup e Ripristino della Partizione `/boot`

Per operare con il comando `dd` è indispensabile disporre dei privilegi di **root**.

#### 1. Esecuzione del Backup
Si ipotizzi di voler creare un'immagine della partizione di avvio `/dev/sda1` (ad esempio il punto di montaggio `/boot`) e di volerla salvare come file all'interno di un altro file system montato su `/data`:
```bash
dd if=/dev/sda1 of=/data/boot.img
```

Durante l'operazione, `dd` non distingue tra spazio libero e spazio occupato: viene effettuata una copia fisica di ogni singolo blocco. Pertanto, se la partizione è da 1 GB, il file risultante sarà esattamente di 1 GB, anche se i dati reali ammontano a pochi megabyte.

#### 2. Verifica del file immagine
Al termine della procedura, è possibile confermare la corretta creazione del file e la sua dimensione effettiva tramite il comando:
```bash
ls -lh /data/boot.img
```

L'output mostrerà un file di dimensioni identiche alla partizione sorgente, a conferma della copia integrale dei blocchi eseguita dall'utility.

#### 3. Ripristino (Restore)
Qualora la partizione originale dovesse subire una corruzione o una perdita di dati, il ripristino può essere effettuato invertendo i parametri di input e output. In questo scenario, l'immagine salvata in precedenza viene utilizzata come sorgente (`if`), mentre la partizione fisica di destinazione viene indicata come output (`of`):

```bash
dd if=/data/boot.img of=/dev/sda1
```

Si raccomanda estrema cautela nell'esecuzione di questa operazione su sistemi in produzione; l'utilizzo di partizioni di test è preferibile per verificare la procedura.

---

### Considerazioni Finali sul Backup
La scelta della metodologia di salvataggio deve essere guidata dalle specifiche esigenze di ripristino e dalla natura dei dati trattati:

* **Backup Completo del Sistema:** Necessario per il *disaster recovery* integrale del server, spesso eseguito tramite immagini ISO o software di terze parti.
* **Backup Applicativo e dei Database:** Indispensabile per garantire la coerenza dei dati dinamici, utilizzando strumenti nativi che gestiscono correttamente le transazioni.
* **Backup del File System:** Ideale per il recupero rapido di singoli file o directory tramite archivi compressi (es. `tar`, `gzip`).
* **Clonazione con `dd`:** Fondamentale per migrazioni hardware, analisi forense o per preservare l'esatta struttura dei blocchi e dei settori di avvio di un disco.

---

#### Avvertenze sull'uso di `dd`
Si ribadisce che il comando `dd` opera a basso livello, ignorando la struttura logica del file system. Tale caratteristica implica che:
1.  Viene copiato anche lo spazio non utilizzato, rendendo il processo potenzialmente più lento rispetto a un backup basato su file.
2.  Non vengono richieste conferme prima della sovrascrittura. Un errore nella specifica del parametro di destinazione (`of`) può causare la perdita immediata e irreversibile di dati critici.

---


# Network File System (NFS)

Il **Network File System (NFS)** è un protocollo fondamentale che permette di condividere dischi, directory e file attraverso una rete. In questa lezione viene analizzato come configurare un sistema **Client-Server** per rendere disponibili risorse remote come se fossero presenti sul disco locale.

---

### Cos'è l'NFS?
Sviluppato originariamente da Sun Microsystems, l'NFS è un protocollo parte dei sistemi **NAS** (Network Attached Storage). Esso permette a più computer di accedere allo stesso spazio di archiviazione contemporaneamente.

* **Esportazione (Exporting):** Il processo con cui il server rende disponibile una directory ai client remoti.
* **Montaggio (Mounting):** Il processo con cui il client mappa la directory condivisa nel proprio file system locale.



---

### Configurazione lato Server
Il server è la macchina che ospita fisicamente i dati e li "offre" alla rete. In questo esempio, si utilizza una macchina denominata `MyFirstLinuxVM`.

#### 1. Installazione dei pacchetti
È necessario installare le utility NFS e il mappatore di ID:
```bash
yum install nfs-utils libnfsidmap
```

#### 2. Abilitazione e avvio dei servizi
I servizi necessari devono essere configurati per l'avvio automatico al boot del sistema e, successivamente, devono essere avviati per la sessione corrente. Su sistemi basati su **systemd** (come CentOS 7 o Red Hat 7/8/9), si procede con i seguenti comandi:

```bash
# Abilitazione all'avvio
systemctl enable rpcbind
systemctl enable nfs-server

# Avvio dei demoni necessari
systemctl start rpcbind
systemctl start nfs-server
systemctl start rpc-statd
systemctl start nfs-idmapd
```

È possibile verificare che i servizi siano correttamente in esecuzione interrogandone lo stato attraverso l'utility `systemctl`:

```bash
systemctl status rpcbind nfs-server
```

L'output deve riportare la dicitura `active (running)` per entrambi i demoni, confermando che il sistema è pronto a gestire le richieste di rete.



#### 3. Creazione della risorsa condivisa e gestione dei permessi
Si procede alla creazione della directory fisica che ospiterà i dati da condividere. Qualora la directory non sia presente, viene generata tramite il comando `mkdir`. Successivamente, è fondamentale assegnare i permessi corretti per permettere l'interazione da parte degli utenti remoti:

```bash
mkdir /mypretzels
chmod a+rwx /mypretzels
```

In questa fase di laboratorio, l'assegnazione dei permessi globali (**777**) assicura che il client NFS possa eseguire operazioni di lettura, scrittura ed esecuzione senza restrizioni locali derivanti dal file system del server.



#### 4. Definizione delle regole di esportazione in `/etc/exports`
Il file `/etc/exports` costituisce il database delle configurazioni del server NFS. In esso vengono specificati i percorsi da condividere, gli host autorizzati e le opzioni di sicurezza. Prima di procedere alla modifica, è caldamente consigliata l'esecuzione di una copia di backup:

```bash
cp /etc/exports /etc/exports.bak
vi /etc/exports
```

La sintassi per la condivisione segue lo schema percorso host(opzioni). Per autorizzare l'accesso a qualsiasi client della rete, viene inserita la seguente riga:

```Plaintext
/mypretzels *(rw,sync,no_root_squash)
```

**Dettaglio delle opzioni di esportazione:**

* **`rw` (read-write):** Consente al client di eseguire sia operazioni di lettura che di scrittura sulla directory condivisa.
* **`sync`:** Obbliga il server a confermare le operazioni di scrittura solo dopo che i dati sono stati effettivamente salvati sul supporto fisico (disco). Questo garantisce una maggiore integrità dei dati, sebbene possa influire leggermente sulle prestazioni.
* **`no_root_squash`:** Per impostazione predefinita, NFS mappa l'utente `root` del client su un utente anonimo del server (`nfsnobody`) per motivi di sicurezza. Utilizzando questa opzione, tale comportamento viene disabilitato, permettendo all'utente root della macchina client di mantenere i privilegi di superutente anche sui file residenti nella condivisione del server.



#### 5. Pubblicazione delle condivisioni
Per rendere effettive le modifiche apportate al file `/etc/exports` senza dover riavviare l'intero stack dei servizi NFS, viene utilizzato il comando `exportfs`. Tale utility aggiorna istantaneamente la tabella delle esportazioni mantenuta dal kernel:

```bash
exportfs -rv
```

* L'opzione **`-r`** (re-export) forza la rilettura del file di configurazione, aggiornando la lista delle directory condivise.
* L'opzione **`-v`** (verbose) visualizza a video i dettagli delle directory esportate e i relativi parametri applicati, confermando l'esito dell'operazione.



---

### Configurazione del Client NFS
Una volta predisposto il server, è necessario configurare la macchina client (denominata nell'esempio `monks`) per accedere alla risorsa remota. La procedura segue i passaggi descritti di seguito:

#### 1. Installazione dei pacchetti necessari
Anche sul lato client è richiesta la presenza delle utility NFS per gestire il protocollo di rete:
```bash
yum install nfs-utils rpcbind
```

#### 2. Gestione dei servizi sul Client
Il demone `rpcbind` deve essere abilitato e avviato per permettere al client di negoziare la connessione con il server. A differenza del server, sul client non è solitamente necessario mantenere attivo il servizio `nfs-server`:

```bash
systemctl enable rpcbind
systemctl start rpcbind
```

È opportuno verificare che non vi siano firewall attivi (come `firewalld` o regole di `iptables`) o configurazioni di sicurezza di rete che possano bloccare il traffico sulle porte utilizzate da NFS tra le due macchine. In ambienti di produzione, anziché disabilitare completamente il firewall, è preferibile autorizzare specificamente i servizi `nfs`, `mountd` e `rpc-bind`.

#### 3. Verifica delle esportazioni remote
Prima di procedere all'operazione di montaggio, è possibile interrogare il server remoto per ottenere l'elenco delle directory che quest'ultimo sta effettivamente rendendo disponibili per la rete:

```bash
showmount -e <IP_DEL_SERVER>
```

#### 4. Creazione del punto di montaggio e connessione
Si procede alla creazione di una directory locale, definita "punto di montaggio", che funge da interfaccia per accedere ai file remoti. Successivamente, viene eseguito il comando `mount` specificando il tipo di file system `nfs`, l'indirizzo IP del server e il percorso della risorsa condivisa:

```bash
mkdir /mnt/kramer
mount -t nfs <IP_DEL_SERVER>:/mypretzels /mnt/kramer
```

#### 5. Verifica e smontaggio (Unmount)
Per confermare che il montaggio sia avvenuto correttamente e per visualizzare lo spazio disco disponibile sulla risorsa remota, viene utilizzato il comando di sistema:

```bash
df -h
```

Nell'output del comando, la risorsa apparirà indicando l'indirizzo IP del server, il percorso della directory esportata e il punto di montaggio locale.

Qualora fosse necessario scollegare la risorsa per terminare la sessione o eseguire operazioni di manutenzione, si utilizza il comando `umount` seguito dal punto di montaggio:
```bash
umount /mnt/kramer
```

### Procedura Pratica di Laboratorio: Configurazione del Client
Durante l'esercitazione viene illustrata la creazione di una seconda macchina virtuale, denominata `monks`, destinata a operare come client. 

* **Installazione del Sistema Operativo:** Viene selezionata l'opzione "Minimal Install" di CentOS 7. Questa scelta permette di ottimizzare le risorse di sistema escludendo l'interfaccia grafica, non necessaria per le operazioni di amministrazione via terminale.
* **Configurazione della Rete:** Risulta fondamentale che il client e il server siano in grado di comunicare sulla stessa sottorete. All'interno dell'ambiente VirtualBox, viene configurata una scheda di rete di tipo "Bridge Adapter" o "Rete Interna".
* **Verifica della Connettività:** Al termine dell'installazione, viene verificato l'indirizzo IP del client attraverso il comando `ip addr`. Qualora venissero rilevate discrepanze tra le sottoreti del server e del client, viene effettuato un intervento sulle impostazioni della scheda di rete della macchina virtuale per garantire il corretto instradamento del traffico NFS.



#### Esecuzione dei comandi sul Client
Una volta garantita la connettività, vengono eseguiti i seguenti passaggi operativi:

1.  **Installazione delle utility:** Si procede all'installazione dei pacchetti necessari per il protocollo NFS.
    ```bash
    yum install nfs-utils rpcbind
    ```
2.  **Avvio dei servizi:** Viene attivato il servizio `rpcbind`, essenziale per la gestione delle chiamate RPC.
    ```bash
    systemctl start rpcbind
    ```
3.  **Montaggio della directory:** Viene creato il punto di montaggio locale e collegata la directory remota del server.
    ```bash
    mkdir /mnt/kramer
    mount -t nfs <IP_DEL_SERVER>:/mypretzels /mnt/kramer
    ```



---

# Introduzione a Samba: Installazione e Configurazione

Samba è un'utility Linux che permette la condivisione di risorse (come file e stampanti) tra sistemi operativi diversi. A differenza di NFS, che opera principalmente in ambienti Unix-like, Samba consente l'interoperabilità con sistemi **Windows** e **macOS**.

---

## 1. Architettura e Protocolli
Samba utilizza principalmente due protocolli per la condivisione dei dati:
* **SMB (Server Message Block):** Sviluppato originariamente da IBM.
* **CIFS (Common Internet File System):** Una variante di SMB introdotta da Microsoft.
* **NMB (NetBIOS Name Server):** Utilizzato per la risoluzione dei nomi in ambiente Windows.

Nell'uso comune, i termini SMB e CIFS vengono spesso utilizzati come sinonimi.



---

## 2. Preparazione del Server
Prima di procedere con la configurazione, è consigliabile eseguire uno **snapshot** della macchina virtuale per poter ripristinare lo stato precedente in caso di errori critici.

### Installazione dei pacchetti
Per configurare il server, devono essere installati i seguenti pacchetti tramite il gestore `yum`:
```bash
yum install samba samba-client samba-common
```

#### Configurazione del Firewall e SELinux
Per garantire il corretto transito dei dati tra server e client, è necessario gestire i servizi di sicurezza che potrebbero bloccare le connessioni Samba.

1.  **Firewall:** In ambienti di produzione, devono essere aperte le porte relative ai servizi Samba. In contesti di test o laboratorio, è possibile procedere alla disattivazione temporanea del servizio `firewalld`:
    ```bash
    systemctl stop firewalld
    systemctl disable firewalld
    ```

2.  **SELinux:** Il sistema di sicurezza SELinux può impedire a Samba di accedere alle directory del file system. Per evitare blocchi durante le fasi di configurazione iniziale, è possibile impostare SELinux in modalità `disabled` o `permissive`. La modifica permanente viene effettuata nel file di configurazione:
    ```bash
    vi /etc/selinux/config
    # Impostare la riga come segue:
    SELINUX=disabled
    ```
    *Nota: La disattivazione di SELinux richiede il riavvio del sistema per essere effettiva.*



---

#### 3. Configurazione della Risorsa Condivisa

**Creazione della directory e gestione dei permessi**
Viene definita la directory fisica che ospiterà i file condivisi. Per consentire l'accesso a utenti non autenticati (guest), la proprietà della cartella deve essere assegnata all'utente di sistema `nobody`:

```bash
mkdir -p /samba/more_pretzels
chmod -R 777 /samba/more_pretzels
chown -R nobody:nobody /samba/more_pretzels
```

#### Modifica del file smb.conf
Il file `/etc/samba/smb.conf` rappresenta il cuore della configurazione di Samba. Al suo interno vengono definite le impostazioni globali del server e le specifiche delle singole directory condivise. Prima di procedere con qualsiasi modifica, è considerata una procedura standard la creazione di una copia di backup del file originale per consentire un eventuale ripristino:

```bash
cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
vi /etc/samba/smb.conf
```

All'interno del file, viene rimossa la configurazione predefinita non necessaria e inserita una nuova definizione per la risorsa condivisa (ad esempio, denominata `[anonymous]`):
```plaintext
[global]
    workgroup = WORKGROUP
    netbios name = CentOS
    security = user
    map to guest = bad user
    dns proxy = no

[anonymous]
    path = /samba/more_pretzels
    browsable = yes
    writable = yes
    guest ok = yes
    read only = no
```

#### 4. Verifica e Avvio dei Servizi
Per convalidare la correttezza della sintassi inserita nel file di configurazione ed escludere errori formali che potrebbero impedire l'esecuzione dei demoni, viene utilizzato lo strumento di diagnostica integrato:

```bash
testparm
```

Una volta confermata la validità dei parametri, si procede all'abilitazione dei servizi per l'avvio automatico al boot del sistema e al loro contestuale avvio per la sessione corrente. I servizi coinvolti sono `smb` (gestione della condivisione file e stampa) e `nmb` (risoluzione dei nomi NetBIOS):
```bash
# Abilitazione all'avvio
systemctl enable smb nmb

# Avvio immediato dei servizi
systemctl start smb nmb
```

Lo stato operativo dei servizi può essere monitorato per garantire che entrambi risultino in stato `active (running)`:
```bash
systemctl status smb nmb
```

#### 5. Accesso alla Risorsa dai Client

**Accesso da sistemi Windows**
Per accedere alla condivisione da una postazione Windows, viene utilizzato l'indirizzo IP del server all'interno di Esplora File o nella finestra "Esegui" (tasto `Win + R`), utilizzando la convenzione UNC:

```text
\\<IP_DEL_SERVER>
```

Se la configurazione è stata impostata come anonima (guest), la cartella condivisa risulta accessibile senza la richiesta di credenziali. È possibile testare l'interoperabilità creando un file (ad esempio, un documento di testo) dalla macchina Windows e verificandone la presenza sul server Linux all'interno della directory `/samba/more_pretzels`.



**Accesso da sistemi Linux (Client)**
Per consentire a un altro sistema Linux di montare la condivisione Samba, è necessaria l'installazione dei pacchetti `cifs-utils` e `samba-client`. La procedura prevede la creazione di un punto di montaggio locale e l'esecuzione del comando `mount` specificando il protocollo `cifs`:

```bash
# Installazione delle utility sul client
yum install cifs-utils samba-client

# Creazione del punto di montaggio locale
mkdir /mnt/sambashare

# Esecuzione del montaggio della risorsa remota
mount -t cifs //<IP_DEL_SERVER>/anonymous /mnt/sambashare
```

Qualora venga richiesta una password durante l'esecuzione del comando, è sufficiente premere Invio, dato che l'accesso è configurato in modalità guest.

#### 6. Verifica e Smontaggio della Risorsa
L'avvenuto montaggio della risorsa remota può essere confermato attraverso il comando che visualizza lo stato del file system e l'occupazione del disco:

```bash
df -h
```

Nell'output del comando, la risorsa deve apparire associata al punto di montaggio locale (es. `/mnt/sambashare`), indicando l'indirizzo IP del server e il nome della condivisione. Questo conferma che il file system remoto è integrato correttamente nell'albero delle directory locale.



Per terminare la sessione di lavoro e scollegare in modo sicuro la directory remota dal sistema, viene utilizzato il comando `umount` seguito dal percorso del punto di montaggio:

```bash
umount /mnt/sambashare
```

È fondamentale assicurarsi che nessun processo o terminale stia utilizzando file all'interno della directory durante lo smontaggio, al fine di evitare errori di tipo "target is busy". In caso di errore, è possibile identificare i processi attivi tramite il comando `lsof` o `fuser`.



---

### Procedure Avanzate: Condivisioni Protette
Oltre alla configurazione ad accesso libero (guest), è possibile implementare condivisioni sicure che richiedano l'autenticazione obbligatoria. Tale processo prevede i seguenti passaggi:

1.  **Creazione dell'utente di sistema:** Viene generato un utente Linux dedicato (senza necessariamente una shell di login attiva) se non già presente nel sistema.
2.  **Registrazione in Samba:** L'utente viene inserito nel database delle password di Samba tramite l'utility `smbpasswd`, che permette di definire credenziali specifiche per l'accesso alla rete:
    ```bash
    smbpasswd -a nome_utente
    ```
3.  **Configurazione in `smb.conf`:** Si procede alla modifica della sezione della condivisione nel file `/etc/samba/smb.conf` per limitare l'accesso ai soli utenti autorizzati, disabilitando l'accesso guest:
    ```text
    [secured_share]
        path = /samba/protected_data
        valid users = nome_utente
        guest ok = no
        writable = yes
    ```



In questo modo, l'accesso alla risorsa da parte dei client (Windows o Linux) sarà vincolato all'inserimento di credenziali valide, garantendo una maggiore protezione dei dati sensibili all'interno dell'ambiente di rete.

---

# Configurazione di Dispositivi NAS per NFS e Samba

Oltre alla possibilità di configurare uno storage condiviso direttamente su un server Linux, è possibile utilizzare dispositivi **NAS (Network Attached Storage)** dedicati. Questi dispositivi permettono di condividere file system con client Linux (tramite NFS) e client Windows (tramite Samba/SMB) in modo centralizzato e semplificato.

---

## 1. Architettura Fisica e Logica
Un dispositivo NAS si presenta solitamente come un case esterno (box) o un'unità montabile a rack, contenente più alloggiamenti per dischi rigidi.

* **Hardware:** Dispone di slot per hard disk facilmente sostituibili (hot-swappable) e porte di rete (RJ45 CAT5/6) per la connettività LAN.
* **Software:** Il dispositivo è gestito da un sistema operativo dedicato (firmware) accessibile tramite interfaccia web, che permette di amministrare i volumi, gli utenti e i protocolli di condivisione.



---

## 2. Inizializzazione dello Storage
Prima di condividere i dati, è necessario preparare lo storage fisico attraverso i seguenti passaggi:

1.  **Creazione del Pool di Archiviazione:** Si selezionano i dischi disponibili e si definisce il tipo di **RAID** (es. RAID 5 per bilanciare prestazioni e ridondanza).
2.  **Creazione del Volume:** Si definisce la dimensione del volume logico e il file system desiderato (es. **Btrfs** per funzionalità avanzate o **EXT4** per massima compatibilità).



---

## 3. Configurazione della Cartella Condivisa
Una volta creato il volume, si procede alla creazione della cartella condivisa (es. denominata `peanuts`):

* **Permessi Utente:** Si assegnano i diritti di lettura/scrittura agli utenti autorizzati.
* **Abilitazione Protocolli:** Si attivano i servizi NFS e SMB nelle impostazioni del dispositivo NAS.
* **Permessi NFS:** È necessario specificare quali host possono accedere alla risorsa. Utilizzando il carattere jolly `*`, l'accesso viene consentito a tutti i client della rete.

---

## 4. Montaggio della Risorsa sui Client

### Su Client Linux (NFS)
Per collegare la cartella condivisa dal NAS a un sistema Linux, si crea un punto di montaggio locale e si utilizza il comando `mount` specificando l'IP del NAS e il percorso remoto:

```bash
# Creazione del punto di montaggio
mkdir /a

# Montaggio della risorsa NFS
mount 192.168.1.56:/volume1/peanuts /a
```

La verifica del corretto montaggio può essere effettuata con il comando: `df -h`

#### Accesso da Client Windows (SMB/Samba)
Su sistemi Windows, l'accesso alla risorsa condivisa avviene tramite il protocollo SMB. La procedura non richiede l'installazione di software aggiuntivo, poiché il supporto per le reti Windows è integrato nel sistema operativo.

1.  **Inserimento del percorso:** Viene aperta una finestra di Esplora File o la casella "Esegui" (scorciatoia `Win + R`).
2.  **Sintassi UNC:** Viene digitato l'indirizzo IP del dispositivo NAS preceduto dal doppio backslash (es. `\\192.168.1.56`).



3.  **Autenticazione:** Se richiesto, vengono inserite le credenziali (username e password) create durante la fase di configurazione iniziale del NAS.
4.  **Visualizzazione:** La cartella condivisa (nell'esempio, `peanuts`) apparirà nell'elenco delle risorse disponibili, pronta per essere utilizzata come un'unità disco locale.

---

#### 5. Verifica della Sincronizzazione Multi-Piattaforma
L'utilizzo di un dispositivo NAS centralizzato garantisce che i file siano accessibili e modificabili simultaneamente da diversi sistemi operativi. 

* **Test di scrittura:** Creando un file da un terminale Linux all'interno del punto di montaggio:
    ```bash
    cd /a
    touch file_di_test_linux.txt
    ```
* **Riscontro su Windows:** Il file apparirà istantaneamente all'interno della finestra di Esplora File su Windows. 



Questa interoperabilità conferma il corretto funzionamento dei servizi NFS e Samba attivi sul NAS, permettendo una collaborazione fluida in ambienti di rete eterogenei.

---

### Conclusioni sulla gestione NAS
La configurazione tramite dispositivo dedicato risulta notevolmente più rapida rispetto alla gestione manuale su server Linux, grazie a interfacce web intuitive che riducono le operazioni da riga di comando a pochi passaggi essenziali di montaggio.

---

# Confronto tra Tecnologie di Storage: SATA vs SAS

I termini **SATA** e **SAS** identificano i protocolli di comunicazione e le tipologie di dischi utilizzati all'interno di un computer per il trasferimento dei dati tra la scheda madre (motherboard) e i dispositivi di archiviazione.

---

## 1. Definizioni e Acronimi

* **SATA (Serial Advanced Technology Attachment):** Evoluzione seriale della vecchia tecnologia ATA (o IDE).
* **SAS (Serial Attached SCSI):** Evoluzione seriale dell'interfaccia SCSI (*Small Computer System Interface*), storicamente nota per le alte prestazioni in ambito professionale.

---

## 2. Evoluzione della Comunicazione: Da Parallela a Seriale

Per comprendere il funzionamento attuale, è necessario analizzare il passaggio tecnologico avvenuto negli anni:

### Comunicazione Parallela (Tecnologia Datata)
In passato, il trasferimento dati utilizzava cavi larghi e piatti. La comunicazione richiedeva due percorsi fisici distinti e ingombranti: un cavo dedicato al traffico in entrata e uno separato per il traffico in uscita. Questo limitava la miniaturizzazione dei dispositivi (come i laptop) a causa dell'ingombro dei cavi.



### Comunicazione Seriale (Tecnologia Attuale)
Sia SATA che SAS utilizzano la comunicazione seriale. In questo modello, un singolo cavo gestisce entrambi i flussi di dati (inbound e outbound) attraverso "corsie" logiche separate all'interno della stessa guaina. Ciò ha permesso di creare computer più compatti e migliorare il flusso d'aria interno.

---

## 3. Differenze Principali tra SATA e SAS

Sebbene entrambi siano protocolli seriali, presentano differenze sostanziali in termini di prestazioni e destinazione d'uso:

| Caratteristica | SATA | SAS |
| :--- | :--- | :--- |
| **Affidabilità** | Standard (uso consumer) | Elevata (uso enterprise) |
| **Velocità** | Moderata | Alta |
| **Costo** | Economico | Più elevato |
| **Ambiente ideale** | Desktop, archiviazione file | Server, workstation, database |

### Gestione dei Cablaggi e Connettività
* **SATA:** Progettato principalmente per un collegamento punto-punto tra scheda madre e disco. Ogni porta SATA gestisce un singolo dispositivo.
* **SAS:** Permette una maggiore scalabilità. Grazie alla gestione separata dei segnali all'interno del protocollo, è possibile collegare la scheda madre a un disco e contemporaneamente a un altro hardware compatibile SAS, permettendo la creazione di catene di dispositivi più complesse.



---

## 4. Analogia Stradale
Per visualizzare la differenza di connettività, si può utilizzare una metafora geografica:
* Il sistema **SATA** agisce come un'autostrada diretta che collega solo due punti (es. Città A -> Città B).
* Il sistema **SAS** agisce come uno snodo autostradale che consente l'accesso a più destinazioni o città diverse (es. Città A -> Città B e contemporaneamente Città C), grazie alla sua capacità di gestire connessioni multiple e ridondanti.

---

# Guida all'Amministrazione di Database in Ambiente Linux: MySQL e MariaDB

La gestione dei database rappresenta una competenza fondamentale per un amministratore di sistema Linux, spaziando dall'installazione e configurazione iniziale fino alla risoluzione dei problemi (troubleshooting).

---

## 1. Relazione tra MySQL e MariaDB

MySQL e MariaDB sono sistemi di gestione di database relazionali (RDBMS) estremamente simili, condividendo le stesse fondamenta tecnologiche.

* **MySQL:** Attualmente di proprietà di **Oracle Corporation**.
* **MariaDB:** Creato dagli sviluppatori originali di MySQL come un **fork** open-source, per garantire che il software rimanesse libero e guidato dalla comunità.
* **Compatibilità:** Le applicazioni progettate per MySQL funzionano generalmente con MariaDB senza necessità di modifiche, poiché utilizzano gli stessi protocolli, comandi e utilità.



---

## 2. Architettura di MariaDB

MariaDB è un sistema multithreaded rilasciato sotto licenza **GPL (GNU Public License)**. I dati vengono organizzati in tabelle composte da righe e colonne, una struttura che facilita la gestione e il recupero di grandi volumi di informazioni.

Il sistema supporta il linguaggio **SQL (Structured Query Language)** per le operazioni fondamentali e offre funzionalità avanzate come:
* Stored Procedures e Trigger.
* Supporto per dati JSON e query in stile NoSQL.
* Integrazione con Big Data.

---

## 3. Workflow Operativo (Step-by-Step)

### Fase 1: Installazione e Gestione del Servizio
L'installazione e l'avvio del servizio su distribuzioni basate su RPM (come RHEL o CentOS) avvengono tramite i seguenti comandi:

```bash
# Installazione dei pacchetti server e client
dnf install mariadb-server mariadb -y

# Abilitazione e avvio del servizio
systemctl enable mariadb
systemctl start mariadb

# Verifica dello stato
systemctl status mariadb
```

#### Fase 2: Messa in Sicurezza del Database
Dopo l'installazione iniziale, il database presenta impostazioni predefinite che potrebbero costituire un rischio per la sicurezza. Viene quindi utilizzata l'utility interattiva `mysql_secure_installation` per blindare l'ambiente. Nonostante il nome richiami MySQL, il comando è pienamente compatibile e standard anche per MariaDB.



La procedura guidata permette di eseguire le seguenti operazioni critiche:

1.  **Definizione della Password di Root:** Viene impostata una password robusta per l'utente amministratore del database (da non confondere con l'utente root del sistema operativo).
2.  **Rimozione degli Utenti Anonimi:** Vengono eliminati gli account che consentono l'accesso al database senza un'identità definita.
3.  **Disabilitazione del Login Remoto per Root:** Viene impedito l'accesso dell'utente root da macchine esterne, limitandolo esclusivamente alla connessione locale (localhost).
4.  **Rimozione del Database di Test:** Viene cancellato il database "test" creato di default, insieme ai permessi che ne consentono l'accesso a chiunque.
5.  **Ricaricamento dei Privilegi:** Vengono applicate immediatamente tutte le modifiche apportate senza necessità di riavviare l'intero servizio.



---

#### Fase 3: Operazioni CRUD (Create, Read, Update, Delete)
Una volta completata la messa in sicurezza, è possibile accedere alla shell di comando per la gestione dei dati. Per effettuare l'accesso come amministratore, viene utilizzato il seguente comando:

```bash
mysql -u root -p
```

Dopo l'inserimento della password, l'ambiente permette di eseguire i comandi SQL fondamentali per la gestione del ciclo di vita dei dati.

---


