# Modulo 1

## Linux nella vita quotidiana

Esempi di Linux nella vita di tutti i giorni:

| Categoria | Descrizione |
| :--- | :--- |
| **Aereo** | I film durante il volo si trovano su un sistema Linux integrato (embedded). |
| **Accesso a Internet** | Il router wifi probabilmente ha una copia integrata di Linux che esegue una applicazione gestionale web. |
| **TV** |L'interfaccia utente che si vede a schermo spesso esegue una versione integrata di Linux |
| **ricerca Google** | Quasi tutta la roba back office che è usata per generare i risultati delle tue query viene eseguita su Linux |
| **Siti web** | Molto probabilmente il sito web che utilizzi più di frequente viene eseguito su un server Linux |
| **Smartphone** | Molte persone utilizzano un cellulare android o un iPhone che è basato su un open source derivante da Unix |
| **Tablets (esempio e-books)** | La maggior parte contiene software open source da qualche parte. |
| **Disco di ripristino dati di Windows** | Molte sono distribuzioni Linux personalizzate |
| **Elettrodomestici** | I nuovi elettrodomestici con funzionamento tramite touch screen girano su Linux.|
| **Auto a guida autonoma** | Eseguono Linux. |

## Cos'è un sistema operativo

Un sistema operativo (OS dall'inglese operating system) è un software che agisce come un "uomo nel mezzo" o un ponte tra l'hardware del computer e l'utente del computer. Esso fornisce un'interfaccia utente e controlla l'hardware del computer così che il software possa funzionare correttamente.

Ci sono diversi tipi di sistema operativo come ad esempio:

 - Sistemi operativi desktop: ad esempio Microsoft Windows, macOS, Linux (such as Ubuntu)
 - Sistemi operativi server: Windows Server, distribuzioni Linux (in gergo chiamate distro) come ad esempio CentOS o Red Hat Enterprise Linux
 - Sistemi operativi mobile: Android, iOS, Windows Mobile
 - Sistemi operativi integrati (embedded): usati in dispositivi come router, smart TV, automobili, elettrodomestici e così via
 - Sistemi operativi in tempo reale (RTOS): usati in sistemi critici come ad esempio equipaggiamento medico, in ambito aeronautico, nella difesa, nei firewall di rete, nei sistemi di sicurezza per la casa, la centralina elettronica dell'auto, ecc...

## Cos'è Linux

Linux è un sistema operativo gratuito e open-source. Esso è simile a Windows e macOS ma è diverso in molte maniere. Linux è molto famoso per la sua stabilità, sicurezza e flessibilità. Può essere modificato e distribuito da chiunque, e questo ha portato a diverse versioni chiamate distribuzioni, e ogni distribuzione è tagliata per usi diversi. La sua natura open-source significa che una community di sviluppatori e utenti contribuisce al suo sviluppo.

Linux è largamente utilizzato in server e cloud computing. La sua filosofia è il software libero. Ha una forte interfaccia a linea di comando (CLI). Permette una elaborazione più veloce, una maggiore sicurezza, una personalizzazione derivante dalla sua natura open-source, ha il supporto della community, permette di capire il funzionamento degli altri sistemi operativi e aumenta le opportunità di carriera. 

## Storia

Prima di Linux c'era Unix sviluppato nel 1970 da AT&T. Dopo, nel 1983, è arrivato il progetto GNU (GNU's not UNIX) da Richard Stallman. Dopo arrivò Linux da Linus Torvalds che sviluppò il kernel Linux basato su GNU e così nacqus il sistema operativo. Da quel momento c'è stata l'evoluzione che conosciamo tutti fino ad oggi.

## Differenza tra Unix e Linux

Unix è stato sviluppato da AT&T, General Electric e il Massachusetts institute of Technology (MIT). Era multi utente e multi tasking. Dopo nacque Linux nel 1991 grazie a Linus Torvalds. Unix non era gratuito, mentre Linux sì, ed era anche open source. Unix è utilizzato da Sun as Solaris, HP-UX, AIX etc... mentre Linux è usato da molto sviluppatori e aziende (Redhat, CentOS, Debian, etc...). Unix supporta pochi file system. Linux può essere installato su una grande varietà di hardware computer, che spaziano da cellulari, tablet, console per videogame a mainframe e supercomputer.

## Distribuzioni Linux

