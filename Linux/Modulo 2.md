# Metodologie di Progettazione del Laboratorio di Pratica

Per l'apprendimento pratico e l'esecuzione delle esercitazioni tecniche, è necessaria la configurazione di un ambiente di laboratorio dedicato. Esistono due approcci principali per l'allestimento di tale ambiente.

---

## 1. Virtualizzazione Locale
Questa metodologia sfrutta le risorse hardware di un computer fisico (Host) con sistema operativo Windows o macOS per eseguire ambienti isolati.



* **Principio di Funzionamento**: L'installazione di un software di virtualizzazione (Hypervisor) consente l'esecuzione di più sistemi operativi ospiti contemporaneamente al sistema principale, senza interferire con quest'ultimo.
* **Strumenti di Riferimento (Open Source/Free)**:
    * **VMware Player**
    * **Oracle VirtualBox**
* **Flusso Operativo**: Prevede il download del software e la successiva creazione di una **Virtual Machine (VM)**. All'interno di questo spazio virtuale isolato viene poi installato il sistema operativo target (tipicamente **Linux**).

---

## 2. Infrastruttura Cloud
Questa alternativa evita l'installazione di software locali pesanti, delegando la gestione dell'hardware a risorse remote accessibili via internet.



* **Principio di Funzionamento**: L'ambiente di laboratorio risiede nell'infrastruttura di un provider esterno, dove vengono allocate risorse computazionali on-demand.
* **Piattaforme Leader del Settore**:
    * **AWS (Amazon Web Services)**
    * **Google Cloud Platform**
    * Sistemi cloud analoghi che offrono piani "Free Tier" per l'erogazione di istanze virtuali gratuite.
* **Flusso Operativo**: Si procede alla configurazione di un'istanza (macchina virtuale) direttamente sul portale del provider e alla successiva installazione o distribuzione del sistema operativo Linux.

---

## Considerazioni Tecniche e Scelta dell'Ambiente
Entrambi i modelli sono validi per il raggiungimento degli obiettivi didattici. Tuttavia, la **virtualizzazione locale (Opzione 1)** è considerata la scelta preferenziale per chi necessita di una corrispondenza esatta con le procedure standard di laboratorio e una gestione diretta della rete virtuale, nonostante la disponibilità di documentazione specifica per entrambi i modelli.

# Analisi di Oracle VirtualBox e della Virtualizzazione x86

L'installazione di sistemi operativi alternativi, come CentOS Linux, su un hardware preesistente richiede una strategia che preservi l'integrità dei dati e la continuità operativa del sistema principale. Esistono due metodologie d'installazione: la sostituzione integrale del sistema operativo corrente o l'impiego di una soluzione di virtualizzazione.

---

## Definizione di Oracle VirtualBox
Oracle VirtualBox è un **hypervisor** gratuito e open-source progettato per architetture x86. Attualmente sviluppato da Oracle Corporation, il software è compatibile con sistemi basati su processori Intel o AMD e può essere eseguito su diversi sistemi operativi ospitanti (Host), tra cui:
* Windows
* macOS
* Linux
* Solaris

---

## Architettura e Funzionamento
Il software agisce come uno strato intermedio che estende le capacità dell'hardware fisico, permettendo l'esecuzione simultanea di più sistemi operativi sullo stesso computer. L'architettura logica si sviluppa secondo la seguente gerarchia:

1.  **Livello Hardware**: La base fisica del computer (CPU, RAM, Disco).
2.  **Sistema Operativo Host**: Il sistema operativo principale già presente (es. Windows o Mac).
3.  **Software di Virtualizzazione**: L'installazione di VirtualBox come applicazione standard.
4.  **Sistemi Operativi Guest**: Istanze separate e isolate di Linux, Windows o macOS che operano all'interno di VirtualBox.

---

## Vantaggi della Virtualizzazione
L'adozione di VirtualBox offre benefici strutturali significativi per l'allestimento di ambienti di test e apprendimento:

* **Conservazione dei Dati**: Consente l'installazione di CentOS Linux senza la necessità di formattare il disco o rimuovere il sistema operativo preesistente.
* **Esecuzione Simultanea**: Permette di operare contemporaneamente su più sistemi operativi senza dover riavviare la macchina fisica.
* **Isolamento**: Ogni sistema guest opera come una versione indipendente e contenuta, minimizzando i rischi per il sistema host.

# Procedura di Installazione di Oracle VirtualBox

L'implementazione dell'ambiente di virtualizzazione locale richiede il completamento di una procedura strutturata, che spazia dal reperimento del software alla sua configurazione iniziale.

---

## 1. Acquisizione del Software
La fase di download deve essere effettuata esclusivamente tramite i canali ufficiali per garantire l'integrità del pacchetto:

