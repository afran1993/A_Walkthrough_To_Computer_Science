# Lezione: Linux File Editor (vi)

**Istruttore:** Hello, everyone.

## Introduzione agli Editor di Testo
In questa lezione copriremo il **Linux File Editor**. Un editor è un programma che permette di creare e manipolare dati o testo in un file Linux. Esistono diversi editor standard disponibili sulla maggior parte dei sistemi Linux, tra cui:
* `vi`
* `ed`
* `ex`
* `emacs`
* `pico`
* `vim` (la versione avanzata di vi)

In questa lezione ci concentreremo su **vi** perché è disponibile in quasi ogni distribuzione Linux (Ubuntu, SUSE, Red Hat, CentOS) ed è molto facile da imparare.

---

## Comandi Principali di vi
L'editor vi fornisce comandi per inserire, eliminare, sostituire testo, navigare nel file, cercare stringhe e fare taglia/incolla.

| Tasto | Funzione |
| :--- | :--- |
| `i` | Entra in modalità Inserimento (**Insert**) |
| `Esc` | Torna alla modalità Comando |
| `r` | Sostituisce un singolo carattere |
| `x` | Elimina un singolo carattere |
| `dd` | Elimina l'intera riga corrente |
| `u` | Annulla l'ultima azione (**Undo**) |
| `o` | Crea una nuova riga sotto quella corrente ed entra in Inserimento |
| `a` | Inserisce testo dopo il cursore (**Append**) |
| `/testo` | Cerca la parola "testo" nel file |

---

## Esercitazione Pratica

### 1. Creare un file
Per creare un nuovo file, digita nel terminale:
`vi my_first_file`

### 2. Modalità Comando vs Modalità Inserimento
Quando apri vi, sei in **Modalità Comando**. Non puoi scrivere finché non entri in **Modalità Inserimento**.
* Premi `i` per iniziare a scrivere. Vedrai la scritta `-- INSERT --` in basso.
* Scrivi il tuo testo (es. "Hello world").
* Per smettere di scrivere e tornare ai comandi, premi il tasto `Esc`.

### 3. Salvare e Uscire
Esistono diversi modi per uscire dall'editor:
* **Salvare e uscire velocemente:** Premi `Esc` e poi `Shift + ZZ`.
* **Salvare e uscire (metodo classico):** Premi `Esc`, poi digita `:wq!` e premi `Invio`.
* **Uscire senza salvare:** Premi `Esc`, poi digita `:q!` e premi `Invio`.

### 4. Navigazione ed Editing
* **Spostarsi:** Usa le frecce direzionali (solo in modalità comando).
* **Cancellare:** Posiziona il cursore e premi `x` per i caratteri o `dd` per le righe.
* **Sostituire:** Usa `r` per cambiare una sola lettera sopra il cursore.
* **Cerca:** Digita `/` seguito dalla parola che cerchi e premi `Invio`.

---

## Consigli Finali
La chiave per padroneggiare `vi` è la pratica. Provate a creare file nelle vostre home directory, scrivete storie o liste e usate i diversi comandi per modificare il testo. Ricordate: se vi sentite bloccati, il tasto **Esc** è il vostro "salvavita" per tornare alla modalità sicura.

# Confronto vi vs vim e Guida al Comando sed

## 1. Differenze tra vi e vim
Sebbene i due editor condividano la stessa logica di base, esistono ragioni specifiche per conoscere entrambi.

### Perché usare vi?
* **Universalità:** È il primo editor introdotto con Linux/Unix. È presente su ogni sistema, inclusi i sistemi legacy come Solaris, AIX o HP-UX, dove spesso `vim` non è installato.
* **Semplicità:** È più leggero e ha un nome più breve da richiamare.

### Perché usare vim (vi IMproved)?
* **Funzionalità Avanzate:** Offre l'autocompletamento (simile al tasto Tab in Bash) e l'evidenziazione della sintassi.
* **Strumenti Extra:** Include controllo ortografico (spell check), supporto Unicode, plugin, modalità GUI e supporto per linguaggi di scripting.
* **Facilità:** Per molti è più intuitivo grazie alle sue estensioni moderne.

### Risorse per l'apprendimento interattivo
Per padroneggiare i tasti di movimento (`H`, `J`, `K`, `L`) e i comandi, l'istruttore suggerisce:
* **OpenVim.com:** Tutorial interattivo passo dopo passo.
* **VimGenius.com:** Per imparare i comandi tramite quiz.
* **Vim-Adventures.com:** Un gioco che insegna a usare l'editor navigando in una mappa.

---

## 2. Il comando sed (Stream Editor)
Il comando `sed` è uno strumento potentissimo per manipolare il testo all'interno di un file (sostituzioni, cancellazioni, modifiche) senza doverlo aprire manualmente.

### Concetti Chiave
* **Efficienza:** Ideale per file enormi (milioni di righe) dove una modifica manuale con `vi` sarebbe impossibile.
* **Visualizzazione vs Modifica:** Per default, `sed` stampa il risultato a video. Per applicare le modifiche permanentemente al file, è necessario usare l'opzione **`-i`** (insert/in-place).

### Esempi di Comandi Comuni

| Operazione | Comando |
| :--- | :--- |
| **Sostituzione Globale** | `sed 's/parola_vecchia/parola_nuova/g' file.txt` |
| **Salvare la sostituzione** | `sed -i 's/vecchio/nuovo/g' file.txt` |
| **Cancellare righe con testo specifico** | `sed '/testo_da_cercare/d' file.txt` |
| **Rimuovere righe vuote** | `sed '/^$/d' file.txt` |
| **Eliminare la prima riga** | `sed '1d' file.txt` |
| **Eliminare un intervallo (es. righe 1-2)** | `sed '1,2d' file.txt` |
| **Sostituire Tab con Spazi** | `sed 's/\t/ /g' file.txt` |
| **Mostrare solo righe specifiche (es. 12-18)** | `sed -n '12,18p' file.txt` |

### Esempio Pratico
Se hai un file di personaggi e vuoi cambiare il nome "Kenny" in "Lenny" in tutto il documento:
`sed -i 's/Kenny/Lenny/g' seinfeld-characters.txt`

# Guida alla Gestione degli Account Utente e delle Password in Linux

## 1. Comandi Fondamentali per la Gestione Utenti
In ambiente Linux, l'amministrazione di utenti e gruppi viene gestita tramite una serie di utility standard da riga di comando.

### Comandi Utente
* `useradd`: Crea un nuovo account utente.
* `userdel`: Elimina un utente esistente. L'opzione `-r` rimuove anche la directory home.
* `usermod`: Modifica un utente esistente (es. aggiunta a gruppi, cambio della shell).
* `id [utente]`: Visualizza l'identificativo utente (UID) e i gruppi di appartenenza.

### Comandi Gruppo
* `groupadd`: Crea un nuovo gruppo.
* `groupdel`: Elimina un gruppo esistente.
* `chgrp`: Cambia il gruppo proprietario di un file o di una directory.

### Gestione Password
* `passwd [utente]`: Assegna o modifica la password per un utente specifico.

---

## 2. File di Configurazione di Sistema
Le informazioni sugli utenti e i parametri di sicurezza sono memorizzati in tre file principali:

| File | Descrizione |
| :--- | :--- |
| `/etc/passwd` | Contiene i dati degli account (UID, GID, directory home, shell). |
| `/etc/group` | Contiene l'elenco dei gruppi e i relativi membri. |
| `/etc/shadow` | Memorizza le password criptate e i parametri di scadenza. |
| `/etc/login.defs` | Definisce le configurazioni predefinite per la creazione di nuovi utenti. |



---

## 3. Creazione Avanzata di un Utente
In contesti aziendali, gli utenti vengono spesso creati definendo più parametri contemporaneamente in un'unica stringa di comando:

```bash
useradd -g superheroes -s /bin/bash -c "Iron Man Character" -m -d /home/ironman ironman
```

**Dettaglio delle opzioni utilizzate:**
* `-g`: Specifica il gruppo primario.
* `-s`: Definisce l'ambiente della shell di login.
* `-c`: Aggiunge un commento o una descrizione dell'utente.
* `-m -d`: Forza la creazione della directory home nel percorso specificato.

---

## 4. Sicurezza e Scadenza Password (`chage`)
Il comando `chage` (Change Age) permette di applicare policy di sicurezza sulle password su base individuale.

### Opzioni principali di `chage`
* `-m`: **Giorni minimi** tra una modifica della password e l'altra.
* `-M`: **Giorni massimi** di validità della password prima del cambio obbligatorio.
* `-W`: **Giorni di preavviso** (warning) prima della scadenza.
* `-I`: **Giorni di inattività** dopo i quali l'account viene disabilitato se la password è scaduta.
* `-E`: **Data di scadenza** dell'account (espressa in formato assoluto).

### Esempio di applicazione policy
Per forzare un utente a cambiare password ogni 90 giorni, con 10 giorni di preavviso e un intervallo minimo di 5 giorni tra i cambi:

```bash
chage -m 5 -M 90 -W 10 -I 3 nomeutente
```

---

## 5. Valori Predefiniti di Sistema
Per applicare automaticamente queste policy a tutti i futuri utenti, è necessario modificare il file `/etc/login.defs`. Questo file controlla:
* **Intervalli UID/GID:** Valori iniziali per i nuovi utenti (es. a partire da 1000).
* **Invecchiamento Password:** Valori predefiniti per `PASS_MAX_DAYS` e `PASS_MIN_DAYS`.
* **Criptazione:** Metodo di hashing predefinito (solitamente `SHA512`).
* **UMASK:** Permessi predefiniti per i file creati dai nuovi utenti.

> **Nota:** Le modifiche a `/etc/login.defs` non hanno effetto sugli utenti già esistenti, ma solo su quelli creati successivamente.

### Gestione degli Utenti e Privilegi Sudo in Linux

Dopo aver appreso la gestione degli account utente, è necessario comprendere come effettuare il passaggio tra utenti e come concedere l'accesso **sudo**.

#### Definizioni e Comandi Principali

* **Sudo:** È un comando che permette a un utente ordinario di eseguire comandi con privilegi di livello **root**.
* **SU (Switch User):** Il comando `su - [nome_utente]` consente di passare da un account a un altro. Viene richiesta la password dell'utente di destinazione. Se eseguito da root, non è richiesta alcuna password.
* **Visudo:** È l'utility utilizzata per modificare il file di configurazione `/etc/sudoers`. Questo file determina quali utenti o gruppi possono eseguire comandi con privilegi elevati.
* **Directory /etc:** In ambiente Linux, questa directory ospita tutti i file di configurazione del sistema.