| Distribuzione | Logo | Descrizione Breve | Scopo |
| :--- | :---: | :--- | :--- |
| **Ubuntu** | <img src="https://upload.wikimedia.org/wikipedia/commons/a/ab/Logo-ubuntu_coco.png" width="40"> | La distro più famosa al mondo, basata su Debian. Offre un'esperienza "chiavi in mano". | Ideale per principianti, desktop e sviluppatori che cercano stabilità. |
| **Fedora** | <img src="https://upload.wikimedia.org/wikipedia/commons/3/3f/Fedora_logo.svg" width="40"> | Nota per includere sempre le ultimissime versioni dei software e delle tecnologie. | Per chi vuole provare le novità tecnologiche prima di chiunque altro. |
| **Debian** | <img src="https://upload.wikimedia.org/wikipedia/commons/6/66/Openlogo-debian.svg" width="40"> | Una delle distro più vecchie, focalizzata su stabilità granitica e software libero. | Utilizzata principalmente per server e infrastrutture critiche. |
| **RHEL** | <img src="https://upload.wikimedia.org/wikipedia/commons/d/d8/Red_Hat_logo.svg" width="40"> | **Red Hat Enterprise Linux**. Sistema commerciale con supporto professionale 24/7. | Scelta obbligata per grandi aziende, banche e governi. |
| **CentOS** | <img src="https://upload.wikimedia.org/wikipedia/commons/9/9e/Centos_official_logo.svg" width="40"> | Storicamente la copia gratuita di RHEL. Ora esiste come CentOS Stream. | Usata per studio, test o server aziendali senza costi di licenza. |
| **Arch Linux** | <img src="https://upload.wikimedia.org/wikipedia/commons/a/a5/Archlinux-vert-dark.svg" width="40"> | Distro minimalista con installazione manuale e modello *Rolling Release*. | Per esperti che vogliono il controllo totale su ogni pacchetto. |
| **openSUSE** | <img src="https://upload.wikimedia.org/wikipedia/commons/d/d0/OpenSUSE_Logo.svg" width="40"> | Progetto solido con lo strumento di configurazione **YaST** per gestire tutto via GUI. Si divide in tumbleweed (con aggiornamenti continui (rolling release), ideale in ambiente di sviluppo e test) e leap (con aggiornamento a rilascio fisso, ideale per l'ambiente di produzione in quanto l'aggiornamento è già testato e stabile) | Ottima alternativa professionale a Windows. |
| **Linux Mint** | <img src="https://upload.wikimedia.org/wikipedia/commons/3/3f/Linux_Mint_logo_without_text.svg" width="40"> | Basata su Ubuntu, familiare per chi viene da Windows, con interfaccia classica. | Perfetta per chi vuole un computer pronto all'uso immediatamente. C'è un tool integrato per eseguire i media. |
| **Gentoo** | <img src="https://upload.wikimedia.org/wikipedia/commons/4/48/Gentoo-logo.svg" width="40"> | Ogni pacchetto viene compilato dai sorgenti specificamente per l'hardware dell'utente. | Per chi cerca le massime prestazioni e vuole imparare Linux a fondo. |
| **Slackware** | <img src="https://upload.wikimedia.org/wikipedia/commons/2/22/Slackware_logo.svg" width="40"> | La più antica ancora mantenuta. Design semplice e fedele alla filosofia Unix. | Per i puristi che preferiscono configurazioni manuali e file di testo. |
| **Alpine Linux** | <img src="https://upload.wikimedia.org/wikipedia/commons/c/ca/Alpine_Linux_logo.svg" width="40"> | Distro incredibilmente leggera e sicura, basata su musl libc e BusyBox. | Lo standard per i **container Docker** grazie alle dimensioni minuscole. |
| **Kali Linux** | <img src="https://upload.wikimedia.org/wikipedia/commons/2/2b/Kali-linux-logo.svg" width="40"> | Basata su Debian, pre-installata con strumenti per la sicurezza informatica. | Utilizzata esclusivamente per **Cybersecurity** e Penetration Testing. |

## Utenti Linux

Linux è utilizzato da utenti e organizzazioni, inclusi sviluppatori e università, ma anche agenzie governative. Altri utenti possono essere le imprese, le compagnie tech e i server cloud e web. Ma esso viene utilizzato anche da supercomputer, nelle telecomunicazioni e nel networking. Il suo utilizzo può essere ritrovato anche nei media e nell'intrattenimento (Netflix).

### Linux vs Windows

Linux è open source, per cui è facile da studiare. Windows invece è un software proprietario e il proprietario è Microsoft. 
Il costo di Linux è zero, è gratuito, mentre Windows è a pagamento. 
L'interfaccia Linux si presenta in modi diversi come ad esempio GNOME; KDE e XFCE. In Windows abbiamo una GUI standard in tutte le versioni. Windows ha un prompt dei comandi e PowerShell. 
In Linux invece la console è il cuore pulsante. Per quanto riguarda l'installazione di software e la gestione dei pacchetti, in Linux il software è generalmente installato usando gestori di pacchetto come APT, YUM o Pacman. In Windows invece abbiamo dei file eseguibili (.exe) che permettono di effettuare le installazioni.
Linux è generalmente considerato molto sicuro. Windows invece essendo largamente utilizzato dagli utenti, è più soggetto a essere obiettivo di malware.
Linux è conosciuto per la sua stabilità e per le sue prestazioni. Windows sebbene sia stabile, tende a rallentare dopo un po' di tempo.
Per quanto riguarda i file system, Linux utilizzat ext4, XFS e Btrfs. In Windows invece i file system primari in uso sono NTFS e FAT32