* **Sito di riferimento**: [www.virtualbox.org](http://www.virtualbox.org).
* **Selezione della versione**: È necessario identificare la sezione "Downloads" e selezionare la versione più recente disponibile.
* **Scelta del pacchetto (Platform Packages)**: Il file deve essere scelto in base al sistema operativo host in uso. Per sistemi basati su Windows, si seleziona la voce **"Windows hosts"**.

---

## 2. Processo di Installazione
Una volta eseguito il file eseguibile (`.exe`), ha inizio la procedura guidata (Setup Wizard).

* **Configurazione delle funzionalità**: Si consiglia di mantenere le impostazioni predefinite (default) per garantire la piena compatibilità con le configurazioni di laboratorio standard.
* **Gestione delle Dipendenze**: Durante l'installazione, il software potrebbe richiedere l'installazione di componenti aggiuntivi (es. Python Core o pacchetti Microsoft). Tali dipendenze sono necessarie per il corretto funzionamento dell'applicativo.

### Avvisi di Rete
L'installazione delle interfacce di rete virtuali comporta un **reset temporaneo della connettività internet** dell'host. È pertanto necessario assicurarsi che non siano in corso operazioni di rete critiche o download non interrompibili prima di procedere.

---

## 3. Interfaccia e Post-Installazione
Al termine della procedura, l'applicativo si presenta come un portale di gestione centralizzato (VirtualBox Manager).

* **Funzionalità del Portale**: L'interfaccia consente la creazione di nuove macchine virtuali (VM), l'importazione/esportazione di appliance esistenti e la gestione delle risorse hardware allocate.
* **Stato Operativo**: Il corretto avvio del programma conferma la riuscita dell'integrazione tra lo strato software di virtualizzazione e il sistema operativo host.

---

## Considerazioni Finali
Il completamento dell'installazione di VirtualBox è il prerequisito fondamentale per la successiva creazione di istanze virtuali. L'obiettivo primario dell'infrastruttura così creata è l'ospitazione di una distribuzione **Linux**, che fungerà da ambiente di esecuzione per l'apprendimento dei comandi e delle logiche di sistema.

# Implementazione di VMware Workstation Player

VMware Workstation Player rappresenta un'alternativa software a Oracle VirtualBox per la gestione di ambienti virtualizzati. A seguito dell'acquisizione di VMware da parte di Broadcom (novembre 2023), le procedure di accesso al software hanno subito variazioni strutturali.

---

## 1. Reperimento del Software e Politiche di Licenza
L'acquisizione del pacchetto d'installazione avviene tramite il portale ufficiale Broadcom. La procedura richiede il superamento di diversi passaggi amministrativi:

* **Accesso al Portale**: È necessaria la registrazione e l'autenticazione presso il *Customer Support Portal* di Broadcom.
* **Restrizioni della Versione 17**: La versione più recente (v17.x) richiede un'autorizzazione specifica (*entitlement*) o una licenza commerciale, limitandone il download immediato.
* **Soluzione per Uso Didattico**: Per l'accesso gratuito è possibile optare per versioni precedenti (come la **v16.2.5**), le quali non presentano le medesime restrizioni di licenza e risultano idonee per finalità non commerciali.

---

## 2. Configurazione e Installazione
Il processo di installazione viene gestito tramite un Setup Wizard e prevede alcune scelte configurative rilevanti:

* **Enhanced Keyboard Driver**: L'installazione di questo driver ottimizza la gestione dell'input della tastiera, ma richiede un riavvio obbligatorio del sistema host per essere attivata.
* **Integrazione di Sistema**: Durante il setup, è possibile aggiungere gli strumenti di console VMware al PATH di sistema e creare collegamenti rapidi sul desktop per facilitare l'accesso operativo.
* **Dipendenze e Aggiornamenti**: Il wizard gestisce automaticamente la verifica dei requisiti, consentendo di mantenere le impostazioni predefinite per la maggior parte degli scenari di laboratorio.

---

## 3. Primo Avvio e Vincoli d'Uso
Al primo avvio dell'applicativo, viene richiesta la selezione del modello di licenza.

* **Uso Non-Commerciale**: Per finalità educative e di laboratorio personale, è necessario selezionare l'opzione **"Free for non-commercial use"**.
* **Interfaccia Operativa**: Una volta completata la configurazione, l'interfaccia principale consente la creazione di nuove macchine virtuali e l'installazione di sistemi operativi guest (es. Linux).

---

## Sintesi Comparativa
Sebbene VMware Workstation Player offra prestazioni elevate, la complessità del processo di download dovuta alla transizione Broadcom rende questa opzione secondaria rispetto a soluzioni più accessibili come VirtualBox, pur rimanendo tecnicamente valida per il completamento del corso.

# Evoluzione di CentOS: Da Progetto Downstream a Upstream

La comprensione della differenza tra CentOS tradizionale e CentOS Stream richiede un'analisi del flusso di sviluppo del codice all'interno dell'ecosistema Red Hat.

---

## Cenni Storici e Origini
Il progetto CentOS (**Community Enterprise Operating System**) nasce nel 2004 su iniziativa di Greg Kurtzer. L'obiettivo originario era creare una distribuzione basata interamente sul codice sorgente di **Red Hat Enterprise Linux (RHEL)**, essendo quest'ultimo un progetto open-source.

* **Rbranding**: Il codice di Red Hat veniva preso, privato dei marchi registrati e ridistribuito gratuitamente.
* **Acquisizione**: Nel 2014, Red Hat ha acquisito la comunità CentOS, rendendo i due sistemi operativi sostanzialmente identici nel codice (corrispondenza 1:1).

---

## Mutamento del Flusso di Sviluppo (Pipeline)
A partire da febbraio 2021, la gerarchia di distribuzione degli aggiornamenti ha subito una variazione fondamentale nel posizionamento di CentOS all'interno della catena di sviluppo.

### Modello Pre-Febbraio 2021 (Modello Downstream)
In precedenza, CentOS era considerato un progetto **downstream**, ovvero riceveva il codice solo dopo che era stato consolidato in RHEL.
1.  **Fedora**: Progetto bleeding-edge (innovazione rapida).
2.  **RHEL**: Sistema stabile per le aziende.
3.  **CentOS**: Copia speculare e gratuita di RHEL (Downstream).

### Modello Post-Febbraio 2021 (Modello Upstream / CentOS Stream)
Con l'introduzione di CentOS Stream, il sistema è diventato un progetto **upstream** rispetto a RHEL. In questa configurazione, CentOS funge da piattaforma di sviluppo per la successiva versione minore di Red Hat.
1.  **Fedora**: Sviluppo primario.
2.  **CentOS Stream**: Anteprima dei futuri aggiornamenti di RHEL (Upstream).
3.  **RHEL**: Versione finale consolidata.



---

## Valutazione Didattica e Professionale
Nonostante il cambio di architettura nella distribuzione degli aggiornamenti, lo studio di CentOS (in particolare della versione **CentOS Stream 9**) rimane un pilastro fondamentale per la formazione Linux.

* **Equivalenza Tecnica**: CentOS Stream mantiene una struttura pressoché identica a Red Hat Enterprise Linux.
* **Rilevanza**: Le competenze acquisite su CentOS sono direttamente trasferibili in ambito aziendale su sistemi RHEL.
* **Disponibilità**: CentOS rimane una delle opzioni gratuite più solide e diffuse per l'apprendimento delle distribuzioni di classe Enterprise.

---

## Sintesi
Il passaggio a CentOS Stream non indica la fine del sistema operativo, ma una sua trasformazione in un ambiente di "rolling preview" per Red Hat. Per un utente in fase di formazione, CentOS Stream 9 rappresenta attualmente lo standard di riferimento per lo studio del kernel Linux in ambito enterprise.

# Diversi modi di installare Linux

* Inserire un CD in cui è presente l'installazione di Linux
* Tramite immagine ISO scaricabile da Internet
* Tramite chiavetta USB con immagine ISO
* Tramite un'applicazione di installazione (esempio: Redhat Kickstart)

In questo caso cercheremo di legare l'immagine ISO a Oracle. 

# Configurazione e Creazione della Macchina Virtuale (VM)

Una volta installato il software di virtualizzazione (Oracle VirtualBox o VMware Player), è necessario procedere alla creazione logica dell'hardware virtuale. Questa fase simula l'assemblaggio di un computer fisico, definendo le risorse che verranno allocate al sistema operativo guest.

---

## 1. Configurazione su Oracle VirtualBox

La creazione di una nuova VM segue un processo guidato che permette di definire i parametri hardware fondamentali.

### Definizione Nome e Tipologia
* **Nome della VM**: Identificativo univoco all'interno del gestore (es. `myfirstlinuxvm`). Questo nome è puramente organizzativo e non coincide con l'hostname che verrà assegnato al sistema operativo Linux in fase di installazione.
* **Riconoscimento Automatico**: Il software è in grado di identificare automaticamente il tipo di sistema operativo (Linux) e la versione (es. 64-bit) in base al nome digitato.
* **ISO Image**: In questa fase iniziale, il file ISO (l'immagine del disco di installazione) può essere omesso, posticipando l'associazione al momento dell'avvio.

### Allocazione Risorse Hardware
L'assegnazione delle risorse dipende dalle disponibilità del computer host:
* **Base Memory (RAM)**: Si consiglia di elevare il valore predefinito da 1 GB (1024 MB) a **2 GB (2048 MB)** per garantire fluidità operativa.
* **Processore**: Il valore predefinito è solitamente impostato su 1 CPU, modificabile anche in seguito.
* **Hard Disk**: Per un ambiente di laboratorio standard, un'allocazione di **20 GB** è considerata ottimale e sufficiente.

---

## 2. Configurazione su VMware Workstation Player

Il flusso operativo su VMware presenta analogie strutturali con VirtualBox, con alcune variazioni nell'interfaccia.

* **Metodo di installazione**: Si seleziona l'opzione "I will install the operating system later" per configurare prima l'hardware e solo successivamente il software.
* **Selezione della Distribuzione**: Qualora una versione specifica (come CentOS 9 Stream) non sia presente nell'elenco predefinito, è possibile selezionare una voce generica come **"Other Linux 5.x kernel 64-bit"**.
* **Gestione Disco**: Viene impostata una dimensione minima di **20 GB**, selezionando l'opzione per archiviare il disco virtuale come un **singolo file** per ottimizzare le prestazioni.

---

## 3. Configurazione della Rete (Parametro Critico)

Indipendentemente dal software scelto, la configurazione della scheda di rete è fondamentale per garantire la connettività della VM.

* **Modalità Bridged (Bridged Adapter)**: È necessario cambiare l'impostazione predefinita (spesso NAT) in **Bridged**. 
* **Vantaggio**: Questa modalità permette alla macchina virtuale di connettersi direttamente alla rete fisica dell'host (Wi-Fi o Ethernet), ottenendo un proprio indirizzo IP e facilitando l'accesso a Internet per il download di pacchetti e aggiornamenti durante e dopo l'installazione.

---

## Sintesi Operativa
Al termine di questi passaggi, la macchina virtuale risulta creata ma "spenta" (Powered Off). Il sistema è pronto per la fase successiva: l'associazione dell'immagine ISO del sistema operativo Linux e l'avvio della procedura di installazione vera e propria.

# Installazione del Sistema Operativo Linux (CentOS 7)

L'allestimento del laboratorio prosegue con l'acquisizione dell'immagine del sistema operativo e la sua installazione all'interno dell'ambiente virtuale precedentemente configurato.

---

## 1. Selezione della Distribuzione
Sebbene esistano numerose distribuzioni Linux (Ubuntu, Kali, ecc.), la scelta di **CentOS** è motivata dalla sua stretta parentela con **RHEL (Red Hat Enterprise Linux)**, standard predominante nel settore corporate (circa l'80% del mercato). 
* **Compatibilità**: I comandi appresi su CentOS sono applicabili al 90% sulle altre distribuzioni.
* **Versioni**: CentOS 7 rappresenta una base stabile per l'apprendimento, sebbene le versioni 8 e 9 Stream introducano funzionalità avanzate come il supporto nativo ai container.

---

## 2. Acquisizione dell'Immagine ISO
Il download deve essere effettuato tramite i mirror ufficiali del progetto CentOS:
1. **Sorgente**: Sito ufficiale `centos.org`.
2. **Architettura**: Selezionare l'architettura **x86_64**.
3. **Pacchetto**: Si raccomanda la versione **"Everything ISO"** (dimensioni indicative: 9.5 GB) per disporre di tutti i pacchetti necessari senza dipendere dalla rete durante l'installazione.

---

## 3. Associazione ISO alla Macchina Virtuale
Prima di avviare la VM, è necessario "montare" virtualmente il file ISO.

### In Oracle VirtualBox:
* Selezionare la VM -> Cliccare su **Start**.
* Alla richiesta del disco di avvio, selezionare l'icona della cartella, aggiungere il file `.iso` scaricato e confermare con **Choose** -> **Start**.

### In VMware Player:
* Tasto destro sulla VM -> **Settings** -> **CD/DVD**.
* Selezionare **"Use ISO image file"**, navigare fino al file scaricato e confermare con **OK**.

---

## 4. Configurazione del Processo di Installazione
All'avvio della VM, selezionare tramite tastiera l'opzione **"Install CentOS 7"**. La procedura guidata (GUI) richiede i seguenti passaggi:

* **Localizzazione**: Selezione della lingua (es. English - United States) e del fuso orario.
* **Software Selection**: Modificare l'impostazione predefinita da "Minimal Install" a **"Server with GUI"** per disporre di un'interfaccia grafica desktop.
* **Installation Destination**: Selezionare il disco virtuale creato (es. 20 GB) e confermare il partizionamento automatico.
* **Network & Hostname**: 
    1. Definire l'Hostname (es. `my-linux-vm`).
    2. Attivare l'interfaccia di rete e configurare la connessione automatica in **Configure -> General -> Automatically connect to this network**.

---

## 5. Gestione Utenti e Sicurezza
Durante la copia dei pacchetti, è obbligatorio configurare le credenziali di accesso:

1. **Root Password**: Definizione della password per l'utente amministratore (Root). Se la password è ritenuta debole, il sistema richiede di cliccare "Done" due volte per confermare.
2. **User Creation**: Creazione di un utente standard (es. `nomeutente`). È consigliabile spuntare l'opzione **"Make this user administrator"** per facilitare le operazioni di laboratorio.

---

## 6. Post-Installazione e Primo Accesso
Al termine della procedura, il sistema richiede un **Reboot**.
* **Licenza**: Al primo avvio, è necessario accettare i termini della licenza (License Information).
* **Welcome Screen**: Configurare le ultime preferenze (servizi di localizzazione, account online) e chiudere la guida introduttiva di GNOME.
* **Accesso al Terminale**: Per l'esecuzione dei comandi, cliccare con il tasto destro sul desktop e selezionare **"Open Terminal"**.

---
**Sintesi**: Il sistema operativo è ora operativo e pronto per l'esecuzione di comandi in ambiente shell.

# Installazione di CentOS Stream 8

Questa guida descrive il processo di installazione di CentOS versione 8. Sebbene esistano versioni più recenti (9 Stream) o precedenti (7), la versione 8 rimane un punto di riferimento per molti ambienti di sviluppo.

---

## 1. Preparazione e Download dell'Immagine ISO
Il processo inizia con l'acquisizione del supporto di installazione corretto dal sito ufficiale.

* **Sorgente**: Accedere a `centos.org` e navigare nella sezione **CentOS Stream**.
* **Versione**: Selezionare la versione **8**.
* **Architettura**: Scegliere **x86_64**.
* **Mirror**: Selezionare un mirror locale e scaricare il file con estensione `.iso` di dimensioni maggiori (indicativamente **10.7 GB**, etichettato come `dvd1.iso`), per assicurarsi di avere tutti i pacchetti necessari offline.

---

## 2. Creazione della Macchina Virtuale (VM)
Durante la creazione della VM in Oracle VirtualBox, è necessario prestare attenzione alla compatibilità:

* **Nome**: Assegnare un nome identificativo (es. `CentOS_8` o `myLinuxVM`).
* **Tipo e Versione**: In caso di problemi di compatibilità con il profilo "Red Hat", selezionare il profilo generico **"Linux 2.6 / 3.x / 4.x (64-bit)"**.
* **Risorse**: 
    * **RAM**: Almeno 2 GB (2048 MB) consigliati.
    * **Disco Rigido**: Minimo **20 GB**, allocato dinamicamente (VDI).
* **Rete**: Modificare l'impostazione da NAT a **Bridged Adapter** nelle impostazioni di rete per permettere alla VM di interfacciarsi direttamente con il router e accedere a Internet.

---

## 3. Avvio dell'Installazione
1.  Avviare la VM e selezionare il file ISO scaricato come disco di avvio.
2.  Nel menu di boot, selezionare **"Install CentOS Stream 8"** (usare i tasti freccia e Invio).
3.  Per rilasciare il puntatore del mouse dall'interno della finestra della VM, premere il tasto **CTRL destro** sulla tastiera.

---

## 4. Configurazione del Sistema (Installation Summary)
Nella schermata principale dell'installer, configurare i seguenti moduli:

### Destinazione e Software
* **Installation Destination**: Selezionare il disco virtuale da 20 GB. Assicurarsi che ci sia il segno di spunta e cliccare su "Done".
* **Software Selection**: Selezionare **"Server with GUI"** per ottenere un ambiente desktop grafico completo.

### Rete e Hostname
* **Hostname**: Inserire il nome del computer (es. `my-linux-vm`) e cliccare su **Apply**.
* **Network**: Portare l'interruttore su **ON** per attivare la scheda di rete e ottenere un indirizzo IP automaticamente.

### Sicurezza e Utenti
* **Root Password**: Definire la password dell'amministratore di sistema. Se la password è semplice, cliccare "Done" due volte per confermare.
* **User Creation**: Creare un utente con il proprio nome. Spuntare l'opzione **"Make this user administrator"** per concedere i privilegi di amministrazione (SUDO) al proprio utente personale.

---

## 5. Completamento e Primo Accesso
* Al termine della copia dei pacchetti, cliccare su **Reboot System**.
* **Licenza**: Al riavvio, cliccare su "License Information", accettare i termini e cliccare su **Finish Configuration**.
* **Accesso**: Effettuare il login con l'utente creato.
* **Terminale**: Per iniziare a lavorare, cliccare su **Activities** in alto a sinistra e selezionare l'icona del **Terminal**.

---
**Sintesi**: La versione 8 di CentOS Stream è ora configurata correttamente con interfaccia grafica e connettività di rete attiva.

# Installazione di CentOS Stream 9

Questa guida illustra i passaggi per configurare l'ultima versione della distribuzione, **CentOS Stream 9**, ottimizzata per chi desidera un ambiente Linux moderno e allineato con i futuri sviluppi di RHEL.

---

## 1. Configurazione della Macchina Virtuale (VM)
Indipendentemente dal software di virtualizzazione scelto, i parametri consigliati per CentOS 9 sono i seguenti:

* **Software**: Oracle VirtualBox (preferito nel corso) o VMware Player.
* **Nome VM**: `CentOS_9_Stream`.
* **Tipo e Versione**: Se il profilo "Red Hat" non è disponibile o crea conflitti, selezionare **"Linux 2.6 / 3.x / 4.x / 5.x (64-bit)"**.
* **Memoria RAM**: 1024 MB (1 GB) come requisito minimo.
* **Disco Rigido**: 20 GB (o superiore) con allocazione dinamica (VDI/VMDK).
* **Rete**: Impostare la scheda di rete su **"Bridged Adapter"** (Scheda con bridge) per garantire l'accesso diretto a Internet.

---

## 2. Download dell'Immagine ISO
L'immagine deve essere scaricata dal portale ufficiale:
1.  Visitare `centos.org` e selezionare **CentOS Stream**.
2.  Scegliere la versione **9**.
3.  Selezionare l'architettura **x86_64**.
4.  Scaricare il file `.iso` (operazione che richiede dai 5 ai 10 minuti a seconda della connessione).

---

## 3. Avvio del Processo di Installazione
* **Montaggio Media**: Avviare la VM e collegare il file ISO tramite l'icona della cartella (VirtualBox) o le impostazioni CD/DVD (VMware).
* **Boot Menu**: Selezionare **"Install CentOS Stream 9"** usando le frecce della tastiera e premendo Invio.
* **Interazione**: Premere il tasto **CTRL destro** per liberare il mouse dalla finestra della VM se necessario.

---

## 4. Riepilogo Installazione (Installation Summary)
All'interno dell'installer grafico, configurare i seguenti elementi chiave:

### Sistema e Software
* **Destination**: Selezionare il disco da 20 GB. Cliccare su "Done" per confermare il partizionamento automatico.
* **Software Selection**: Assicurarsi che sia selezionato **"Server with GUI"** per disporre dell'interfaccia grafica GNOME.
* **Network & Hostname**: 
    * Definire l'Hostname (es. `MyLinuxVM`).
    * Attivare l'interruttore Ethernet (ON) e verificare la ricezione dell'indirizzo IP.

### Impostazioni Utente
* **Root Password**: Impostare la password per l'amministratore (Root). 
    * *Nota*: In CentOS 9 è consigliabile spuntare **"Allow root SSH login with password"** per agevolare l'accesso remoto in ambiente di laboratorio.
    * Cliccare "Done" due volte se la password è considerata debole.
* **User Creation**: Creare un account utente standard (es. `iafzal`). Definire una password e cliccare "Next".

---

## 5. Post-Installazione e Setup Finale
Dopo il riavvio (Reboot), il sistema presenterà una procedura di benvenuto:
1.  **Start Setup**: Seguire la guida rapida.
2.  **Privacy**: Disabilitare i servizi di localizzazione se non necessari.
3.  **Account Online**: Cliccare su "Skip".
4.  **Terminal**: Accedere alle "Activities" e cliccare sull'icona del **Terminale**.

---
**Sintesi**: L'installazione di CentOS Stream 9 è completata. Il sistema è ora pronto per la configurazione dei servizi e l'apprendimento dei comandi Linux.

* Installare Linux su una piattaforma Cloud

Fare una ricerca su google "aws free account" e andare sul risultato di ricerca contenente "Free Cloud Computing Services". Qui bisogna quindi creare l'account. Dopodichè bisogna andare in EC2, selezionare la regione più vicina al punto in cui si vuole lavorare e cliccare su "Launch instance", pertanto bisogna configurare l'istanza. Nelle OS Images si può selezionare Red Hat.

# Conclusione e Prossimi Passi: Introduzione a Red Hat Enterprise Linux (RHEL)

Con il completamento dell'installazione di CentOS (versioni 7, 8 o 9), disponete ora di un ambiente di laboratorio solido e funzionale. La prossima fase del corso introduce l'installazione di **Red Hat Enterprise Linux (RHEL)**.

---

## 1. Relazione tra CentOS e RHEL
Come discusso in precedenza, CentOS e RHEL sono strettamente correlati. 
* **RHEL**: È lo standard industriale per gli ambienti corporate e mission-critical.
* **Obiettivo**: Acquisire familiarità con RHEL vi fornirà un vantaggio competitivo diretto nel mercato del lavoro IT professionale.

---

## 2. Note sull'Installazione di RHEL
L'imminente lezione dedicata a Red Hat è considerata **opzionale** ma caldamente raccomandata per i seguenti motivi:

| Caratteristica | Dettaglio |
| :--- | :--- |
| **Opzionalità** | Non è strettamente necessaria per proseguire il corso, poiché CentOS è sufficiente. |
| **Esperienza Pratica** | Permette di sperimentare le sottili differenze nella gestione delle licenze e dei repository ufficiali. |
| **Standard Aziendale** | Red Hat è il sistema operativo di riferimento per la maggior parte delle infrastrutture enterprise. |

---

## 3. Continuità del Corso
È importante notare che:
* Tutte le lezioni successive di questo corso verranno eseguite e dimostrate sull'installazione di **CentOS** completata nei passaggi precedenti.
* La coerenza tra CentOS e RHEL garantisce che ogni comando appreso sarà perfettamente trasportabile da un sistema all'altro.

---
> **Suggerimento dell'istruttore**: Sebbene possiate continuare esclusivamente con CentOS, vi incoraggio a tentare l'installazione di Red Hat per ottenere una visione completa dell'ecosistema Linux professionale.

# Guida Tecnica: Installazione di Red Hat Enterprise Linux (RHEL) su VirtualBox

Questa guida descrive il processo di installazione di RHEL 9.3 (o versioni successive). Sebbene l'installazione sia speculare a quella di CentOS, RHEL rappresenta lo standard per il mondo aziendale (corporate environment).

---

## 1. Fase di Download: Portale Developer
Per utilizzare RHEL gratuitamente per scopi didattici, è necessario ottenere una sottoscrizione per sviluppatori.

1.  **Accesso**: Naviga su [developer.redhat.com](https://developer.redhat.com).
2.  **Registrazione**: Clicca su "Register for a Red Hat account". Sarà necessario fornire:
    * Nome e Cognome
    * Email e Telefono
    * Ruolo lavorativo e Azienda (es. "Student" o "Home Lab")
3.  **Download ISO**: Seleziona la versione più recente (es. **9.3**).
    * **Architettura**: x86_64
    * **File**: DVD ISO (circa 10 GB). Questo file è il pacchetto completo per l'installazione offline.

---

## 2. Preparazione della Macchina Virtuale (Shell)
Prima di installare il sistema operativo, creiamo la "scocca" virtuale su Oracle VirtualBox.

* **Nome**: `Redhat-VM`
* **Tipo**: Linux
* **Versione**: Red Hat (64-bit)
* **Configurazione Hardware**:
    * **RAM**: 1024 MB (1 GB) minimo, 2 GB consigliati.
    * **CPU**: 1 o più core.
    * **Disco Rigido**: 20 GB (VDI, allocato dinamicamente).
* **Rete**: In *Impostazioni > Rete*, seleziona **"Scheda con bridge"** (Bridged Adapter) per consentire alla VM di navigare su internet tramite il tuo router.

---

## 3. Configurazione dell'Installer (Wizard Grafico)
Una volta avviata la VM e caricata l'immagine ISO, l'installer si divide in tre categorie principali:

### A. Localization (Localizzazione)
* **Keyboard**: Impostata di default su English (US), modificabile in base alla tua tastiera.
* **Date & Time**: Imposta il fuso orario corretto (es. Europe/Rome).

### B. Software
* **Software Selection**: Seleziona **"Server with GUI"** per avere l'interfaccia grafica.
* **Installation Source**: Lascia "Local Media" (l'ISO caricata).

### C. System (Sistema)
* **Installation Destination**: Clicca sul disco da 20 GB per confermare il partizionamento automatico.
* **Network & Host Name**: 
    * Scegli un nome (es. `redhat-linux`) e clicca su **Apply**.
    * Attiva l'interruttore della scheda di rete (ON) per ricevere un indirizzo IP.
* **Root Password**: Crea la password per l'amministratore. Spunta **"Allow root SSH login with password"** per laboratori futuri.
* **User Creation**: Crea un utente regolare (es. `tuonome`) e imposta la password.

---

## 4. Finalizzazione
1.  Clicca su **Begin Installation**.
2.  Attendi il completamento (circa 10-15 minuti).
3.  Clicca su **Reboot System**.
4.  Al riavvio, completa il setup iniziale (Setup guide), accetta i termini e apri il **Terminal** dalle "Activities".

---

> **Nota Finale**: CentOS e RHEL sono tecnicamente identici per quasi tutti i comandi che impareremo. La differenza principale è che RHEL richiede una sottoscrizione a pagamento per il supporto aziendale, mentre CentOS è Open Source e gratuito.

---

# Guida all'Installazione di Ubuntu Linux su VirtualBox

Questa lezione illustra come configurare una macchina virtuale per **Ubuntu Linux**. Sebbene il corso si focalizzi su CentOS, questa guida è fornita per ampliare le tue competenze su una delle distribuzioni Linux più popolari al mondo.

---

## 1. Creazione della Macchina Virtuale (VM)
Apri Oracle VirtualBox e clicca su **"New"** (Nuova). Grazie all'intelligenza del software, inserendo il nome corretto molti campi verranno auto-compilati:

* **Name**: `Ubuntu Linux`
* **Type**: Linux (selezionato automaticamente)
* **Version**: Ubuntu (64-bit) (selezionato automaticamente)
* **Memory (RAM)**: 
    * Minimo: 2048 MB (2 GB).
    * Consigliato: Aumenta leggermente se il tuo computer host ha 8 GB o più di RAM.
* **Processors (CPU)**: 
    * Minimo: 1 CPU.
    * Consigliato: 2 CPU per una maggiore fluidità.
* **Hard Disk**: Crea un disco virtuale da **25 GB** (impostazione predefinita). Clicca su **"Finish"**.

---

## 2. Download dell'Immagine ISO
Per installare il sistema, è necessario il file d'installazione ufficiale:

1.  Vai su Google e cerca **"Download Ubuntu"**.
2.  Seleziona il link ufficiale di **Ubuntu Desktop**.
3.  Scarica la versione **LTS** (Long Term Support), ad esempio la **22.04**. 
    * *Nota: La versione specifica potrebbe cambiare nel tempo, ma il processo rimane lo stesso.*
4.  Salva il file `.iso` sul tuo computer (Desktop o cartella Download).

---

## 3. Avvio e Montaggio dell'ISO
1.  Seleziona la VM appena creata e clicca su **"Start"**.
2.  Al prompt di avvio (percorso del disco ottico), clicca sull'icona della cartella piccola.
3.  Scegli **"Other"**, naviga fino alla posizione del file ISO scaricato, selezionalo e clicca su **"Open"**.
4.  Clicca su **"Mount and Retry Boot"**.
5.  Nel menu di avvio di Ubuntu, seleziona la prima opzione e premi **Invio**.



---

## 4. Configurazione del Sistema Operativo
Una volta caricato l'installer grafico:

* **Welcome**: Clicca su **"Install Ubuntu"**.
* **Keyboard Layout**: Seleziona la tua lingua (es. English US o Italian). Clicca su **"Continue"**.
* **Updates and Other Software**: 
    * Scegli **"Normal Installation"**.
    * *Consiglio*: Disabilita "Download updates while installing" se la connessione è lenta, per velocizzare il processo iniziale.
* **Installation Type**: Seleziona **"Erase disk and install Ubuntu"**. 
    * *Importante*: Questa operazione formatterà solo i 25 GB allocati alla VM, non il tuo computer fisico.
* **Where are you?**: Seleziona il tuo fuso orario (es. New York o Rome).

---

## 5. Creazione Utente e Hostname
Inserisci i tuoi dati personali:

* **Your name**: Il tuo nome completo (es. `Imran Afzal`).
* **Your computer's name**: L'hostname della macchina (es. `Ubuntu-Linux`).
* **Pick a username**: Il nome breve per il login (es. `afzal`).
* **Password**: Scegline una e confermala. Seleziona **"Require my password to log in"** per sicurezza.

Clicca su **"Continue"** e attendi il completamento della copia dei file (10-30 minuti).

---

## 6. Operazioni Post-Installazione
Dopo il riavvio (**Restart Now**), il sistema è pronto.

### Connessione Internet (Bridged Network)
Per assicurarti che Ubuntu possa scaricare aggiornamenti:
1.  Vai in **Settings** della VM in VirtualBox.
2.  Seleziona **Network** > **Attached to: Bridged Adapter**.
3.  Clicca OK. Questo permetterà alla VM di ottenere un IP direttamente dal tuo router.

### Accesso al Terminale
Per iniziare a scrivere comandi:
* Fai clic destro sul Desktop e seleziona **"Open Terminal"**.
* Puoi testare i comandi classici:
    * `df -h` (per vedere lo spazio su disco)
    * `ls -l` (per elencare i file)

---
> **Curiosità**: Circa l'80-85% dei comandi Linux appresi su CentOS e Red Hat sono identici su Ubuntu. Divertiti a sperimentare!

# Guida Tecnica: Gestione degli Snapshot in VirtualBox

Dopo aver completato con successo l'installazione di Linux, è fondamentale mettere in sicurezza il lavoro svolto. Lo strumento ideale per farlo è lo **Snapshot**.

---

## 1. Cos'è uno Snapshot?
Uno snapshot è una "fotografia" istantanea dello stato attuale della tua Macchina Virtuale (VM). Ti permette di salvare la configurazione, i file e lo stato del sistema in un preciso momento.

* **Perché è importante?** Se durante il corso dovessi commettere un errore critico (ad esempio cancellare file di sistema o sbagliare una configurazione di rete), non dovrai reinstallare tutto da capo.
* **Ripristino rapido**: Ti basta un clic per riportare la VM allo stato salvato in precedenza (Restore).



---

## 2. Come Creare uno Snapshot
Segui questi passaggi per creare il tuo primo punto di ripristino:

1.  **Spegni la VM**: Assicurati che la tua macchina virtuale sia in stato **"Powered Off"** (Spenta). Se è accesa o in pausa, procedi allo spegnimento completo.
2.  **Seleziona la VM**: Nel pannello sinistro di Oracle VirtualBox, clicca sulla tua VM (es. `CentOS-9-Stream`).
3.  **Accedi al Menu Snapshot**: Clicca sul menu a tendina accanto al nome della VM (spesso rappresentato da tre linee o un'icona blu) e seleziona **"Snapshots"**.
4.  **Crea lo Snapshot**: Clicca sul pulsante **"Take"** (Scatta).
5.  **Assegna un Nome**: Inserisci un nome descrittivo, ad esempio: 
    * *Nome*: `Primo Snapshot Post-Installazione`
    * *Descrizione*: `Sistema pulito appena installato.`
6.  **Conferma**: Clicca su **OK**. VirtualBox impiegherà pochi secondi per salvare lo stato.

---

## 3. Come Ripristinare (Restore)
Se in futuro la tua macchina non dovesse più avviarsi o presentasse errori:

1.  Spegni la VM.
2.  Torna nella sezione **Snapshots**.
3.  Seleziona lo snapshot creato in precedenza.
4.  Clicca sul pulsante **"Restore"** (Ripristina).
5.  Al riavvio, la VM sarà esattamente come il giorno in cui l'hai installata.

---

> **Suggerimento**: Prendi l'abitudine di fare uno snapshot prima di ogni esercizio complesso o prima di modificare file di configurazione critici. Ti farà risparmiare ore di troubleshooting!

---

# Gestione delle Macchine Virtuali (VM Management)

Nel mondo aziendale moderno, circa l'**80% delle infrastrutture** gira su ambienti virtualizzati (VMware, Red Hat Virtualization, Oracle VirtualBox o Microsoft Hyper-V). Come amministratore di sistema, una delle tue responsabilità principali sarà gestire le risorse di queste macchine.



---

## 1. Perché la Virtualizzazione?
A differenza delle macchine fisiche, dove aggiungere componenti richiede lo spegnimento, l'apertura del rack e l'inserimento manuale di hardware (DIMM per la RAM o CPU fisiche), le VM offrono una flessibilità estrema.

* **Hot Add/Remove**: In molti sistemi moderni, è possibile aggiungere CPU o RAM "on the fly" (a caldo), senza spegnere il server.
* **Ottimizzazione**: Puoi allocare solo ciò che serve, evitando sprechi di risorse fisiche.

---

## 2. Risorse Principali e Impostazioni
In Oracle VirtualBox, cliccando con il tasto destro sulla tua VM e selezionando **Settings**, puoi gestire i seguenti parametri:

### General (Generale)
* **Name**: Cambia il nome visualizzato nell'elenco di VirtualBox (Attenzione: questo non cambia l'hostname interno del sistema Linux).
* **Advanced**: Qui puoi impostare il **Shared Clipboard** (Appunti condivisi) e il **Drag'n'Drop** in modalità *Bidirectional*. Questo ti permette di copiare testi o file dal tuo PC (Host) alla VM (Guest) e viceversa.

### System (Sistema)
* **Motherboard**: Qui regoli la **Base Memory** (RAM). Se un utente lamenta lentezze, puoi aumentare la RAM semplicemente trascinando lo slider (a macchina spenta).
* **Processor**: Puoi aumentare il numero di core della CPU dedicati alla macchina.

### Storage (Archiviazione)
Qui gestisci i dischi virtuali (.vdi).
* **Virtual Size vs Actual Size**: Una VM può avere un disco allocato da 10GB (Virtual), ma occupare solo 4GB sul tuo PC (Actual). Il sistema crescerà dinamicamente man mano che aggiungerai file, fino al limite stabilito.

### Network (Rete)

* **NAT**: La VM usa l'IP del tuo computer per navigare, ma è "nascosta" dall'esterno.
* **Bridged Adapter**: La VM riceve un IP reale dal tuo router, diventando un dispositivo a tutti gli effetti nella tua rete locale (necessario per servizi web o SSH esterno).
* **Host-only**: La VM comunica solo con il tuo PC, senza accesso a Internet.

---

## 3. Snapshot vs Backup
Sebbene lo snapshot salvato in precedenza sia uno strumento potente per "tornare indietro" dopo un errore di configurazione o un codice malfunzionante, ricorda che **uno snapshot non è un backup completo**. Serve per la gestione rapida dello stato, non per la conservazione dei dati a lungo termine.

---

> **Consiglio per la carriera**: Durante i colloqui di lavoro per posizioni Linux Admin, aspettati spesso domande sulla gestione delle VM. Sapere come scalare le risorse di un server virtuale è una prova della tua esperienza operativa.

# Guida ai Tasti della Tastiera e Simboli in Linux

In Linux, molti simboli presenti sulla tastiera hanno nomi specifici e funzioni che vanno oltre la semplice digitazione. Questa guida ti aiuterà a riconoscerli, pronunciarli e capire quando usarli.

---

## 1. Tasti di Fuga e Navigazione
* **ESC (Escape)**: Il tuo "salvavita". Si usa per uscire dalle modalità di inserimento (es. nell'editor VI) o per annullare un comando iniziato.
* **TAB**: Fondamentale in Linux per l'**autocompletamento**. Inizia a scrivere un comando o un percorso e premi TAB per completarlo automaticamente.
* **Right Control (Ctrl Destro)**: In VirtualBox, è il tasto "Host". Se il mouse è bloccato dentro la VM, premendo il Ctrl di destra potrai liberarlo e tornare al tuo sistema principale.

---

## 2. Simboli della Prima Riga (Numeri + Shift)



| Simbolo | Nome Tecnico | Uso comune in Linux |
| :--- | :--- | :--- |
| **~** | **Tilde** | Rappresenta la tua **Home Directory** (es. `cd ~`). |
| **\`** | **Backtick** | Da non confondere con l'apostrofo. Serve per l'esecuzione di comandi nidificati. |
| **!** | **Bang** / Exclamation | Usato per richiamare la cronologia dei comandi (es. `!!` ripete l'ultimo comando). |
| **#** | **Hash** / Pound / Hash-tag | Usato negli script per i **commenti** (tutto ciò che segue # non viene eseguito). |
| **$** | **Dollar** | Indica una **variabile** di sistema (es. `$PATH` o `$USER`). |
| **^** | **Caret** / Cap | Indica l'inizio di una riga (molto usato con il comando `grep`). |
| **&** | **Ampersand** | Mette un processo in **background** (es. `comando &`). |
| ***** | **Asterisk** / Star | È il carattere "jolly" (wildcard). Rappresenta "tutto" (es. `rm *.txt`). |

---

## 3. Parentesi e Segni di Punteggiatura
* **Parentesi Tonde `( )`**: *Parentheses* (Open/Closed). Usate in programmazione e funzioni.
* **Parentesi Graffe `{ }`**: *Curly Braces*. Usate per espansioni di nomi o blocchi di codice.
* **Parentesi Quadre `[ ]`**: *Brackets*. Usate per test di condizioni negli script.
* **Trattino `-`**: *Dash* / Hyphen. Introduce le **opzioni** dei comandi (es. `ls -l`).
* **Underscore `_`**: Trattino basso. Usato spesso nei nomi dei file al posto dello spazio.

---

## 4. Simboli Critici per il Terminale
* **Pipe `|`**: (Sopra il tasto backslash). È una delle funzioni più potenti di Linux: prende l'output di un comando e lo passa come input al successivo.
* **Forward Slash `/`**: Usato per i **percorsi (path)** in Linux (es. `/home/user`). Nota: Windows usa il Backslash `\`.
* **Greater Than `>`**: Operatore di **redirezione**. Salva l'output di un comando in un file (sovrascrivendolo).
* **Double Greater Than `>>`**: **Appende** l'output alla fine di un file senza cancellarne il contenuto.
* **Less Than `<`**: Prende l'input da un file.

---

> **Nota sulla pronuncia**: Anche se alcuni nomi possono variare in base alla lingua, termini come **Bang** (!), **Pipe** (|) e **Tilde** (~) sono standard internazionali nel mondo dei sistemisti Linux.

---