#### Accesso Remoto e Identificazione IP

Per operare su una macchina Linux remota, è possibile utilizzare un terminale come **PuTTY**.
1. Identificare l'indirizzo IP tramite il comando `ifconfig`.
2. Individuare l'interfaccia primaria (es. `eth0` o simile) per la comunicazione esterna.
3. Inserire l'indirizzo IP in PuTTY e autenticarsi con le proprie credenziali.

#### Gestione dei Privilegi

Alcuni comandi (come `dmidecode` o `fdisk`) restituiscono l'errore "Permission denied" se eseguiti da un utente standard. Per risolvere senza loggarsi permanentemente come root, si utilizza il gruppo **wheel**.

1.  **Modifica del file sudoers:** Tramite il comando `visudo`, si verifica la presenza del gruppo `%wheel`, che solitamente è configurato di default per eseguire tutti i comandi.
2.  **Aggiunta di un utente al gruppo wheel:** Per elevare i privilegi di un utente (es. *iafzal*), si utilizza il comando:
    `usermod -aG wheel [nome_utente]`
3.  **Verifica:** È possibile verificare l'appartenenza al gruppo consultando il file `/etc/group` con il comando `grep wheel /etc/group`.

#### Esecuzione dei Comandi con Sudo

Una volta aggiunto al gruppo appropriato, l'utente può eseguire comandi privilegiati anteponendo la parola `sudo`:
* `sudo dmidecode`
* `sudo fdisk -l`

Al primo utilizzo nella sessione, il sistema richiederà la password dell'utente per confermare l'identità. Questo metodo permette di mantenere la sicurezza del sistema senza condividere direttamente la password dell'account root.

### Monitoraggio degli Utenti in Linux

Il monitoraggio degli utenti è una delle responsabilità fondamentali per un amministratore di sistema. È necessario verificare chi è connesso e quali attività vengono svolte per garantire la sicurezza e le prestazioni del sistema.

#### Comandi Principali per il Monitoraggio

Esistono diversi comandi per ottenere informazioni sugli utenti connessi e sullo storico degli accessi:

* **who**: Mostra l'elenco degli utenti attualmente autenticati nel sistema. Fornisce dettagli sul nome utente, l'ID del terminale (es. `pts/0`) e l'orario di accesso. Se si aprono più terminali o sessioni (ad esempio tramite PuTTY), il comando elencherà ogni sessione attiva.
* **w**: Simile al comando `who`, ma più dettagliato. Oltre alle informazioni di login, mostra il tempo di inattività (idle time) e i processi che ogni utente sta eseguendo in quel momento.
* **last**: Visualizza lo storico completo di tutti i login e logout effettuati dall'attivazione del sistema. È utile per diagnosticare riavvii imprevisti o accessi sospetti, mostrando anche l'indirizzo IP di provenienza.
    * *Suggerimento*: Per leggere l'output una pagina alla volta, si utilizza il comando in pipe: `last | more`.
    * *Filtro avanzato*: Per estrarre solo i nomi utente unici che hanno effettuato l'accesso, è possibile combinare i comandi: `last | awk '{print $1}' | sort | uniq`.
* **id**: Fornisce informazioni sull'identità dell'utente. 
    * Eseguito senza argomenti (`id`), mostra il proprio UID (User ID), GID (Group ID) e i gruppi di appartenenza. 
    * Eseguito con un nome utente (`id spiderman`), mostra i dettagli specifici di quell'account.
* **finger**: Un programma potente che traccia l'origine dell'utente e i protocolli utilizzati. Solitamente non è incluso di default nelle distribuzioni standard e deve essere installato separatamente.

#### Pratica e Approfondimento

Per una gestione efficace, si raccomanda di testare questi comandi con diverse opzioni. La documentazione completa di ogni comando è consultabile tramite il manuale di sistema, utilizzando la sintassi:
`man [comando]` (es. `man who`).

L'uso costante di questi strumenti permette di intervenire prontamente in caso di carichi di sistema elevati o anomalie di sicurezza.

### Comunicazione con gli Utenti in Linux

In un ambiente Linux, è fondamentale che gli amministratori di sistema possano comunicare con gli utenti collegati, specialmente quando processi o applicazioni (come database o script) influiscono sulle prestazioni del sistema o quando è necessaria una manutenzione.

#### Comandi per l'Identificazione e la Comunicazione

* **users**: Mostra l'elenco dei nomi utente attualmente connessi al sistema. È il primo passo per identificare chi è attivo prima di inviare messaggi.
* **wall (Write All)**: Questo comando permette di trasmettere un messaggio in **broadcast** a tutti gli utenti collegati contemporaneamente.
    * *Utilizzo*: Si digita `wall`, si preme Invio, si inserisce il testo del messaggio e si conclude con la combinazione `Control + D` per inviarlo.
    * *Scopo*: Viene utilizzato principalmente per avvisi urgenti, come lo spegnimento imminente del sistema o l'avvio di una manutenzione programmata.
* **write**: A differenza di `wall`, questo comando è destinato a un **singolo utente specifico**.
    * *Utilizzo*: La sintassi è `write [nome_utente]`. Il messaggio apparirà direttamente sul terminale dell'utente destinatario, indicando anche la provenienza del mittente.
    * *Scopo*: È ideale per comunicazioni mirate, ad esempio per chiedere a un utente di interrompere un'applicazione specifica che sta consumando troppe risorse.

#### Esempio Pratico di Interazione

Per testare queste funzionalità, è possibile aprire più sessioni (ad esempio tramite PuTTY con diversi account utente):

1.  **Identificazione**: Eseguendo `users`, si ottiene la lista degli account attivi (es. `root`, `iafzal`, `spider`).
2.  **Invio Globale**: Con `wall`, l'amministratore avvisa tutti di salvare il lavoro: *"Il sistema verrà riavviato tra 5 minuti"*. Il messaggio appare istantaneamente su tutti i terminali.
3.  **Invio Mirato**: Con `write spider`, l'amministratore può avviare una conversazione privata con l'utente "spider". Il destinatario può rispondere a sua volta utilizzando `write [mittente]`.
4.  **Chiusura**: In entrambi i casi, la sessione di scrittura si termina con `Control + D`.

Questi strumenti sono essenziali per chi possiede privilegi di **root**, in quanto consentono di gestire in modo coordinato le attività di manutenzione senza interrompere bruscamente il lavoro degli utenti senza preavviso.

### Autenticazione degli Account in Linux: Locale vs Centralizzata

In ambiente Linux, la gestione delle identità può avvenire in due modalità principali: locale o centralizzata. Comprendere la differenza è fondamentale quando si passa dalla gestione di un singolo computer a quella di infrastrutture aziendali complesse.

#### 1. Account Locali
Gli account locali vengono creati direttamente sulla singola macchina (ad esempio tramite il comando `useradd`). Questo metodo è semplice e immediato, ma presenta limiti di scalabilità:
* **Inefficienza su larga scala:** Se un'azienda ha migliaia di server, non è pratico accedere a ogni singola macchina per creare o modificare un utente.
* **Difficoltà di gestione:** Cambiare una password o revocare un accesso richiederebbe un intervento manuale su ogni server dell'infrastruttura.

#### 2. Autenticazione Centralizzata (Directory Services)
Per gestire migliaia di utenti su molteplici server, si utilizzano i **servizi di directory**. In questo scenario:
* **Database Unico:** Tutti gli account risiedono in un database centrale su un server dedicato.
* **Processo di autenticazione:** Quando un utente tenta di accedere a un "client" (il server Linux), quest'ultimo interroga il server di directory per verificare l'esistenza dell'account e la correttezza delle credenziali.
* **Risposta del server:** Se l'autenticazione ha successo, il server di directory invia una conferma al client, permettendo l'accesso all'utente.

#### 3. Protocolli e Servizi: Chiarimenti su LDAP e Active Directory
Esiste spesso confusione tra i termini LDAP e Active Directory. È importante distinguere tra il servizio e il protocollo utilizzato:

* **Active Directory (AD):** È un prodotto di Microsoft, nativo per Windows e ampiamente utilizzato in ambito corporate per gestire utenti e risorse.
* **LDAP (Lightweight Directory Access Protocol):** Contrariamente a quanto spesso si crede, **LDAP non è un database o un server**, ma un **protocollo**. 
    * È il linguaggio standard utilizzato da Linux, Windows e macOS per comunicare con un servizio di directory.
    * Dire "usare LDAP per Linux" significa in realtà utilizzare il protocollo LDAP per interrogare un server di directory (che potrebbe essere OpenLDAP, Active Directory o altri).

#### Conclusione
Mentre in ambiente Windows l'uso di Active Directory è lo standard nativo, in Linux la scelta del server di directory può variare, ma il protocollo di comunicazione rimane quasi sempre LDAP. La gestione centralizzata è l'unico modo efficiente per amministrare l'accesso degli utenti in reti di grandi dimensioni.

### Confronto tra Servizi di Directory e Protocolli di Autenticazione

La gestione degli utenti in reti aziendali richiede l'utilizzo di servizi di directory centralizzati. Spesso si genera confusione tra prodotti commerciali, soluzioni open source e i protocolli necessari per la comunicazione. Di seguito viene presentata un'analisi delle principali soluzioni disponibili.

#### 1. Microsoft Active Directory (AD)
È il servizio di directory proprietario di Microsoft. È la soluzione standard per la gestione di migliaia di computer in ambiente Windows. Sebbene nasca per l'ecosistema Microsoft, la sua importanza risiede nella capacità di centralizzare l'autenticazione in infrastrutture complesse.

#### 2. Red Hat Identity Manager (IDM)
Red Hat ha sviluppato **Identity Manager (IDM)** per rispondere alle esigenze delle aziende che gestiscono grandi parchi macchine Linux (Enterprise Linux). È particolarmente utile per le organizzazioni con numerosi amministratori di sistema che necessitano di una gestione degli accessi simile a quella offerta da Active Directory, ma ottimizzata per Linux.

#### 3. WinBIND
**WinBIND** non è un servizio di directory, ma un componente di **Samba**. Funziona come un "traduttore" o messaggero che permette ai client Linux di autenticarsi direttamente presso un server **Microsoft Active Directory**. È la soluzione ideale quando si desidera utilizzare gli utenti Windows esistenti per accedere alle macchine Linux.