# Varie Parti di un Sistema Operativo

I sistemi operativi UNIX e "UNIX-like" (come Linux) consistono in un **kernel** e in alcuni **programmi di sistema**. Esistono inoltre dei programmi applicativi per svolgere il lavoro. Il kernel è il cuore del sistema operativo. In realtà, viene spesso erroneamente considerato come il sistema operativo stesso, ma non è così. Un sistema operativo fornisce molti più servizi di un semplice kernel.

Il kernel tiene traccia dei file sul disco, avvia i programmi e li esegue simultaneamente, assegna la memoria e altre risorse ai vari processi, riceve e invia pacchetti alla rete, e così via. Il kernel fa molto poco da solo, ma fornisce gli strumenti con cui possono essere costruiti tutti i servizi. Impedisce inoltre a chiunque di accedere direttamente all'hardware, costringendo tutti a utilizzare gli strumenti che esso fornisce. In questo modo il kernel garantisce una certa protezione agli utenti l'uno dall'altro. Gli strumenti forniti dal kernel vengono utilizzati tramite le **chiamate di sistema** (*system calls*).

I programmi di sistema utilizzano gli strumenti forniti dal kernel per implementare i vari servizi richiesti da un sistema operativo. I programmi di sistema, e tutti gli altri programmi, girano "sopra il kernel", in quella che viene chiamata **modalità utente** (*user mode*). La differenza tra programmi di sistema e programmi applicativi è una questione di intenti: 

* Le **applicazioni** sono destinate a svolgere attività utili (o a giocare, se si tratta di un videogioco).
* I **programmi di sistema** sono necessari per far funzionare il sistema. 

Un elaboratore di testi è un'applicazione; `mount` è un programma di sistema. La differenza è spesso sfumata ed è importante solo per i categorizzatori compulsivi. Un sistema operativo può contenere anche compilatori e le relative librerie (in particolare **GCC** e la libreria C sotto Linux), sebbene non tutti i linguaggi di programmazione debbano far parte del sistema operativo. Anche la documentazione, e talvolta persino i giochi, possono farne parte.

---

# Parti importanti del kernel

Il kernel Linux è composto da diverse parti fondamentali:
* Gestione dei processi
* Gestione della memoria
* Driver dei dispositivi hardware
* Driver del filesystem
* Gestione della rete
* Varie altre componenti minori

<img src="https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg" width="40">

Probabilmente le parti più importanti del kernel (senza le quali nulla funzionerebbe) sono la **gestione della memoria** e la **gestione dei processi**. La gestione della memoria si occupa di assegnare aree di memoria e aree di spazio di **swap** ai processi, alle parti del kernel e per la cache del buffer. La gestione dei processi crea i processi e implementa il **multitasking** scambiando il processo attivo sul processore.

Al livello più basso, il kernel contiene un driver di dispositivo hardware per ogni tipo di hardware supportato. Poiché il mondo è pieno di diversi tipi di hardware, il numero di driver è elevato. Spesso esistono componenti hardware simili che differiscono nel modo in cui vengono controllati dal software. Queste somiglianze rendono possibile avere classi generali di driver che supportano operazioni simili; ogni membro della classe ha la stessa interfaccia verso il resto del kernel, ma differisce in ciò che deve fare per implementarle. Ad esempio, tutti i driver dei dischi appaiono uguali al resto del kernel, ovvero hanno tutti operazioni come "inizializza l'unità", "leggi settore N" e "scrivi settore N".

# Cos'è la memoria virtuale?

Linux supporta la **memoria virtuale**, ovvero l'uso del disco come estensione della RAM, in modo che la dimensione effettiva della memoria utilizzabile aumenti di conseguenza. Il kernel scrive il contenuto di un blocco di memoria attualmente inutilizzato sul disco rigido, in modo che quella porzione di RAM possa essere usata per altri scopi. Quando il contenuto originale è di nuovo necessario, viene riletto e riportato in memoria.