#### 4. OpenLDAP
Spesso confuso con il protocollo LDAP, **OpenLDAP** è una specifica implementazione open source di un server di directory.
* È ampiamente utilizzato in ambienti Linux e Unix.
* Essendo gratuito e altamente configurabile, è lo strumento standard per testare o implementare directory in ambienti che non richiedono soluzioni proprietarie come IDM di Red Hat.

#### 5. Altre Soluzioni Professionali
* **IBM Directory Services:** Una soluzione proprietaria e robusta offerta da IBM per il mercato enterprise.
* **JumpCloud:** Un servizio moderno che propone la "Directory-as-a-Service" (DaaS), operando interamente in cloud.

---

### Chiarimento Cruciale: Il Protocollo LDAP
È fondamentale distinguere tra un servizio (il software che memorizza i dati) e un protocollo (il linguaggio usato per parlare con il software).

* **LDAP (Lightweight Directory Access Protocol):** Non è un servizio di directory, ma il **protocollo di comunicazione**.
* Sia Active Directory che OpenLDAP o IDM utilizzano il protocollo LDAP per scambiare informazioni di autenticazione tra il client e il server.

### Tabella Riassuntiva

| Soluzione | Tipo | Sistema Operativo Primario |
| :--- | :--- | :--- |
| **Active Directory** | Servizio (Proprietario) | Windows |
| **Identity Manager (IDM)** | Servizio (Enterprise) | Linux (Red Hat) |
| **OpenLDAP** | Servizio (Open Source) | Linux / Unix |
| **WinBIND** | Componente di integrazione | Linux verso Windows |
| **LDAP** | **Protocollo di rete** | Universale |

### Comandi di Utilità di Sistema in Linux

In ambiente Linux, la gestione di informazioni basilari come l'orario, il calendario o le statistiche di sistema avviene tramite comandi di utilità eseguiti nel terminale. Sebbene l'interfaccia grafica (GUI) offra strumenti visuali, l'amministrazione professionale avviene prevalentemente tramite client testuali come PuTTY, dove l'uso dei comandi è indispensabile.

#### 1. Informazioni Temporali e di Sistema
* **date**: Mostra la data e l'ora corrente del sistema, inclusi i secondi e il fuso orario. È spesso utilizzato all'interno di script per automatizzare operazioni basate sul tempo.
* **uptime**: Fornisce dati sulla durata dell'attività del sistema dal momento dell'accensione, il numero di utenti attualmente connessi e il carico medio del sistema (load average).
* **hostname**: Visualizza il nome identificativo della macchina. È una buona norma eseguirlo all'accesso per assicurarsi di operare sul server corretto ed evitare modifiche critiche su macchine errate.
* **uname**: Identifica il sistema operativo. L'opzione `uname -a` fornisce dettagli completi come la versione del kernel, l'architettura del processore e la data di build.

#### 2. Localizzazione dei Comandi
* **which**: Permette di individuare il percorso assoluto di un comando eseguibile.
    * *Esempio*: `which pwd` restituirà solitamente `/usr/bin/pwd`.
    * Ogni comando in Linux è un file memorizzato nel filesystem (spesso in `/usr/bin`). È possibile verificarne i permessi e la proprietà con `ls -l`.

#### 3. Strumenti di Calcolo e Calendario
* **cal**: Visualizza il calendario del mese corrente.
    * Per visualizzare un mese specifico di un anno passato o futuro: `cal [mese] [anno]` (es. `cal 9 1977`).
    * Per visualizzare l'intero calendario di un anno: `cal [anno]` (es. `cal 2016`).
* **bc (Binary Calculator)**: Avvia una calcolatrice testuale interattiva.
    * Una volta avviata, permette di eseguire operazioni matematiche (es. `2+2` o `256 * 321`).
    * Per uscire dal programma si utilizza il comando `quit`.

#### 4. Analisi dei Comandi Disponibili
Per determinare quanti comandi eseguibili sono presenti in una directory di sistema, si può combinare l'elenco dei file con il conteggio delle righe:
`ls -l /usr/bin | wc -l`

---

> **Nota**: Per approfondire le opzioni di ogni comando, si raccomanda l'uso del manuale di sistema tramite il comando `man [nome_comando]`.

### Processi e Job in Linux: Concetti e Comandi

Per amministrare correttamente un ambiente Linux, è necessario distinguere chiaramente la terminologia relativa all'esecuzione dei programmi e alla gestione delle risorse.

#### Terminologia Fondamentale

* **Applicazione o Servizio**: È un programma eseguito sul computer (es. NTP, NFS o il server web Apache). In Linux, questi termini sono spesso usati come sinonimi per indicare software che fornisce funzionalità specifiche.
* **Script**: Un insieme di istruzioni scritte in un file e pacchettizzate per essere eseguite. Anche i comandi lanciati quotidianamente nel terminale possono essere considerati script. Molte applicazioni, come Apache, vengono avviate tramite script che ne gestiscono l'esecuzione in background.
* **Processo**: Rappresenta l'istanza di un'applicazione in esecuzione. Ogni volta che si avvia un programma, il sistema genera uno o più processi, ciascuno identificato da un **PID (Process ID)** univoco.
* **Daemon (Demone)**: Un tipo particolare di processo che rimane costantemente attivo in background. Non si interrompe e rimane in ascolto di traffico in entrata o in uscita (es. richieste di rete).
* **Thread**: Un processo può avere più thread associati. Se un'applicazione come NFS riceve connessioni da più computer remoti, genererà thread separati per gestire ogni singola connessione simultaneamente.
* **Job**: Un'attività pianificata da uno schedulatore. Può essere considerato come un "ordine di lavoro" per l'esecuzione di applicazioni o servizi a intervalli prestabiliti.

#### Comandi di Gestione

* **systemctl**: È il comando moderno utilizzato nelle distribuzioni recenti (come Red Hat e CentOS) per gestire i servizi. Ha sostituito il vecchio comando `service`. Permette di avviare, arrestare o verificare lo stato delle applicazioni (es. `systemctl stop httpd`).
* **ps**: Consente di visualizzare i processi attualmente attivi nel sistema. Attraverso varie opzioni, permette di filtrare e individuare esattamente il processo desiderato.
* **top**: Fornisce una vista dinamica e in tempo reale dei processi in esecuzione. Mostra informazioni critiche come l'utilizzo della CPU, l'occupazione della memoria e il carico di sistema, ordinando i processi in base alle risorse consumate.
* **kill**: Utilizzato per terminare forzatamente un processo. L'interruzione può avvenire specificando il nome del processo o, più comunemente, il suo PID.
* **crontab**: Strumento fondamentale per la pianificazione (scheduling) di processi e script. Utilizzando l'opzione `-e`, è possibile modificare il file di configurazione per impostare l'esecuzione automatica di "job" a orari o giorni specifici.
* **at**: Simile a crontab, ma utilizzato per la pianificazione di attività **una tantum**. È ideale per eseguire processi in modalità ad hoc che non richiedono una ripetizione ciclica.

### Gestione dei Servizi con il Comando systemctl

Il comando **systemctl** (o *system control*) è lo strumento principale per la gestione dei servizi e delle applicazioni in ambiente Linux. Nelle distribuzioni Red Hat e CentOS (versione 7 e successive), questo comando sostituisce il vecchio strumento `service`.

#### 1. Funzionalità Principali
A differenza degli ambienti Windows, dove le applicazioni si avviano tramite icone grafiche, in Linux (prevalentemente testuale) si utilizza `systemctl` per controllare i servizi del sistema e le applicazioni di terze parti. Le operazioni fondamentali sono:

* **start**: Avvia un servizio.
* **stop**: Arresta un servizio in esecuzione.
* **status**: Visualizza lo stato dettagliato di un servizio (se è caricato, attivo, il suo PID e i log recenti).
* **restart / reload**: Riavvia il servizio o ricarica la configurazione dopo aver apportato modifiche ai file di impostazione.

#### 2. Gestione all'Avvio (Boot)
* **enable**: Configura il servizio per l'avvio automatico all'accensione della macchina.
* **disable**: Impedisce al servizio di avviarsi automaticamente al boot.

#### 3. Visualizzazione delle "Unit"
All'interno della gestione `systemctl`, ogni risorsa o servizio viene definito **Unit**.
* `systemctl list-units`: Elenca tutte le unità caricate e attive.
* `systemctl list-units --all`: Mostra tutte le unità, comprese quelle inattive o "morte".

L'output del comando si divide in colonne:
* **UNIT**: Il nome del servizio.
* **LOAD**: Indica se il file di configurazione è stato caricato correttamente in memoria.
* **ACTIVE**: Stato riassuntivo (attivo o inattivo).
* **SUB**: Stato di basso livello con dettagli specifici sull'esecuzione.
* **DESCRIPTION**: Breve descrizione testuale della funzione dell'unità.

#### 4. Controllo del Sistema e Integrazioni
Oltre ai singoli servizi, `systemctl` può gestire l'intero stato della macchina:
* `systemctl poweroff`: Spegne completamente il sistema.
* `systemctl reboot`: Riavvia la macchina.

Per integrare un'applicazione di terze parti sotto la gestione di `systemctl`, è necessario creare un file con estensione `.service` nella directory `/etc/systemd/system/`.

---

> **Nota**: Per eseguire la maggior parte di questi comandi è necessario disporre dei privilegi di **root** o utilizzare **sudo**.

### Monitoraggio dei Processi con il Comando ps

Il comando **ps** (*Process Status*) è lo strumento fondamentale in Linux per visualizzare lo stato dei processi attualmente in esecuzione nel sistema. A differenza della gestione dei servizi (systemctl), `ps` fornisce un'istantanea dettagliata di ciò che il kernel sta gestendo in un determinato istante.

#### 1. Utilizzo Base e Colonne di Output
L'esecuzione del comando `ps` senza opzioni mostra i processi associati alla sessione (shell) corrente dell'utente. L'output standard si compone di quattro colonne:

* **PID (Process ID)**: Identificativo univoco del processo.
* **TTY**: Indica il tipo di terminale a cui l'utente è connesso (es. la console o un terminale virtuale).
* **TIME**: Indica il tempo totale di CPU (minuti e secondi) utilizzato dal processo. Valori pari a `00:00:00` indicano che il processo non ha ancora richiesto elaborazioni significative alla CPU.
* **CMD**: Il nome del comando o dell'eseguibile che ha generato il processo.

#### 2. Varianti e Opzioni Comuni
Il comando `ps` offre diverse modalità di visualizzazione a seconda delle necessità:

* **ps -e**: Mostra tutti i processi attivi nell'intero sistema Linux.
* **ps aux**: Visualizza i processi in formato BSD (Berkeley Systems Description). Questa variante aggiunge colonne cruciali come l'utente proprietario, la percentuale di utilizzo di CPU e memoria, e l'orario di avvio.
* **ps -ef**: Una delle opzioni più utilizzate dagli amministratori di sistema. Fornisce un elenco completo in formato esteso, includendo il **PPID** (Parent Process ID), ovvero l'ID del processo padre.
* **ps -u [nome_utente]**: Filtra i processi visualizzando solo quelli avviati da uno specifico utente (es. `ps -u root`).

#### 3. Integrazione con altri strumenti
È possibile combinare `ps` con il comando `grep` per isolare un processo specifico. 
Se si conosce il PID (ottenuto ad esempio tramite `systemctl status`), è possibile verificare i dettagli del processo corrispondente:
* *Esempio*: `ps -ef | grep 750` oppure `ps -ef | grep firewalld`.

#### 4. Confronto con Ambiente Windows
In ambito Windows, l'equivalente del comando `ps` è il **Gestore Attività** (Task Manager). Entrambi gli strumenti permettono di monitorare le applicazioni, i servizi in background, i relativi ID di processo e l'utilizzo delle risorse hardware.

---

> **Nota**: Per l'esecuzione del comando `ps` non è necessario disporre dei privilegi di root, a meno che non si vogliano visualizzare dettagli riservati di processi appartenenti ad altri utenti.

### Monitoraggio in tempo reale: Il comando top

Il comando **top** è uno degli strumenti più utilizzati dagli amministratori di sistema e dagli ingegneri per il monitoraggio dei processi in ambiente Linux. Fornisce una vista dinamica e in tempo reale dello stato del sistema, elencando i processi o i thread attualmente gestiti dal kernel.

#### 1. Modalità Interattiva
All'esecuzione, il comando entra in una **modalità interattiva**. Per terminare la visualizzazione e tornare al prompt dei comandi, è necessario premere il tasto **Q** sulla tastiera.

#### 2. Interpretazione dell'Output (Righe di riepilogo)
Le prime cinque righe forniscono una panoramica globale del sistema:
* **Uptime e Load Average**: Indica da quanto tempo il sistema è attivo, il numero di utenti connessi e il carico medio del sistema (calcolato su 1, 5 e 15 minuti).
* **Tasks**: Mostra il numero totale di processi e il loro stato (in esecuzione, in attesa/sleeping, arrestati o **zombie**). Un processo *zombie* è un processo figlio il cui processo padre è stato terminato.
* **CPU**: Dettagli sull'utilizzo della potenza di calcolo.
* **Memoria Fisica (RAM) e Swap**: Statistiche sull'uso della memoria reale e della memoria virtuale (totale, libera, utilizzata e in cache).

#### 3. Descrizione delle Colonne
Sotto il riepilogo, i processi sono elencati in colonne:
* **PID**: Identificativo univoco del processo.
* **USER**: L'utente che ha avviato il processo.
* **PR / NI**: Indicano rispettivamente la priorità di scheduling e il valore di "nice". Un valore di *nice* negativo indica una priorità più alta.
* **VIRT / RES / SHR**: Gestione della memoria (Virtuale, Residente/RAM fisica, Condivisa).
* **S**: Stato del processo (rappresentato da una singola lettera).
* **%CPU / %MEM**: Percentuale di risorse di calcolo e memoria fisica consumate dal singolo processo.
* **TIME+**: Tempo totale di CPU utilizzato, con precisione al centesimo di secondo.
* **COMMAND**: Il nome del comando o il percorso del processo eseguito.

#### 4. Comandi Interattivi e Opzioni
Mentre `top` è in esecuzione, è possibile utilizzare i seguenti tasti:
* **C**: Mostra il percorso assoluto (path) completo dei comandi in esecuzione.
* **K**: Permette di terminare (*kill*) un processo inserendo il relativo PID. Nota: per terminare processi non propri è necessario aver avviato `top` come root.
* **M (Shift+M)**: Ordina l'elenco dei processi in base all'utilizzo della memoria.
* **P (Shift+P)**: Ordina l'elenco in base all'utilizzo della CPU.

È inoltre possibile filtrare l'avvio del comando per un singolo utente tramite l'opzione `-u`:
`top -u [nome_utente]` (es. `top -u root`).

---

> **Nota**: Per impostazione predefinita, `top` aggiorna i dati ogni 3 secondi. Questo intervallo è personalizzabile all'interno delle impostazioni del comando.

### Terminazione dei Processi: Il comando kill

Il comando **kill** viene utilizzato per terminare manualmente i processi nel sistema Linux. Contrariamente a quanto suggerisce il nome, il comando invia un **segnale** a un processo o a un gruppo di processi, chiedendo loro di agire in un determinato modo.

#### 1. Utilizzo e Sintassi
Sebbene i servizi possano essere gestiti tramite `systemctl stop`, talvolta alcuni processi associati non si interrompono correttamente. In questi casi, il comando `kill` permette di intervenire direttamente sul **PID** (Process ID).

La sintassi base è:
`kill [opzione/segnale] [PID]`

* Il **PID** può essere individuato tramite i comandi `top` o `ps -ef`.
* Se non viene specificata alcuna opzione, il comando invia il segnale predefinito **SIGTERM** (15).

#### 2. Segnali comuni
È possibile visualizzare l'elenco completo dei 64 segnali disponibili eseguendo il comando `kill -l`. I più utilizzati in ambito amministrativo sono:

| Segnale | Nome | Descrizione |
| :--- | :--- | :--- |
| **-1** | SIGHUP | Ricarica o riavvia il processo. |
| **-2** | SIGINT | Interruzione da tastiera (equivalente a `Ctrl+C`). |
| **-9** | SIGKILL | Terminazione forzata immediata (il processo non può ignorarlo). |
| **-15** | SIGTERM | Terminazione garbata (graceful), permette al processo di chiudersi correttamente. |

#### 3. Esempi Pratici
* **Terminazione standard**: `kill 750` (invia un SIGTERM al processo 750).
* **Riavvio/Reload**: `kill -1 6290` (richiede al processo di ricaricare la configurazione).
* **Terminazione forzata**: `kill -9 6576` (interrompe il processo immediatamente, utile per processi bloccati o "appesi").

#### 4. Comandi Correlati
Oltre a `kill`, esistono varianti per semplificare la gestione:
* **pkill [nome_processo]**: Permette di terminare un processo utilizzando il suo nome invece del PID (es. `pkill firewalld`).
* **killall [nome_processo]**: Termina tutte le istanze di un processo specifico e i relativi processi figli.

#### 5. Confronto con Windows
In ambiente Windows, l'equivalente funzionale del comando `kill` si trova nel **Gestore Attività** (Task Manager). L'azione "Termina attività" (End Task) su un'applicazione o su un servizio specifico corrisponde all'invio di un segnale di terminazione in Linux.

---

> **Nota**: Per terminare processi appartenenti al sistema o ad altri utenti, è necessario disporre dei privilegi di **root** (es. tramite `sudo` o `su -`).

### Pianificazione dei compiti: Il comando crontab

Il comando **crontab** (*cron table*) viene utilizzato per pianificare l'esecuzione automatica di compiti e script nel sistema Linux. Questa funzionalità è essenziale per gli amministratori di sistema che devono eseguire comandi ripetitivi a intervalli specifici senza dover intervenire manualmente.

#### 1. Comandi di Gestione
La gestione della tabella dei processi pianificati avviene tramite le seguenti opzioni:

* **crontab -e**: Apre l'editor per modificare o aggiungere nuove voci alla tabella.
* **crontab -l**: Elenca tutte le pianificazioni attualmente attive per l'utente.
* **crontab -r**: Rimuove l'intera tabella crontab dell'utente.

Il servizio che gestisce l'esecuzione di questi compiti in background è il demone **crond**. È possibile verificarne lo stato con il comando:
`systemctl status crond`

#### 2. Formato della Tabella Crontab
Ogni voce nel crontab deve seguire un formato rigoroso composto da sei colonne. Un errore nella formattazione impedirà l'esecuzione del comando.

| Colonna | Valore | Descrizione |
| :--- | :--- | :--- |
| **1** | 0 - 59 | Minuto |
| **2** | 0 - 23 | Ora |
| **3** | 1 - 31 | Giorno del mese |
| **4** | 1 - 12 | Mese |
| **5** | 0 - 6 | Giorno della settimana (0 = Domenica) |
| **6** | Comando | Percorso dello script o comando da eseguire |

*Nota: L'uso dell'asterisco (`*`) in una colonna indica "ogni" (es. ogni minuto, ogni giorno).*

#### 3. Esempio Pratico
Per scrivere una stringa di testo in un file ogni giorno alle ore 16:21 del mese di ottobre, la riga da inserire tramite `crontab -e` sarebbe:

`21 16 * 10 * echo "Messaggio di test" >> /home/utente/crontab-entry`

#### 4. Considerazioni Importanti
* **Percorsi**: Se non viene specificato un percorso assoluto, il sistema utilizzerà la *home directory* dell'utente.
* **Stato del Servizio**: Se il servizio `crond` è arrestato, le voci presenti nel crontab non verranno eseguite.
* **Privilegi**: Ogni utente ha la propria tabella crontab; per compiti di sistema è necessario operare come root.

### Pianificazione di compiti singoli: Il comando at

Il comando **at** è uno strumento di pianificazione simile a crontab, ma con una differenza fondamentale: permette di eseguire un compito **una sola volta** nel futuro. Mentre crontab è progettato per compiti ripetitivi, `at` è ideale per operazioni occasionali.

#### 1. Funzionamento e Modalità Interattiva
A differenza di `crontab -e`, che apre un editor di testo, l'esecuzione di `at` seguito dall'orario avvia una **modalità interattiva** direttamente nel terminale:

1. Si digita il comando (es. `at 17:05 PM`).
2. Si inseriscono i comandi da eseguire.
3. Si preme **Ctrl+D** per salvare e uscire dalla modalità interattiva.

#### 2. Comandi di Gestione
La gestione dei lavori pianificati avviene tramite i seguenti strumenti:

* **atq**: Elenca tutti i lavori (job) attualmente in coda e non ancora eseguiti. Ogni lavoro è contrassegnato da un numero identificativo univoco.
* **atrm [numero_job]**: Rimuove un lavoro specifico dalla coda utilizzando il suo numero identificativo.
* **atd**: È il demone di sistema che gestisce l'esecuzione dei compiti. Lo stato del servizio può essere monitorato con:
  `systemctl status atd`