Tutto questo avviene in modo completamente trasparente per l'utente; i programmi in esecuzione su Linux vedono solo la maggiore quantità di memoria disponibile e non si accorgono che parti di essi risiedono periodicamente sul disco. Naturalmente, leggere e scrivere sul disco rigido è più lento (nell'ordine di migliaia di volte più lento) rispetto all'uso della memoria reale, quindi i programmi non girano altrettanto velocemente. La parte del disco rigido utilizzata come memoria virtuale è chiamata **spazio di swap** (swap space).

### Partizioni di Swap vs File di Swap

Linux può utilizzare come spazio di swap sia un normale file nel filesystem, sia una partizione separata:

* **Partizione di Swap:** È più veloce, ma è più difficile da ridimensionare (richiede il ripartizionamento del disco).
* **File di Swap:** È più facile da creare e ridimensionare senza dover riconfigurare l'intero disco.

**Consiglio:** Se non sai di quanto spazio hai bisogno, puoi iniziare con un file di swap, testare il sistema per un po' e, una volta determinata la dimensione ideale, creare una partizione di swap dedicata.

### Utilizzo Flessibile dello Swap

Linux permette di utilizzare **contemporaneamente** diverse partizioni e/o file di swap. Ciò significa che se occasionalmente hai bisogno di una quantità insolita di spazio di swap, puoi configurare un file di swap extra solo per quel periodo, invece di tenere allocata l'intera quantità stabilmente.

### Nota sulla terminologia

In informatica si distingue solitamente tra:
1.  **Swapping:** Scrittura dell'intero processo nello spazio di swap.
2.  **Paging:** Scrittura solo di parti a dimensione fissa (solitamente pochi kilobyte) alla volta.

Il **paging** è generalmente più efficiente ed è ciò che Linux fa realmente, anche se la terminologia tradizionale di Linux continua a usare il termine "swapping".

# All'interno di Linux (Inside Linux)

### Il Kernel
* **Il cuore del sistema UNIX:** Viene caricato all'avvio del sistema (boot) ed è un programma di controllo che risiede permanentemente nella memoria (memory-resident).
* **Gestione delle risorse:** Gestisce l'intero parco risorse del sistema, presentandole a te e a ogni altro utente come un sistema coerente. Fornisce servizi alle applicazioni utente come la gestione dei dispositivi, la pianificazione dei processi (scheduling), ecc.
* **Esempi di funzioni eseguite dal kernel:**
    * Gestione della memoria della macchina e allocazione della stessa a ciascun processo.
    * Pianificazione (scheduling) del lavoro svolto dalla CPU, affinché il lavoro di ogni utente sia eseguito nel modo più efficiente possibile.
    * Esecuzione del trasferimento di dati da una parte all'altra della macchina.
    * Interpretazione ed esecuzione delle istruzioni provenienti dalla shell.
    * Applicazione dei permessi di accesso ai file.
* *Nota: Non è necessario conoscere i dettagli del kernel per utilizzare un sistema UNIX; queste informazioni sono fornite solo a scopo informativo.*



---

### La Shell
* **Interfaccia di accesso:** Ogni volta che effettui l'accesso (login) a un sistema Unix, vieni inserito in un programma shell. Il "prompt" della shell è solitamente visibile nella posizione del cursore sullo schermo. Per svolgere il tuo lavoro, inserisci i comandi in corrispondenza di questo prompt.
* **Interprete di comandi:** La shell è un interprete; prende ogni comando e lo passa al kernel del sistema operativo affinché venga eseguito. Successivamente, mostra i risultati dell'operazione sul tuo schermo.
* **Varietà di Shell:** Su ogni sistema UNIX sono solitamente disponibili diverse shell, ognuna con i propri pregi e difetti. Utenti diversi possono utilizzare shell diverse. Inizialmente, l'amministratore di sistema fornirà una shell predefinita, che può essere sovrascritta o modificata.
* **Le shell più comuni:**
    * **sh** (Bourne shell)
    * **csh** (C shell)
    * **ksh** (Korn shell)
    * **tcsh** (TC Shell)
    * **bash** (Bourne Again Shell) - *La più diffusa oggi.*
* **Linguaggio di programmazione:** Ogni shell include anche il proprio linguaggio di programmazione. I file di comando, chiamati "**shell script**", vengono utilizzati per compiere una serie di attività automatizzate.

---

### Utility (Utilità)
* **Centinaia di programmi:** UNIX fornisce diverse centinaia di programmi di utilità, spesso definiti semplicemente "comandi".
* **Funzioni universali:** Svolgono compiti comuni come:
    * Editing di testi.
    * Manutenzione dei file.
    * Stampa.
    * Ordinamento (sorting).
    * Supporto alla programmazione.
    * Informazioni online, ecc.
* **Modularità:** Sono programmi modulari; singole funzioni possono essere raggruppate (tramite le "pipe") per eseguire compiti più complessi.