#### 3. Sintassi e Flessibilità Temporale
Il comando `at` si distingue per la sua capacità di interpretare espressioni temporali simili al linguaggio naturale. Di seguito alcuni esempi di formati supportati:

* **Orario specifico**: `at 2:45 AM 101621` (esegue alle 02:45 del 16 ottobre 2021).
* **Intervalli relativi**: `at 4:00 PM + 4 days` (esegue alle 16:00 tra quattro giorni).
* **Basato sull'ora attuale**: `at now + 5 hours` (esegue tra cinque ore esatte).
* **Giorni della settimana**: `at 8:00 AM Sunday` (esegue alle 08:00 della prossima domenica).
* **Scadenze mensili**: `at 10:00 AM next month` (esegue alle 10:00 dello stesso giorno del mese successivo).

#### 4. Esempio Pratico
Per creare un file di testo alle 17:05:
1. Digitare `at 5:05 PM`.
2. All'interno del prompt interattivo, inserire: `echo "Esempio comando AT" > at-entry.txt`.
3. Premere **Ctrl+D**.
4. Verificare la presenza del compito in coda con `atq`.

---

> **Nota**: Se il servizio `atd` è inattivo, è possibile aggiungere nuovi lavori alla coda, ma questi non verranno eseguiti finché il servizio non sarà ripristinato.

### Gestione avanzata dei Cron Job: Directory predefinite

Oltre alla pianificazione manuale tramite il comando `crontab -e`, il sistema operativo Linux offre una struttura di directory predefinite per l'esecuzione automatica di script con cadenze temporali standard. Questo metodo evita la configurazione manuale della sintassi cron per ogni singolo file.

#### 1. Directory di sistema
Esistono quattro directory principali situate in `/etc/` dove è possibile inserire script eseguibili:

* **/etc/cron.hourly/**: Per compiti da eseguire ogni ora.
* **/etc/cron.daily/**: Per compiti da eseguire una volta al giorno.
* **/etc/cron.weekly/**: Per compiti da eseguire una volta alla settimana.
* **/etc/cron.monthly/**: Per compiti da eseguire una volta al mese.

Per rendere operativo un processo, è sufficiente spostare lo script desiderato all'interno della directory corrispondente.

#### 2. Configurazione dei tempi di esecuzione (Anacron)
Il momento esatto in cui i lavori giornalieri, settimanali e mensili vengono eseguiti è definito nel file di configurazione:
`/etc/anacrontab`

Attraverso il comando `cat /etc/anacrontab` è possibile visualizzare i parametri che determinano il periodo (in giorni) e il ritardo (in minuti) con cui i job vengono avviati. Questo sistema garantisce l'esecuzione dei compiti anche se il sistema era spento al momento dell'orario previsto.

#### 3. Pianificazione dei compiti orari
A differenza degli altri, l'intervallo per i compiti orari è definito in un file separato:
`/etc/cron.d/0hourly`

In questo file è specificato che gli script contenuti in `cron.hourly` vengano eseguiti al **minuto 01 di ogni ora** (es. 12:01, 13:01, 14:01).

#### 4. Riepilogo dei metodi di pianificazione
L'amministratore può scegliere tra due approcci:
1.  **Metodo Manuale (`crontab -e`)**: Ideale per pianificazioni personalizzate con orari specifici non standard.
2.  **Metodo per Directory (`/etc/cron.*`)**: Più rapido e organizzato; basta inserire lo script nella cartella corretta e lasciare che il sistema gestisca l'esecuzione in base alle impostazioni di default.

---

> **Nota**: Assicurarsi che ogni script inserito in queste directory abbia i permessi di esecuzione corretti per poter essere avviato correttamente dal demone cron.

### Gestione dei Processi in Linux

La gestione dei processi comprende tutte le attività relative al controllo del ciclo di vita di un programma in esecuzione: l'avvio, l'arresto, il monitoraggio e la regolazione della priorità o della visibilità (background/foreground).

#### 1. Esecuzione in Background e Foreground
Quando un comando viene eseguito in un terminale, solitamente occupa la sessione (foreground), impedendo l'inserimento di altri comandi fino al termine dell'operazione.

* **Spostamento in Background**:
    1.  Eseguire il comando.
    2.  Premere `Ctrl + Z` per sospendere il processo.
    3.  Digitare il comando `bg` per far riprendere l'esecuzione del processo in background.
* **Visualizzazione dei processi attivi**: Il comando `jobs` elenca i processi gestiti nella sessione corrente del terminale.
* **Ritorno in Foreground**: Il comando `fg` riporta un processo in esecuzione dal background al primo piano del terminale.



#### 2. Esecuzione Persistente con `nohup`
Normalmente, se si chiude un terminale o una sessione SSH (come PuTTY), tutti i processi associati vengono interrotti. Per evitarlo, si utilizza il comando `nohup` (*no hang up*).

* **Sintassi base**: `nohup [comando] &`
* **Redirezione completa**: Spesso i messaggi di output o errore vengono inviati a un file (`nohup.out`). Per eliminare questi messaggi e inviarli nel "buco nero" del sistema, si usa:
    `nohup [comando] > /dev/null 2>&1 &`
    * `/dev/null`: Destinazione per scartare l'output.
    * `2>&1`: Reindirizza gli errori (stderr) sullo stesso canale dell'output standard (stdout).

#### 3. Terminazione e Monitoraggio
* **Terminare per nome**: Oltre al comando `kill [PID]`, è possibile usare `pkill [nome_processo]` per terminare un'istanza senza cercarne l'ID numerico.
* **Monitoraggio**:
    * `top`: Fornisce una visualizzazione dinamica e in tempo reale dei processi che consumano più risorse.
    * `ps -ef`: Genera un'istantanea statica di tutti i processi in esecuzione nel sistema.

#### 4. Priorità dei Processi (`nice`)
Linux permette di assegnare una priorità di accesso alla CPU tramite il valore di "niceness", che varia da **-20** (massima priorità) a **19** (minima priorità).

* **Sintassi**: `nice -n [valore] [comando]`
* **Concetto**: Un processo con valore -20 è considerato "poco gentile" (meno *nice*) verso gli altri perché sottrae risorse alla CPU, mentre un valore 19 è estremamente "gentile" e attende che la CPU sia libera.

### Monitoraggio del Sistema in Linux

Il monitoraggio delle risorse è una competenza fondamentale per l'amministrazione di sistema e l'ingegneria dei sistemi Linux. Questi strumenti permettono di analizzare il comportamento del sistema e diagnosticare eventuali rallentamenti o guasti hardware.

#### 1. Comandi per Processi e Memoria
* **top**: Fornisce una panoramica dinamica in tempo reale delle prestazioni del sistema, inclusi l'utilizzo della CPU, la memoria, i processi attivi e gli utenti che li eseguono.
* **free**: Mostra la quantità totale di memoria fisica (RAM) e di swap libera e utilizzata nel sistema.
* **cat /proc/cpuinfo** e **/proc/meminfo**: File virtuali che contengono dettagli tecnici approfonditi rispettivamente sul processore e sulla memoria.



#### 2. Gestione del Disco e dello Spazio
* **df -h**: Visualizza l'utilizzo dello spazio su disco per tutti i file system montati. L'opzione `-h` (human-readable) converte i dati in formati comprensibili (GB, MB).
* **du**: Utilizzato per stimare lo spazio occupato da file o directory specifici, utile per individuare file eccessivamente grandi.

#### 3. Diagnostica Hardware e I/O
* **dmesg**: Visualizza i messaggi del buffer del kernel. È essenziale per diagnosticare errori del BIOS, guasti hardware, problemi alla scheda madre o leak di memoria durante l'avvio o l'esecuzione.
* **iostat**: Fornisce statistiche sull'input/output per i dispositivi di archiviazione e le partizioni.
    * `iostat 1`: Aggiorna le statistiche ogni secondo per un monitoraggio continuo delle velocità di lettura/scrittura.

#### 4. Rete e Connettività
Nelle versioni moderne di Linux (come CentOS 8 e successive), il comando tradizionale `netstat` è stato sostituito da strumenti più efficienti:
* **ip route**: Visualizza le tabelle di routing, il gateway predefinito e gli indirizzi IP assegnati alle interfacce.
* **ss**: (Socket Statistics) Fornisce informazioni dettagliate sulle connessioni socket attive, porte aperte e stati delle connessioni, superando in velocità il vecchio `netstat`.



---

**Suggerimento per l'amministratore**:
In caso di crash del sistema, il primo comando da eseguire è solitamente `df -h`. Se l'utilizzo del disco raggiunge il **100%**, molti servizi smetteranno di funzionare correttamente, richiedendo l'immediata liberazione di spazio.

### Monitoraggio dei Log in Linux

Il monitoraggio dei log rappresenta una delle attività più critiche per un amministratore di sistema. I log possono essere paragonati a una cartella clinica del sistema: registrano la cronologia degli eventi, gli errori passati, le modifiche hardware e le attività delle applicazioni, permettendo di diagnosticare problemi attuali o prevenire guasti futuri.

#### 1. Directory Principale dei Log
In quasi tutte le distribuzioni Linux, la directory predefinita per la memorizzazione dei file di log è:
`/var/log`

Sebbene alcune applicazioni possano essere configurate per scrivere altrove, la maggior parte dei servizi di sistema utilizza questa posizione centralizzata.

#### 2. Principali File di Log
Di seguito sono elencati i file di log più rilevanti per l'amministrazione del sistema:

| File di Log | Descrizione |
| :--- | :--- |
| **boot.log** | Registra tutti i messaggi generati durante l'avvio del sistema (avvio dei servizi, pulizia memoria, ecc.). |
| **messages** | Il log più importante. Raccoglie messaggi generici del sistema, inclusi eventi hardware, processi e applicazioni. |
| **secure** | Registra le attività di autenticazione, inclusi i login effettuati con successo, i tentativi falliti e l'uso di `sudo`. |
| **cron** | Memorizza le informazioni sulle attività pianificate tramite il demone crond. |
| **maillog** | Contiene i log relativi ai servizi di posta elettronica (es. Sendmail o Postfix). |
| **httpd/** | Directory che solitamente contiene i log di accesso e di errore del server web Apache. |

#### 3. Gestione dei Permessi
L'accesso ai file di log è generalmente limitato per motivi di sicurezza. Molti file (come `secure` o `messages`) sono di proprietà dell'utente **root** e non sono leggibili da utenti ordinari. Per visualizzarli, è necessario elevare i privilegi tramite il comando `su` o `sudo`.

#### 4. Strumenti per l'Analisi dei Log
Per esaminare i file di log in modo efficace, si utilizzano diversi comandi standard:

* **more / less**: Per leggere il contenuto di un log una pagina alla volta.
* **tail -f**: Utilizzato per monitorare un log in tempo reale. Il parametro `-f` (*follow*) mantiene il file aperto e visualizza immediatamente ogni nuova riga aggiunta (estremamente utile per testare i login in `secure`).
* **grep -i "error"**: Permette di filtrare i log alla ricerca di specifiche parole chiave (come "error" o "failed"), ignorando la differenza tra maiuscole e minuscole grazie all'opzione `-i`.
* **wc -l**: Utilizzato per contare il numero totale di righe presenti in un file di log, utile per valutare la verbosità del sistema.

### Comandi per la Manutenzione del Sistema Linux

La manutenzione del sistema comprende tutte le operazioni necessarie per gestire lo stato operativo della macchina, come il riavvio, lo spegnimento o il passaggio a modalità di esecuzione specifiche (es. modalità utente singolo). Queste azioni sono di esclusiva competenza dell'amministratore di sistema.

#### 1. Spegnimento e Riavvio tramite `shutdown`
Il comando `shutdown` è il metodo più sicuro per arrestare il sistema, poiché notifica gli utenti connessi e chiude correttamente i processi.
* **shutdown -r**: Riavvia il sistema (*reboot*).
* **shutdown -h**: Arresta il sistema (*halt* o *poweroff*).
* **shutdown -P**: Spegne fisicamente la macchina.

#### 2. Livelli di Esecuzione con `init`
Il comando `init` (legato a *systemd* nelle versioni moderne) gestisce i diversi "runlevel" o stati del sistema, identificati da numeri:
* **init 0**: Spegnimento immediato del sistema.
* **init 1**: Modalità utente singolo (manutenzione).
* **init 3**: Modalità multi-utente (interfaccia a riga di comando).
* **init 6**: Riavvio completo del sistema.

#### 3. Comandi Diretti: `reboot` e `halt`
* **reboot**: È il comando più immediato per riavviare la macchina. Chiude la sessione corrente e riavvia il sistema operativo.
* **halt**: Forza l'arresto immediato del sistema. A differenza di `shutdown`, `halt` può non attendere il termine corretto di tutti i processi in esecuzione, agendo in modo simile alla pressione prolungata del tasto di accensione fisico.

#### 4. Procedure di Sicurezza
Prima di eseguire un comando di manutenzione che interrompe l'attività del server, è fondamentale:
1.  **Verificare l'identità dell'host**: Utilizzare il comando `hostname` per assicurarsi di operare sulla macchina corretta e non su un server di produzione per errore.
2.  **Elevare i privilegi**: Quasi tutti i comandi di manutenzione richiedono i privilegi di **root**.
3.  **Controllare gli utenti connessi**: Verificare chi è collegato al sistema per evitare perdite di dati non salvati da parte di altri utenti.

---

> **Nota Operativa**: Durante un riavvio, le sessioni remote (come PuTTY o SSH) verranno interrotte. Sarà necessario attendere il completamento del ciclo di boot per ristabilire la connessione.

### Modifica del Hostname in Sistemi Linux

Il **hostname** è il nome identificativo assegnato a una macchina Linux durante l'installazione iniziale. È prassi comune doverlo modificare quando un server viene riconvertito, dismesso o destinato a un nuovo standard aziendale.

#### 1. Comandi e Procedure (Linux 7 e versioni successive)
Il metodo più rapido e moderno per modificare il nome dell'host è l'utilizzo dell'utility `hostnamectl`.

* **Comando di modifica**: `hostnamectl set-hostname [nuovo_hostname]`
* **Privilegi**: L'operazione richiede privilegi di **root** per essere eseguita correttamente.

Una volta eseguito il comando, il sistema aggiorna automaticamente il file di configurazione, ma il prompt del terminale continuerà a mostrare il vecchio nome finché la sessione della shell non viene riavviata o il sistema non viene riavviato.

#### 2. File di Configurazione
A seconda della versione della distribuzione Linux in uso, il hostname viene salvato in percorsi differenti:

* **Linux 7 e versioni attuali**: Il file di riferimento è `/etc/hostname`. È possibile modificare il nome anche editando direttamente questo file di testo.
* **Versioni precedenti (es. Linux 6)**: La configurazione risiedeva nel file `/etc/sysconfig/network`.

#### 3. Applicazione delle Modifiche
Affinché il sistema riconosca pienamente il nuovo hostname e lo visualizzi correttamente nel prompt del terminale, è necessario un riavvio.
* **Comandi per il riavvio**: È possibile utilizzare il comando `reboot` oppure `init 6`.



#### 4. Confronto con Ambiente Windows
A differenza di Linux, dove la modifica avviene tramite riga di comando o editing di file, in ambiente Windows la procedura standard è di tipo grafico (GUI):
1.  Accedere a "Questo PC".
2.  Fare clic con il tasto destro su "Proprietà".
3.  Selezionare "Rinomina questo PC".

### Identificazione delle Informazioni di Sistema in Linux

Ogni volta che si accede a una macchina Linux, è fondamentale identificare correttamente il sistema operativo in uso, la versione del kernel e le caratteristiche dell'hardware sottostante. Esistono diversi comandi per ottenere queste informazioni in modo rapido e preciso.

#### 1. Verifica della Distribuzione e della Versione
Per conoscere la versione specifica della distribuzione installata (nel caso di derivate Red Hat), si può visualizzare il contenuto del file di rilascio:
* **Comando**: `cat /etc/redhat-release`
* **Esempio di output**: Indica la versione esatta (es. CentOS Linux release 7.4.1708).

#### 2. Informazioni sul Kernel e l'Architettura
Il comando `uname` fornisce una panoramica del sistema operativo e del cuore del sistema (il kernel).
* **Comando**: `uname -a`
* **Dettagli forniti**: Nome del sistema, hostname, versione e data del kernel, architettura hardware (es. x86_64) e tipo di sistema operativo.

#### 3. Analisi dell'Hardware (DMI)
Per ottenere dettagli più profondi sull'hardware "sotto il cofano" (processore, memoria RAM, BIOS, produttore), si utilizza l'utility `dmidecode`.
* **Comando**: `dmidecode`
* **Requisiti**: Questo comando richiede privilegi di **root** (permesso negato per utenti ordinari).
* **Gestione output**: Poiché genera una quantità elevata di dati, è consigliabile utilizzarlo con una pipe: `dmidecode | more`.
* **Informazioni utili**: Nome del prodotto (es. VirtualBox), produttore (es. Oracle Corporation), numeri di serie e dettagli sulla scheda madre (base board).

#### 4. Verifica dell'Identità del Host
Uno dei passaggi più critici per un amministratore di sistema è la conferma della macchina su cui si sta operando per evitare errori fatali (come riavviare il server sbagliato).
* **Comando**: `hostname`
* **Utilizzo**: Fornisce il nome univoco della macchina all'interno della rete. È una procedura di sicurezza standard eseguire sempre questo comando prima di operazioni invasive come il riavvio (`reboot`).

### Architettura del Sistema: 32-bit vs 64-bit

L'architettura di un sistema dipende direttamente dalla CPU e dal chipset installati nel computer. Esistono principalmente due tipi di architetture: a **32-bit** e a **64-bit**.

#### 1. Differenze Principali
La distinzione fondamentale tra un processore a 32-bit e uno a 64-bit risiede nel **numero di calcoli al secondo** che possono essere eseguiti. Un'architettura a 64-bit è in grado di gestire una quantità di dati significativamente superiore, migliorando le prestazioni generali del sistema.

#### 2. Compatibilità del Software
Esiste una regola precisa per quanto riguarda l'esecuzione delle applicazioni su diverse architetture:
* **Su un sistema a 64-bit**: è possibile eseguire sia applicazioni a 32-bit che a 64-bit.
* **Su un sistema a 32-bit**: è possibile eseguire esclusivamente applicazioni a 32-bit; il software a 64-bit non è compatibile.

#### 3. Identificazione dell'Architettura

**In ambiente Linux:**
Per determinare l'architettura in uso senza privilegi di root, si possono utilizzare i seguenti comandi:
* `arch`: un comando breve e immediato che restituisce direttamente l'architettura (ad esempio, riportando il valore `64` per i sistemi a 64-bit). È particolarmente utile all'interno di script di automazione.
* `uname -a`: fornisce una panoramica completa del sistema, includendo l'architettura verso la fine dell'output.

**In ambiente Windows:**
La verifica avviene tramite interfaccia grafica:
1.  Accedere a **Questo PC** (o Mio Computer).
2.  Cliccare con il tasto destro e selezionare **Proprietà**.
3.  Consultare la voce relativa al tipo di sistema per identificare se il processore è a 32 o 64-bit.

### Tasti di Controllo del Terminale Linux

In ambiente Linux, alcune combinazioni di tasti hanno un effetto speciale sul terminale, sia che si acceda tramite una sessione remota (come PuTTY) sia che si utilizzi direttamente la console. Queste funzioni si attivano tenendo premuto il tasto **CTRL** insieme a una seconda lettera.

#### Principali Combinazioni di Tasti

Di seguito sono elencati i tasti di controllo più comuni e le loro funzionalità:

* **CTRL+u**: Cancella l'intera riga di comando digitata. Risulta molto più rapido rispetto all'uso del tasto *backspace* per eliminare lunghi comandi carattere per carattere.
* **CTRL+c**: Interrompe o termina forzatamente un comando o un programma in esecuzione. È lo strumento principale da utilizzare quando un processo sembra bloccato o non restituisce il prompt dei comandi.
* **CTRL+z**: Sospende un comando in esecuzione, mettendolo in secondo piano (*background*) e restituendo immediatamente il controllo del prompt all'utente.
* **CTRL+d**: Permette di uscire da programmi interattivi o di chiudere la sessione corrente. Funge spesso da alternativa ai comandi specifici di uscita (come `quit` o `exit`).

#### Esempi Pratici di Utilizzo

| Scenario | Combinazione Consigliata |
| :--- | :--- |
| Errore nella digitazione di un comando lungo | **CTRL+u** per svuotare la riga |
| Programma interattivo (es. `top`) bloccato | **CTRL+c** per forzare la chiusura |
| Calcolatrice binaria (`bc`) o shell aperta | **CTRL+d** per terminare l'input o uscire |
| Necessità di liberare il prompt senza chiudere il processo | **CTRL+z** per sospendere l'attività |

---

### Comandi di Gestione del Terminale Linux

Esistono comandi specifici progettati per ottimizzare la gestione delle sessioni nel terminale e migliorare l'efficienza operativa dell'utente.

#### 1. Il Comando `clear`
Il comando `clear` viene utilizzato per ripulire lo schermo del terminale. Rimuove visivamente i comandi e gli output precedenti, riposizionando il prompt nella prima riga in alto. È prassi comune inserire questo comando all'inizio degli script per garantire che l'output sia presentato su una schermata pulita.

#### 2. Il Comando `exit`
Il comando `exit` permette di chiudere la sessione corrente. Le sue applicazioni principali includono:
* **Chiusura del terminale**: Termina la finestra della shell attiva.
* **Ritorno alla sessione originale**: Se è stato effettuato un cambio utente tramite il comando `su` (es. `su - spider`), digitando `exit` si torna all'utente precedentemente loggato.
* **Uscita da sub-shell**: Termina eventuali sessioni di shell annidate.

#### 3. Il Comando `script`
Il comando `script` è uno strumento avanzato che permette di registrare integralmente l'attività del terminale in un file di log.
* **Funzionamento**: Una volta avviato, registra ogni comando digitato e il relativo output prodotto dal sistema.
* **Sintassi**: `script [nome_file]`. Se l'utente non specifica un nome, il sistema utilizzerà il nome predefinito `typescript`.
* **Conclusione**: Per interrompere la registrazione, è necessario digitare `exit`. Il sistema restituirà il messaggio "script done".

| Utilità del comando `script` |
| :--- |
| **Troubleshooting**: Registra i passaggi eseguiti durante la risoluzione di un problema. |
| **Documentazione**: Crea un registro storico di procedure complesse da consultare in futuro. |
| **Audit**: Mantiene una traccia precisa di tutte le modifiche effettuate durante una sessione. |

---

### Procedura di Recupero della Password di Root in Linux

Ogni amministratore di sistema può trovarsi nella condizione di dover recuperare la password dell'utente **root**, a causa di smarrimento, errori di digitazione durante la modifica o standard di sicurezza che ne impongono il cambio periodico. Poiché per cambiare la password tramite il comando `passwd` è necessario essere già autenticati come root, la risoluzione richiede l'accesso fisico o tramite console al sistema per intervenire durante la fase di avvio.

#### 1. Accesso alla Console e Riavvio
Non è possibile eseguire questa procedura tramite una sessione SSH (come PuTTY), poiché la connessione viene interrotta durante il riavvio. È necessario operare direttamente sulla console della macchina (fisica o virtuale).
* Se non si hanno i permessi per riavviare via software, occorre procedere con un **reset hardware**.
* Durante la fase di boot, è necessario interagire con il menu di **GRUB** entro i primi 5 secondi.

#### 2. Modifica dei Parametri di Avvio (GRUB)
Dalla schermata di selezione del kernel:
1.  Selezionare la prima voce del menu e premere il tasto **`e`** per modificare i parametri.
2.  Individuare la riga che inizia con `linux` o `linux16`.
3.  Cercare il parametro **`ro`** (Read Only) e sostituirlo con le istruzioni per l'accesso in scrittura e l'avvio di una shell.

> **Configurazione per versioni recenti (CentOS/Red Hat):**
> Sostituire `ro` con:
> `rw init=/sysroot/bin/sh`
>
> *(Nota: in alcune versioni recenti come RHEL 9 è possibile utilizzare anche il parametro `rd.break` alla fine della riga).*

4.  Premere **`Ctrl + X`** per avviare il sistema con i parametri modificati.

#### 3. Modifica della Password in Modalità Utente Singolo
Una volta ottenuto l'accesso alla shell di emergenza, il file system è montato su `/sysroot`. Per operare correttamente, occorre seguire questi passaggi:

* **Cambio del root del file system**:
  ```bash
  chroot /sysroot
  ```
* **Impostazione della nuova password**:
  ```bash
  passwd root
  ```
* **Aggiornamento delle policy di sicurezza (SELinux): In molti sistemi moderni, è necessario forzare la rietichettatura dei file al riavvio per rendere effettive le modifiche:**:
  ```bash
  touch /.autorelabel
  ```

#### 4. Conclusione e Riavvio
Dopo aver ricevuto la conferma dell'aggiornamento dei token della password ("all authentication tokens updated successfully"):

1.  **Uscita dall'ambiente**: Digitare `exit` per uscire dall'ambiente *chroot*.
2.  **Uscita dalla shell**: Digitare nuovamente `exit` per uscire dalla shell di inizializzazione.
3.  **Riavvio**: Eseguire il comando `reboot`. Se il comando non fosse disponibile, il sistema procederà automaticamente al riavvio una volta terminati i processi della shell.

Al termine del riavvio, il sistema caricherà normalmente l'interfaccia di login, dove sarà possibile autenticarsi come **root** utilizzando la nuova password configurata. Si raccomanda di testare immediatamente l'accesso per confermare la riuscita dell'operazione.

---

### Utilizzo del Comando sosreport in Linux

In sistemi Red Hat, CentOS e derivati, il comando sosreport è lo strumento standard per la diagnostica di sistema. Analogamente al segnale internazionale di soccorso (SOS), viene impiegato quando il sistema presenta criticità che richiedono l'intervento del supporto tecnico ufficiale.

#### 1. Finalità del Report
L'obiettivo di sosreport è raccogliere e pacchettizzare automaticamente dati diagnostici per l'analisi. Questo evita scambi manuali di file tra l'amministratore e il supporto. Il report include:
* Configurazioni: File contenuti principalmente nella directory /etc.
* Log: Registri di sistema e dei servizi.
* Hardware: Informazioni su CPU, memoria e periferiche.

#### 2. Installazione e Requisiti
Il pacchetto necessario è solitamente preinstallato e si chiama "sos". Per avviare la raccolta dati, sono indispensabili i privilegi di root.

#### 3. Procedura di Generazione
Il comando da eseguire nel terminale è:

sosreport

Durante l'esecuzione, il sistema richiederà:
1. Conferma: Premere Invio per procedere o Ctrl+C per annullare.
2. Identificazione: Inserire nome e cognome (o iniziali) dell'amministratore.
3. Case ID: Inserire il numero del ticket aperto con il supporto (es. l'ID fornito da Red Hat).

#### 4. Gestione dell'Output
Al termine della procedura, che può durare diversi minuti in base alle risorse del sistema, il report viene salvato come archivio compresso.

* Percorso di salvataggio: /var/tmp/
* Formato file: .tar.xz (accompagnato da un file .md5 per la verifica dell'integrità).
* Esempio nome file: sosreport-utente-IDcaso-data.tar.xz

#### 5. Invio dei Dati
Il file generato può essere trasferito al supporto tecnico tramite i portali web ufficiali, protocollo FTP o scaricato localmente per un upload manuale. Il supporto utilizzerà questi dati per diagnosticare e risolvere il problema segnalato nel Case ID.

---

### Variabili d'Ambiente in Linux

Le variabili d'ambiente sono valori dinamici con un nome assegnato che influenzano il comportamento dei processi in esecuzione su un computer. Esse definiscono l'ambiente in cui l'utente opera, stabilendo regole e valori predefiniti.

Per analogia, si può pensare a una casa: ogni stanza ha una funzione specifica (nella cucina si cucina, nella camera da letto si dorme). Queste sono "regole" che definiscono l'ambiente domestico. In modo simile, in Linux, quando un utente effettua il login, il sistema gli assegna un ambiente specifico che include la sua directory home, il tipo di shell utilizzata, i colori per la visualizzazione dei file e i percorsi (path) per l'esecuzione dei comandi.

#### 1. Visualizzare le Variabili d'Ambiente
Per elencare tutte le variabili d'ambiente attive nella sessione corrente, è possibile utilizzare i seguenti comandi:
* `printenv`
* `env`

Per visualizzare il valore di una variabile specifica, si utilizza il comando `echo` seguito dal nome della variabile preceduto dal simbolo del dollaro (`$`):
* `echo $SHELL` (mostra la shell in uso, solitamente `/bin/bash`).
* `echo $PATH` (mostra i percorsi dove il sistema cerca i comandi eseguibili).
* `echo $HOME` (mostra la directory home dell'utente).

#### 2. Configurazione delle Variabili

Le variabili possono essere impostate in due modalità: temporanea o permanente.

| Tipo di Impostazione | Metodo | Ambito |
| :--- | :--- | :--- |
| **Temporanea** | `export NOME_VAR=valore` | Valida solo per la sessione corrente. Scompare al logout. |
| **Permanente (Utente)** | Modifica del file `~/.bashrc` | Valida per l'utente specifico a ogni nuovo login. |
| **Permanente (Globale)** | Modifica di `/etc/profile` o `/etc/bashrc` | Valida per tutti gli utenti del sistema. |

#### 3. Procedura per l'impostazione permanente (Livello Utente)
Per rendere una variabile persistente per il proprio utente, seguire questi passaggi:
1. Accedere alla propria home directory.
2. Creare una copia di backup del file di configurazione: `cp .bashrc .bashrc.bak`.
3. Modificare il file con un editor di testo (es. `vi .bashrc`).
4. Aggiungere in fondo al file le righe:
   ```bash
   TEST='123'
   export TEST
   ```
5. **Salvare e uscire**: Salvare le modifiche al file.
6. **Applicazione delle modifiche**: Le nuove variabili non saranno attive immediatamente nella sessione corrente. Per renderle effettive è necessario:
    * Chiudere la sessione e rieffettuare il login.
    * Oppure, avviare una nuova sessione di terminale.
    * In alternativa, eseguire il comando `source ~/.bashrc` per caricare le modifiche istantaneamente.

#### 4. Variabili Globali
L'impostazione globale (effettuata tramite i file contenuti in `/etc/`) deve essere gestita con estrema cautela. Errori di configurazione in questi file possono compromettere l'accesso al sistema per tutti gli utenti, incluso l'utente root. In ambito aziendale, è fondamentale testare queste modifiche in ambienti di sviluppo prima di applicarle ai sistemi di produzione.

### Permessi Speciali in Linux: SUID, SGID e Sticky Bit

In Linux, oltre ai permessi standard di lettura (**r**), scrittura (**w**) ed esecuzione (**x**), esistono permessi speciali che possono essere assegnati a file ed eseguibili per gestirne il comportamento in termini di privilegi e sicurezza.

#### 1. Concetti Fondamentali
I permessi sono suddivisi in tre categorie principali: **User** (proprietario), **Group** (gruppo) e **Others** (altri utenti). Questi vengono gestiti tramite il comando `chmod`.

#### 2. SUID (Set User ID)
Il bit **SUID** permette a un utente di eseguire un file con i privilegi del proprietario del file stesso, anziché con quelli dell'utente che lo lancia.
* **Esempio**: Il comando `passwd`. Quando un utente cambia la propria password, deve modificare il file `/etc/shadow`, il quale è accessibile solo da root. Grazie al SUID impostato su `/usr/bin/passwd`, il comando viene eseguito con i privilegi di root.
* **Identificazione**: Si nota una "s" minuscola nel campo del proprietario (es. `-rwsr-xr-x`).

#### 3. SGID (Set Group ID)
Simile al SUID, il bit **SGID** esegue un programma con i privilegi del gruppo proprietario.
* **Esempio**: Comandi come `locate` o `wall`.
* **Identificazione**: Si nota una "s" nel campo del gruppo (es. `-rwxr-sr-x`).

> **Nota**: SUID e SGID funzionano correttamente solo su file binari compilati (come in C o C++) e non hanno effetto sugli script bash.

#### 4. Sticky Bit
Lo **Sticky Bit** viene applicato principalmente alle directory per impedire che gli utenti eliminino o rinominino file contenuti in esse, a meno che non ne siano i proprietari o non siano l'utente root.
* **Esempio**: La directory `/tmp`. Tutti possono scriverci, ma nessuno può cancellare i file degli altri.
* **Identificazione**: Si nota una "t" nell'ultima posizione dei permessi (es. `drwxrwxrwt`).



---

#### 5. Gestione dei Permessi Speciali tramite `chmod`

| Permesso Speciale | Comando per l'attivazione | Comando per la rimozione |
| :--- | :--- | :--- |
| **SUID** | `chmod u+s <file>` | `chmod u-s <file>` |
| **SGID** | `chmod g+s <file>` | `chmod g-s <file>` |
| **Sticky Bit** | `chmod +t <directory>` | `chmod -t <directory>` |

Per trovare tutti i file nel sistema che hanno permessi SUID o SGID, è possibile utilizzare il comando:
```bash
find / -perm /6000 -type f
```
Markdown

#### 6. Esercitazione Pratica: Protezione con lo Sticky Bit

Per comprendere l'utilità dello Sticky Bit, si analizzi il seguente scenario di test condotto in un ambiente di laboratorio:

1.  **Preparazione della directory**: L'utente root crea una cartella denominata `/allinone` e assegna permessi di lettura, scrittura ed esecuzione totali a chiunque tramite il comando `chmod 777 /allinone`.
2.  **Test senza Sticky Bit**: 
    * L'utente "iafzal" crea una sottodirectory con dei file all'interno di `/allinone`.
    * Un secondo utente, "spiderman", accede alla cartella e tenta di eliminare la directory di "iafzal". 
    * In questa configurazione, l'eliminazione avviene con successo poiché l'accesso in scrittura sulla directory genitrice permette di rimuovere qualsiasi oggetto contenuto, indipendentemente dal proprietario.
3.  **Applicazione dello Sticky Bit**: 
    Per correggere questa vulnerabilità, l'amministratore applica lo Sticky Bit alla directory condivisa:
    ```bash
    chmod +t /allinone
    ```
4.  **Verifica della protezione**: 
    Se l'utente "spiderman" tenta nuovamente di eliminare i contenuti creati da "iafzal", il sistema bloccherà l'azione restituendo il messaggio d'errore:
    `Operation not permitted`

L'uso dello Sticky Bit è dunque essenziale in ogni directory comune per garantire che solo il proprietario di un file (o l'utente root) possa procedere alla sua rimozione.

---

### Guida al Comando `screen` in Linux

Il comando `screen` (o **GNU Screen**) è un "moltiplicatore di terminale" (terminal multiplexer). In termini semplici, permette di gestire sessioni multiple di terminale all'interno di una singola finestra e, cosa fondamentale, garantisce la persistenza dei processi.

#### 1. Perché utilizzare `screen`?
L'utilità principale risiede nella capacità di mantenere i processi in esecuzione in background anche se la connessione SSH cade o il terminale viene chiuso accidentalmente. È indispensabile per:
* **Task a lunga durata**: Come il trasferimento di file di grandi dimensioni.
* **Lavoro remoto**: Previene la perdita di dati in caso di instabilità della VPN o della rete.
* **Multitasking**: Permette di dividere lo schermo in più sezioni per monitorare server diversi contemporaneamente.



#### 2. Installazione
Su sistemi RHEL/CentOS e derivati, il pacchetto potrebbe non essere presente nei repository core.
1. **Verifica**: `rpm -qa | grep screen`
2. **Installazione Repository EPEL**: Poiché `screen` è spesso contenuto nel repository EPEL, eseguire:
   ```bash
   dnf install epel-release -y
   ```
3.  **Installazione**:
    ```bash
    dnf install screen -y
    ```

#### 3. Comandi e Scorciatoie Principali
Una volta digitato `screen` nel terminale, l'utility viene avviata. Per interagire con essa si utilizza una combinazione di tasti di comando. Nel tutorial viene utilizzato il prefisso `Alt + A` (sebbene lo standard sia spesso `Ctrl + A`).

| Azione | Scorciatoia |
| :--- | :--- |
| **Dividere verticalmente** | `Alt + A` seguita da `|` (pipe) |
| **Dividere orizzontalmente** | `Alt + A` seguita da `Shift + S` |
| **Creare nuova sessione** | `Alt + A` seguita da `C` |
| **Navigare tra finestre** | `Alt + A` seguita da `Tab` |

#### 4. Gestione delle Sessioni (Recupero del Lavoro)
La caratteristica più potente di `screen` è la capacità di gestire sessioni "appese".
* **Detached**: La sessione è in esecuzione in background ma non è visibile.
* **Attached**: La sessione è attualmente aperta e in uso.



Per elencare le sessioni attive e i relativi stati:
```bash
screen -ls
```

Per riconnettersi a una sessione precedentemente interrotta (ad esempio, dopo una caduta di rete), si utilizza l'opzione -r seguita dall'ID del processo:

```bash
screen -r [Process_ID]
```

#### 5. Esempio Pratico: Simulazione di Interruzione

Se un utente sta modificando un file con l'editor vi all'interno di una sessione screen e la connessione SSH si chiude improvvisamente, il file non andrà perduto. Riconnettendosi al server e digitando screen -r, l'utente si ritroverà esattamente all'interno dell'editor vi, con il testo ancora presente e il processo recuperato.


### Introduzione a TMUX (Terminal Multiplexer)

TMUX è un moltiplicatore di terminale moderno e dinamico per sistemi operativi di tipo Unix. Rappresenta l'evoluzione dello strumento `screen`, risolvendone problemi legati alla complessità del codice e alla mancanza di funzionalità moderne. È lo standard attuale per la gestione delle sessioni in distribuzioni come RHEL 8 e CentOS 8.

#### 1. Funzionalità Principali
TMUX permette di gestire molteplici sessioni di terminale all'interno di un'unica finestra. Le caratteristiche distintive includono:
* **Suddivisione in pannelli (panes)**: Divisione della finestra in senso verticale o orizzontale.
* **Persistenza delle sessioni**: Possibilità di scollegarsi da una sessione (detach), lasciando i processi in esecuzione in background, per poi ricollegarsi (attach) in un secondo momento senza perdita di dati.
* **Multitasking**: Ideale per il monitoraggio simultaneo di più server o l'esecuzione di diversi programmi a riga di comando.

#### 2. Installazione
Per installare TMUX su sistemi che utilizzano il gestore di pacchetti DNF, si utilizza il seguente comando (eseguito come root):
```bash
dnf install tmux -y
```

#### 3. Gestione delle Sessioni e Pannelli
All'avvio di TMUX tramite il comando `tmux`, viene attivata un'interfaccia caratterizzata da una barra di stato nella parte inferiore. Questa barra mostra il numero della sessione, il nome della finestra attiva (seguito da un asterisco `*`) e, sulla destra, informazioni come il nome dell'host, l'ora e la data.

Le operazioni all'interno di TMUX sono gestite tramite una combinazione di tasti "prefisso". Di default, questa combinazione è **Ctrl + B**. Per eseguire un comando, è necessario premere il prefisso, rilasciarlo e poi premere il tasto specifico dell'azione desiderata.



| Azione | Combinazione di tasti |
| :--- | :--- |
| **Suddivisione Verticale** | `Ctrl + B` seguita da `%` (Shift + 5) |
| **Suddivisione Orizzontale** | `Ctrl + B` seguita da `"` (Shift + ') |
| **Spostarsi tra Pannelli** | `Ctrl + B` seguita dalle frecce direzionali |
| **Creare una Nuova Finestra** | `Ctrl + B` seguita da `C` |
| **Passare alla Finestra Successiva** | `Ctrl + B` seguita da `N` |
| **Rinomina Finestra Corrente** | `Ctrl + B` seguita da `,` (virgola) |
| **Scollegarsi (Detach)** | `Ctrl + B` seguita da `D` |
| **Chiudere Pannello/Finestra** | `Ctrl + D` (o digitando `exit`) |

#### 4. Gestione Avanzata tramite Riga di Comando
TMUX offre una flessibilità superiore permettendo di gestire le sessioni anche dall'esterno (shell standard), senza necessariamente trovarsi dentro l'ambiente multiplexer.

* **Creare una sessione con nome**: Utilizzare nomi descrittivi facilita il recupero del lavoro.
    ```bash
    tmux new -s nome_progetto
    ```
* **Elencare le sessioni**: Visualizza tutte le istanze di TMUX attive in background.
    ```bash
    tmux ls
    ```
* **Riconnettersi (Attach)**: Per riprendere una sessione specifica o l'ultima attiva.
    ```bash
    tmux a  # Riconnette all'ultima sessione
    tmux attach-session -t nome_progetto # Riconnette a una specifica
    ```
* **Eliminare sessioni**: Per chiudere definitivamente una sessione e terminare i relativi processi.
    ```bash
    tmux kill-session -t nome_sessione
    ```


