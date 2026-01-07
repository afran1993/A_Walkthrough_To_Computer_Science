# Analisi Tecnica: Sintassi dei Comandi Linux

La struttura operativa di un comando in ambiente Unix-like è definita da una sequenza gerarchica di tre elementi fondamentali: il comando, le opzioni e gli argomenti.

---

## 1. Struttura Standard della Sintassi

Ogni istruzione impartita tramite shell segue il modello logico:
`comando [opzione/i] [argomento/i]`



### Componenti del comando
* **Comando:** Il nome dell'eseguibile o dell'utility di sistema (es. `ls`, `rm`, `mkdir`).
* **Opzione (Flag):** Modifica il comportamento del comando. È generalmente preceduta da un trattino singolo (`-`) per opzioni a lettera singola o doppio trattino (`--`) per opzioni estese.
* **Argomento:** Definisce l'entità su cui il comando agisce (es. percorsi di file, directory o stringhe di testo).

---

## 2. Implementazione delle Opzioni

Le opzioni permettono di raffinare l'output o la modalità di esecuzione. 

* **Raggruppamento:** Più opzioni possono spesso essere concatenate sotto un unico trattino. 
    * Esempio: `ls -ltr` combina `-l` (formato lungo), `-t` (ordinamento temporale) e `-r` (ordine inverso).
* **Funzione:** L'aggiunta di un'opzione altera il set di metadati visualizzati o il rigore dell'azione eseguita (es. l'opzione `-f` in `rm` forza la rimozione ignorando avvisi di sistema).

---

## 3. Casi d'Uso Operativi

| Comando | Descrizione Tecnica | Esempio Sintattico |
| :--- | :--- | :--- |
| `whoami` | Restituisce l'identificativo dell'utente corrente. | `whoami` |
| `pwd` | Visualizza il percorso della directory di lavoro corrente. | `pwd` |
| `ls` | Elenca i contenuti di una directory. | `ls -l [directory]` |
| `rm` | Rimuove file o directory. Richiede `-r` per le directory. | `rm -r [nome_cartella]` |
| `mkdir` | Crea una nuova directory nel file system. | `mkdir [nome_cartella]` |

---

## 4. Strumenti di Documentazione (Manual Pages)

Il sistema Linux integra una guida di riferimento consultabile tramite il comando `man`. 

* **Sintassi:** `man [comando]`
* **Navigazione Interfaccia:**
    * `Spacebar`: Avanzamento di pagina.
    * `Q`: Terminazione del processo di visualizzazione (Quit) e ritorno al prompt.

---

## 5. Script di Esempio Pratico

```bash
# Identificazione ambiente e posizione
whoami
pwd

# Elenco dettagliato ordinato per data (file più recenti in fondo)
ls -ltr

# Operazione su argomento specifico
ls -l /home/user/documento.txt

# Gestione Directory: Rimozione e Ricreazione
rm -r directory_test
mkdir directory_test
```

# Analisi Tecnica: Gestione dei Permessi del File System

Il sistema operativo Linux, essendo un ambiente multi-utente, utilizza un sistema di protezione basato sui permessi per garantire la riservatezza e l'integrità dei dati. Ogni file e directory possiede un set di attributi che definisce chi può leggere, modificare o eseguire la risorsa.

---

## 1. Struttura dei Permessi

I permessi sono definiti per tre classi di utenti e si compongono di tre tipi di accesso fondamentali.

### Classi di Utenti
* **User (u):** Il proprietario (owner) del file.
* **Group (g):** Gli utenti appartenenti al gruppo associato al file.
* **Others (o):** Tutti gli altri utenti del sistema.
* **All (a):** Rappresenta la combinazione di tutte le classi precedenti.

### Tipologie di Accesso
1.  **Read (r):** Permette di visualizzare il contenuto di un file o elencare i file in una directory.
2.  **Write (w):** Permette di modificare/eliminare un file o creare/rimuovere file in una directory.
3.  **Execute (x):** * Per i **file**: permette l'esecuzione se il file è uno script o un programma.
    * Per le **directory**: è indispensabile per poter entrare nella directory (comando `cd`).



---

## 2. Interpretazione dell'Output `ls -l`

L'esecuzione del comando `ls -l` restituisce una stringa di 10 caratteri che descrive i permessi.

**Esempio:** `-rwxr-xr--`

| Posizione | Descrizione | Significato Esempio |
| :--- | :--- | :--- |
| 1 | Tipo di risorsa | `-` = File; `d` = Directory |
| 2-4 | Permessi Proprietario | `rwx` = Lettura, Scrittura, Esecuzione |
| 5-7 | Permessi Gruppo | `r-x` = Lettura ed Esecuzione |
| 8-10 | Permessi Altri | `r--` = Solo Lettura |

---

## 3. Modifica dei Permessi con `chmod`

Il comando `chmod` (Change Mode) viene utilizzato per modificare i bit di permesso utilizzando la sintassi simbolica:
`chmod [Classe][Operatore][Permesso] [File/Directory]`

### Operatori
* `+`: Aggiunge un permesso.
* `-`: Rimuove un permesso.
* `=`: Imposta esattamente i permessi specificati.

---

## 4. Esempi Operativi (Script Bash)

```bash
# 1. Analisi iniziale del file
ls -l documento.txt

# 2. Rimuovere il permesso di scrittura al gruppo
# Comando: chmod | Opzione: g-w | Argomento: documento.txt
chmod g-w documento.txt

# 3. Negare la lettura a tutti (All)
chmod a-r documento.txt

# 4. Aggiungere lettura e scrittura al proprietario (User)
chmod u+rw documento.txt

# 5. Gestione Directory: negare l'accesso (cd) a chiunque
# Senza il bit 'x', la directory non è attraversabile
chmod a-x cartella_test

# 6. Ripristino totale dei permessi di esecuzione per le directory
chmod a+x cartella_test
```

# Documentazione Tecnica: Permessi Numerici (Notazione Ottale) in Linux

Oltre alla notazione simbolica (lettere), i sistemi UNIX-like permettono di definire i permessi di accesso tramite **valori numerici ottali**. Questo metodo consente di impostare l'intera triade di permessi (User, Group, Others) con un singolo comando sintetico.

---

## 1. Struttura della Notazione Ottale

Il valore numerico è composto da tre cifre decimali. Ogni posizione corrisponde a un livello di autorità:
1.  **Prima cifra:** Permessi dell'utente proprietario (**User**).
2.  **Seconda cifra:** Permessi del gruppo (**Group**).
3.  **Terza cifra:** Permessi degli altri utenti (**Others**).



---

## 2. Valori dei Permessi e Calcolo

Ogni permesso ha un valore binario pesato. La cifra finale per ogni categoria è data dalla **somma** dei valori dei singoli permessi desiderati.

| Valore | Permesso | Descrizione |
| :--- | :--- | :--- |
| **4** | `r--` | Lettura (**Read**) |
| **2** | `-w-` | Scrittura (**Write**) |
| **1** | `--x` | Esecuzione (**Execute**) |
| **0** | `---` | Nessun permesso |

### Combinazioni comuni:
* **7** (4+2+1): `rwx` (Accesso completo)
* **6** (4+2): `rw-` (Lettura e scrittura)
* **5** (4+1): `r-x` (Lettura ed esecuzione)
* **4**: `r--` (Solo lettura)

---

## 3. Analisi di un Esempio: `chmod 764`

L'applicazione del comando `chmod 764 file` produce il seguente schema:

* **7 (User):** Lettura + Scrittura + Esecuzione ($4+2+1$) $\rightarrow$ `rwx`
* **6 (Group):** Lettura + Scrittura ($4+2$) $\rightarrow$ `rw-`
* **4 (Others):** Solo Lettura ($4$) $\rightarrow$ `r--`
* **Risultato finale:** `-rwxrw-r--`

---

## 4. Script Operativo (Esempi Bash)

```bash
# Creazione di un file di test
touch sam

# Assegnazione permessi rwx per proprietario, rw per gruppo, r per altri
# Corrisponde a: User=7, Group=6, Others=4
chmod 764 sam

# Verifica dell'output
ls -l sam

# Reset totale: rimuove ogni permesso per chiunque
chmod 000 sam

# Impostazione solo lettura e scrittura per il proprietario, nulla per gli altri
chmod 600 sam

# Assegnazione solo esecuzione a tutti (utile per script minimi)
chmod 111 sam

# Impostazione permessi standard per directory (rwxr-xr-x)
chmod 755 nome_directory
```

# Documentazione Tecnica: Proprietà dei File e delle Directory (Ownership)

In ambiente Linux, ogni risorsa del file system è associata a un proprietario specifico e a un gruppo. La gestione della proprietà è fondamentale per determinare chi ha il controllo operativo sulle risorse.

---

## 1. I Livelli di Proprietà

Ogni file o directory possiede due livelli di proprietà definiti al momento della creazione:
* **User (Owner):** L'utente specifico che ha creato il file o a cui è stata assegnata la proprietà.
* **Group:** Il gruppo di utenti che condivide determinati diritti di accesso sulla risorsa.



### Identificazione della Proprietà
Utilizzando il comando `ls -l`, è possibile identificare i proprietari nelle colonne 3 e 4 dell'output:
* **Colonna 3:** Nome dell'utente proprietario.
* **Colonna 4:** Nome del gruppo proprietario.

---

## 2. Comandi per la Modifica della Proprietà

La modifica dei proprietari è un'operazione amministrativa che richiede solitamente privilegi di superutente (**root**).

### CHOWN (Change Owner)
Viene utilizzato per cambiare l'utente proprietario di un file o di una directory.
* **Sintassi:** `chown [nuovo_utente] [file/directory]`
* **Esempio:** `chown root lisa` (assegna il file "lisa" all'utente root).

### CHGRP (Change Group)
Viene utilizzato specificamente per cambiare il gruppo associato alla risorsa.
* **Sintassi:** `chgrp [nuovo_gruppo] [file/directory]`
* **Esempio:** `chgrp root lisa` (assegna il file "lisa" al gruppo root).

### Modifica Ricorsiva
Per applicare il cambio di proprietà a una directory e a tutto il suo contenuto (sottodirectory e file inclusi), si utilizza l'opzione `-R` (maiuscola).
* **Esempio:** `chown -R utente:gruppo directory/`

---

## 3. Relazione tra Proprietà e Permessi di Scrittura

Un concetto critico nella gestione del file system è la gerarchia dei permessi della directory padre:

1.  **Eliminazione file:** Se un utente ha permessi di scrittura (`w`) su una directory, può eliminare qualsiasi file al suo interno, indipendentemente da chi sia il proprietario del singolo file.
2.  **Restrizioni di sistema:** Directory critiche come `/etc` sono di proprietà di `root`. Un utente regolare non può creare o eliminare file in queste posizioni poiché non possiede i permessi di scrittura sulla directory stessa, anche se tenta di agire come proprietario del file.

---

## 4. Esempi Operativi (Script Bash)

```bash
# Diventare superutente per eseguire modifiche di sistema
su - 

# Cambiare utente proprietario di un file
chown root test_file

# Cambiare gruppo proprietario di un file
chgrp staff test_file

# Cambiare contemporaneamente Utente e Gruppo
chown root:staff test_file

# Applicare la proprietà in modo ricorsivo a una cartella
chown -R admin:admin /home/user/my_folder

# Verifica della nuova configurazione
ls -l test_file
```

# Documentazione Tecnica: Access Control Lists (ACL) in Linux

Le **Access Control Lists (ACL)** rappresentano un'estensione del modello di permessi standard UNIX (proprietario, gruppo, altri). Forniscono un meccanismo più flessibile e granulare per definire i diritti di accesso, permettendo di assegnare permessi specifici a singoli utenti o gruppi che non coincidono con il proprietario o con il gruppo primario della risorsa.

---

## 1. Architettura e Scopo

Mentre i permessi standard (UGO) limitano la gestione a tre entità, le ACL operano come uno strato aggiuntivo che consente di:
* Assegnare permessi a un utente specifico (User B) su un file di proprietà di un altro utente (User A).
* Definire accessi diversi per più gruppi sullo stesso file.
* Evitare la proliferazione di gruppi di sistema creati solo per scopi di condivisione file.



---

## 2. Comandi di Gestione

Per l'amministrazione delle ACL vengono utilizzati due strumenti principali:

| Comando | Operazione | Descrizione |
| :--- | :--- | :--- |
| `getfacl` | Lettura | Visualizza i dettagli dei permessi ACL correnti. |
| `setfacl` | Scrittura | Modifica, aggiunge o rimuove le voci ACL. |

---

## 3. Sintassi Operativa

### A. Visualizzazione
Per controllare se un file ha ACL attive e quali siano:
```bash
getfacl /percorso/del/file
```

# Documentazione Tecnica: Sistemi di Supporto e Aiuto in Linux

L'ambiente Linux offre strumenti nativi per la consultazione della documentazione direttamente dalla riga di comando. Questi strumenti variano per livello di dettaglio, dalla descrizione sintetica alla guida esaustiva.

---

## 1. Il Comando `whatis`
Il comando `whatis` estrae una descrizione di una riga dai database dei manuali di sistema. È lo strumento ideale per identificare rapidamente lo scopo di un comando sconosciuto.

* **Sintassi:** `whatis [comando]`
* **Caratteristiche:** Fornisce solo la funzione primaria della risorsa.
* **Nota tecnica:** Se `whatis` restituisce più definizioni (come per `pwd`), significa che il comando è referenziato in diverse sezioni dei manuali (es. come comando integrato della shell e come eseguibile di sistema).

---

## 2. L'Opzione `--help`
Quasi tutti i programmi e le utility Linux includono un'opzione di aiuto interna. A differenza dei manuali esterni, questa è compilata direttamente nel software.

* **Sintassi:** `[comando] --help`
* **Contenuto:**
    * Sintassi d'uso (usage).
    * Elenco sintetico delle opzioni (flag) disponibili.
    * Brevi descrizioni degli argomenti.

---

## 3. Il Comando `man` (Manual Pages)
Il comando `man` è il sistema di documentazione standard in ambiente UNIX/Linux. Fornisce manuali completi, formattati in modo gerarchico e professionale.



### Navigazione nel Manuale
Quando si consulta una pagina `man`, il sistema utilizza un "pager" (solitamente `less`) per la visualizzazione:
* **Barra Spaziatrice:** Scorrimento verso il basso di una pagina intera.
* **Tasti Freccia:** Scorrimento riga per riga.
* **Q:** Uscita dal manuale e ritorno al prompt.

### Struttura Standard di una Pagina Man
| Sezione | Descrizione |
| :--- | :--- |
| **NAME** | Nome del comando e una breve sintesi. |
| **SYNOPSIS** | Schema formale della sintassi e dei parametri richiesti. |
| **DESCRIPTION** | Spiegazione dettagliata del funzionamento del comando. |
| **OPTIONS** | Descrizione esaustiva di ogni singolo flag e argomento. |

---

## 4. Esempi Operativi

```bash
# Identificazione rapida del comando 'ls'
whatis ls

# Visualizzazione delle opzioni disponibili per 'chmod'
chmod --help

# Apertura del manuale completo per la gestione degli utenti (chown)
man chown

# Ricerca di informazioni sul comando 'pwd'
whatis pwd
```

# Documentazione Tecnica: Efficienza nel Terminale Linux

L'interfaccia a riga di comando (CLI) di Linux integra funzionalità progettate per ottimizzare la velocità di inserimento e ridurre l'errore umano. Le due funzioni più utilizzate sono il completamento automatico tramite il tasto **TAB** e la navigazione cronologica tramite le **frecce direzionali**.

---

## 1. Tab-Completion (Completamento Automatico)

Il tasto **TAB** permette alla shell di completare automaticamente nomi di comandi, file o percorsi di directory. È uno strumento essenziale per garantire la precisione dei comandi inseriti.

### Modalità di funzionamento:
* **Completamento Univoco:** Se i caratteri digitati corrispondono a un'unica risorsa, premendo **TAB** una volta il sistema completa automaticamente il nome.
    * *Esempio:* Inserendo `chm` e premendo **TAB**, il comando verrà completato in `chmod`.
* **Corrispondenze Multiple:** Se esistono più risorse che iniziano con gli stessi caratteri, premendo **TAB** due volte rapidamente (Double-TAB), il terminale elencherà tutte le opzioni disponibili.
    * *Esempio:* Se nella directory sono presenti i file `lex`, `lisa` e `lois`, digitando `ls l` e premendo **TAB** due volte, verranno visualizzate tutte e tre le occorrenze.



### Vantaggi:
1.  **Velocità operativa:** Riduce il numero di tasti premuti per percorsi complessi.
2.  **Validazione in tempo reale:** Se il tasto TAB non produce risultati, è un segnale immediato che il percorso o il nome del comando è errato.

---

## 2. Storico dei Comandi (Frecce Direzionali)

Il sistema memorizza i comandi eseguiti in una sessione, permettendo all'utente di richiamarli senza doverli digitare nuovamente.

* **Freccia SU ($\uparrow$):** Scorre lo storico dei comandi a ritroso (dal più recente al meno recente).
* **Freccia GIÙ ($\downarrow$):** Scorre lo storico in avanti (verso i comandi più recenti).



### Casi d'uso:
Questa funzione è fondamentale per rieseguire comandi lunghi o per correggere rapidamente errori di sintassi in un comando appena inviato.

---

## 3. Esempi Pratici

```bash
# Esempio: Entrare rapidamente in una directory
# Invece di scrivere l'intero nome:
cd Desk[TAB]
# Il terminale completa in: cd Desktop/

# Esempio: Ricerca di comandi con prefisso comune
# Digitare 'ch' e premere TAB due volte:
ch[TAB][TAB]
# Output: chgrp  chmod  chown  chsh (e altri...)

# Esempio: Recupero rapido
# Eseguire un comando:
date
# Premere Freccia SU per richiamarlo e premere INVIO per rieseguirlo.
```

# Documentazione Tecnica: Gestione Testo e Reindirizzamenti (Redirect) in Linux

Esistono diverse metodologie per popolare un file di informazioni o per aggiungere testo a file precedentemente creati (ad esempio con il comando `touch`). In questa guida esploreremo l'uso dei comandi `echo`, `cat` e degli operatori di reindirizzamento.

---

## 1. Il Comando `echo`
Il comando `echo` viene utilizzato per stampare a video (standard output) una stringa di testo o una sequenza di caratteri digitati dall'utente.

* **Funzionamento base:**
  ```bash
  echo "Ciao, questo è un test"
  ```

* **Risultato:** Il terminale visualizzerà semplicemente la frase indicata.

---

## 2. Operatori di Reindirizzamento (Redirect)

Per salvare l'output di un comando in un file invece di visualizzarlo a schermo, si utilizzano i simboli di maggiore (`>`).



### A. Reindirizzamento Singolo (`>`) - Sovrascrittura
L'operatore `>` crea un nuovo file o **sovrascrive** completamente il contenuto di un file esistente se questo è già presente.
* **Sintassi:** `comando > nome_file`
* **Esempio:**
  ```bash
  echo "Jerry Seinfeld è il protagonista" > jerry.txt
  ```

* **Nota:** Se il file `jerry.txt` conteneva del testo, questo verrà perso definitivamente a causa della sovrascrittura.

### B. Reindirizzamento Doppio (`>>`) - Append
L'operatore `>>` permette di aggiungere nuovo testo alla fine di un file esistente. A differenza dell'operatore singolo, questo **non cancella** il contenuto precedente, ma lo integra (operazione di *append*).

* **Sintassi:** `comando >> nome_file`
* **Esempio:**
    ```bash
    echo "La serie è stata creata nel 1989" >> jerry.txt
    ```

---

## 3. Visualizzare il Contenuto con `cat`
In ambiente Linux, non esistendo l'interazione tramite "doppio clic" tipica delle interfacce grafiche, si utilizza il comando `cat` (*concatenate*) per leggere il contenuto testuale di un file direttamente nel terminale.



* **Sintassi:** `cat nome_file`
* **Esempio:**
    ```bash
    cat jerry.txt
    ```
    *Risultato:* Verranno mostrate a video tutte le righe aggiunte precedentemente con i comandi di redirect.

---

## 4. Reindirizzare l'Output di altri Comandi
Il reindirizzamento non è limitato al comando `echo`. È possibile dirigere l'output di qualsiasi comando di sistema verso un file per scopi di log o archiviazione.

### Esempio con `ls`:
Per salvare l'elenco dettagliato dei file di una directory in un documento di testo:
```bash
ls -ltr > elenco_file.txt
```

### Esempio con `date`:
Per aggiungere un riferimento temporale (data e ora correnti) in coda a un file di log o a un documento esistente, si utilizza il comando `date` combinato con l'operatore di append:

```bash
date >> jerry.txt
```

---

## 5. Tabella Riassuntiva degli Operatori

| Operatore | Nome | Effetto sul File | Comportamento |
| :--- | :--- | :--- | :--- |
| `>` | Redirect Singolo | **Sovrascrittura** | Crea un nuovo file o svuota quello esistente prima di scrivere. |
| `>>` | Redirect Doppio | **Append** | Aggiunge i dati alla fine del file, preservando il contenuto esistente. |



---

## 6. Metodi Alternativi: Editor di Testo e Visualizzazione

Oltre ai reindirizzamenti tramite shell, esistono metodi più avanzati per gestire i contenuti dei file:

* **Editor `vi` / `vim`:** Un editor di testo interattivo da terminale che permette di scrivere, modificare e gestire file complessi in modalità testo.
* **Comandi di lettura mirata:** * `cat`: Mostra l'intero contenuto del file.
    * `head`: Visualizza le prime righe di un file.
    * `tail`: Visualizza le ultime righe di un file (utile per i file di log).

---

## Esercizio Pratico Suggerito

Per consolidare la comprensione della differenza tra l'operatore di sovrascrittura e quello di aggiunta, si consiglia di eseguire la seguente sequenza di comandi:

1.  **Inizializzazione:** `echo "Dati Sessione 1" > sessione.log`
2.  **Verifica:** `cat sessione.log`
3.  **Aggiunta dati (Append):** `date >> sessione.log`
4.  **Sovrascrittura accidentale:** `echo "Nuovi Dati" > sessione.log`
5.  **Verifica finale:** `cat sessione.log` 
    *(Si noterà come i dati precedenti e il timestamp della data siano stati eliminati e sostituiti).*

# Documentazione Tecnica: Gestione dei Flussi I/O in Linux

In Linux, il sistema operativo considera ogni risorsa (tastiera, monitor, file o directory) come un **file**. Per gestire il passaggio di informazioni, Linux utilizza tre canali di comunicazione standard, identificati da **File Descriptor (FD)** numerici.

---

## 1. I Tre Flussi Standard (File Descriptors)

| Nome | Sigla | FD | Descrizione | Hardware Associato |
| :--- | :--- | :--- | :--- | :--- |
| **Standard Input** | `STDIN` | **0** | Flusso dei dati in entrata (es. digitazione). | Tastiera |
| **Standard Output** | `STDOUT` | **1** | Flusso dei risultati corretti del comando. | Monitor |
| **Standard Error** | `STDERR` | **2** | Flusso dei messaggi di errore. | Monitor |



---

## 2. Reindirizzamento dell'Output (STDOUT - FD 1)

Per impostazione predefinita, l'output di un comando viene visualizzato sul monitor. È possibile deviare questo flusso verso un file utilizzando il simbolo `>`.

### Sovrascrittura (`>`)
Sostituisce completamente il contenuto del file di destinazione.
* **Esempio:** `ls -l > listings.txt`
  *L'output del comando `ls` non apparirà a schermo ma verrà salvato nel file.*

### Append (`>>`)
Aggiunge i dati alla fine del file senza cancellare le informazioni esistenti.
* **Esempio:** `echo "Hello World" >> findpath.txt`
  *Utile per mantenere uno storico dei dati o dei log.*

---

## 3. Reindirizzamento degli Errori (STDERR - FD 2)

Quando un comando fallisce (es. per permessi negati o file non trovati), il sistema genera un errore sul descrittore **2**. Per catturare questi messaggi in un file dedicato, si antepone il numero `2` al simbolo di redirect.



### Esempi Pratici:
* **Log degli errori:** `ls -l /root 2> errorfile.txt`  
  *(Se non sei root, il messaggio "Permission denied" verrà scritto nel file invece che a schermo).*
* **Pulizia output:** `telnet localhost 2> errorfile.txt`  
  *(I tentativi di connessione falliti verranno loggati nel file).*

---

## 4. Reindirizzamento dell'Input (STDIN - FD 0)

L'operatore `<` permette di "nutrire" un comando o uno script utilizzando il contenuto di un file come se fosse digitato in tempo reale dall'utente.

* **Esempio:** `cat < findpath.txt`
* **Caso d'uso avanzato:** Inviare una mail pre-scritta tramite script:
  `mail -s "Oggetto" utente@esempio.com < corpo_mail.txt`

---

## 5. Esempi Operativi di Riepilogo

```bash
# Salvare il percorso attuale in un nuovo file
pwd > findpath.txt

# Aggiungere l'elenco dei file nascosti allo stesso file (Append)
ls -la >> listings.txt

# Tentare di leggere una cartella protetta e salvare l'errore
ls -l /root 2> errori_sistema.log

# Visualizzare il contenuto dei log per verifica
cat errori_sistema.log
```

# Documentazione Tecnica: Il Comando `tee`

Il comando **`tee`** è uno strumento essenziale per la gestione dei flussi di dati in ambiente Linux/Unix. Il suo scopo principale è leggere i dati dallo **Standard Input (STDIN)** e scriverli contemporaneamente sia sullo **Standard Output (STDOUT)** (il monitor) sia in uno o più **file**.

---

## 1. Il Concetto di "T-Splitter"
Il nome del comando si ispira al **raccordo a T** utilizzato nell'idraulica. Proprio come un tubo a T divide il flusso d'acqua in due direzioni diverse, il comando `tee` sdoppia il flusso di dati:
1.  Verso il **terminale** (per visualizzare l'output in tempo reale).
2.  Verso un **file** (per l'archiviazione o il logging).



### Differenza tra `tee` e il reindirizzamento classico (`>`)
* **Con `>`:** L'output viene inviato direttamente al file e non viene visualizzato nulla sul terminale.
* **Con `tee`:** L'output viene visualizzato a schermo **mentre** viene salvato nel file. Questo permette di verificare la correttezza di un'operazione durante la sua esecuzione.

---

## 2. Sintassi e Utilizzo

Il comando `tee` viene utilizzato tipicamente in combinazione con una **pipe** (`|`).

### Scrittura su file (Sovrascrittura)
Di default, `tee` sovrascrive il contenuto del file specificato.
```bash
comando | tee nome_file
```
Esempio:
`echo "David Puddy è il ragazzo di Elaine" | tee elaine-david.txt`
Aggiunta a un file (Append)

Per aggiungere dati a un file esistente senza cancellare i dati precedenti, si utilizza l'opzione -a (append).
`echo "David e Elaine sono una coppia" | tee -a elaine-david.txt`

## 3. Casi d'Uso Avanzati

### Scrittura su più file simultaneamente
`tee` può scrivere lo stesso output in più file contemporaneamente, una funzione che i normali operatori di redirect non possono svolgere con una singola istruzione:
```bash
ls -l | tee file1.txt file2.txt file3.txt
```

### Logging di comandi e analisi (wc)

È possibile utilizzare tee per visualizzare l'output di un comando, salvarlo e contemporaneamente passarlo a un altro strumento di analisi come wc (word count):
```bash
ls -l | tee listdir.txt | wc -l
```
In questo caso, vedrai l'elenco dei file, il numero totale di righe verrà stampato in fondo e tutto l'elenco sarà salvato in listdir.txt.

## 4. Tabella Riassuntiva delle Opzioni

| Opzione | Nome Esteso | Descrizione |
| :--- | :--- | :--- |
| **`-a`** | `--append` | Aggiunge l'output alla fine del file senza sovrascrivere il contenuto precedente. |
| **`-i`** | `--ignore-interrupts` | Ignora i segnali di interruzione (es. `CTRL+C`) per garantire il completamento della scrittura. |
| **`--help`** | | Mostra il manuale rapido con tutte le opzioni disponibili. |
| **`--version`** | | Visualizza la versione del comando e le informazioni sugli sviluppatori (es. Richard M. Stallman). |

---

## 5. Script Bash Riassuntivo (Copiabile)

Puoi copiare ed eseguire questo blocco nel tuo terminale per testare tutte le funzionalità descritte nella lezione:

```bash
# --- ESEMPIO PRATICO COMANDO TEE ---

# 1. Crea un file e visualizza l'output (Sovrascrittura)
echo "David Puddy è il ragazzo di Elaine" | tee elaine-david.txt

# 2. Aggiungi una riga allo stesso file e visualizza (Append)
echo "David e Elaine sono una coppia" | tee -a elaine-david.txt

# 3. Conta i caratteri nel file e salva il risultato a schermo
wc -c elaine-david.txt | tee conteggio.txt

# 4. Salva l'elenco dei file in tre destinazioni diverse contemporaneamente
ls -l | tee file1.txt file2.txt file3.txt

# 5. Verifica finale del contenuto
echo -e "\n--- Contenuto finale di elaine-david.txt ---"
cat elaine-david.txt
```

# Documentazione Tecnica: Le Pipe in Linux

In ambiente Linux, una **Pipe** (tubo) è uno strumento utilizzato dalla shell per collegare l'output di un comando direttamente all'input di un altro comando. Rappresenta uno dei concetti più potenti della filosofia Unix: combinare piccoli strumenti specializzati per eseguire compiti complessi.

---

## 1. Concetto e Sintassi
Il simbolo della pipe è la barra verticale (**`|`**). 

Funziona come un condotto: i dati "escono" dal primo comando e "entrano" nel secondo senza dover passare per la creazione di file temporanei.



### Sintassi Base:
comando1 [opzioni] | comando2 [opzioni]

### Posizionamento sulla tastiera:
* **Mac:** Generalmente si trova sul tasto all'estrema destra della tastiera.
* **PC/Windows:** Si trova solitamente sotto il tasto Backspace o vicino al tasto Invio, attivabile con `Shift` + `\`.

---

## 2. Comandi di Supporto Comuni
Le pipe vengono spesso utilizzate con comandi che aiutano a gestire o filtrare grandi quantità di dati:

* **`more`**: Permette di visualizzare l'output una pagina alla volta. Utilizza la `Barra Spaziatrice` per scendere di una pagina e `Q` per uscire.
* **`tail`**: Estrae le ultime righe di un output. L'opzione `-1` estrae solo l'ultima riga.
* **`ll`**: Un alias comune per `ls -l` (visualizzazione dettagliata).

---

## 3. Esempi Pratici

### Visualizzazione Paginata
Nella directory `/etc` sono presenti moltissimi file. Eseguendo un semplice `ls`, i nomi scorrerebbero troppo velocemente. Usando la pipe con `more`, possiamo leggerli con calma:
`ls -ltr /etc | more`

### Estrazione dell'ultimo elemento
Se vogliamo conoscere solo l'ultimo file modificato o l'ultima riga di un elenco generato:
`ls -l | tail -1`

---

## 4. Importanza delle Pipe
Le pipe sono fondamentali per l'automazione e il filtraggio dei dati. Permettono di raffinare i risultati di un comando finché non si ottiene esattamente l'informazione cercata, concatenando potenzialmente decine di comandi in una singola riga.

---

## 5. Esercizio di Riepilogo (Script Bash)

```bash
# --- ESEMPIO PRATICO UTILIZZO PIPE ---

# 1. Spostiamoci in una directory densa di file
cd /etc

# 2. Elenchiamo i file e leggiamoli pagina per pagina
# (Premere Spazio per scorrere, Q per uscire)
ls -ltr | more

# 3. Torniamo alla home
cd ~

# 4. Creiamo un esempio per estrarre solo l'ultima riga di un elenco
echo "Generazione elenco file nella home..."
ls -l | tail -1
```

# Documentazione Tecnica: Comandi di Manutenzione File

In questa lezione analizziamo i comandi fondamentali per gestire, spostare, rinominare e cambiare la proprietà di file e directory in ambiente Linux.

---

## 1. Gestione di File e Directory

### Copiare File (`cp`)
Il comando `cp` (copy) duplica un file da una sorgente a una destinazione.
* **Sintassi:** `cp [sorgente] [destinazione]`
* **Esempio:** `cp george david` (Crea una copia del file "george" chiamata "david").
* **Copia in altra directory:** `cp david /tmp/` (Copia il file nella cartella temporanea).

### Rimuovere File (`rm`)
Il comando `rm` (remove) elimina definitivamente i file. **Attenzione:** in Linux non esiste un cestino per la riga di comando.
* **Sintassi:** `rm [nome_file]`
* **Esempio:** `rm apoho`

### Spostare e Rinominare (`mv`)
Il comando `mv` (move) serve a due scopi: spostare un file in un'altra posizione o rinominarlo.
* **Rinominare:** `mv lex luther` (Cambia il nome da "lex" a "luther").
* **Spostare:** `mv puddy /tmp/` (Sposta il file nella cartella `/tmp`).

---

## 2. Gestione delle Directory

### Creare Directory (`mkdir`)
Utilizzato per creare nuove cartelle.
* **Sintassi:** `mkdir [nome_directory]`
* **Esempio:** `mkdir gameofthrone`

### Rimuovere Directory (`rmdir` e `rm -r`)
Esistono due modi per eliminare una directory:
1. **`rmdir [nome]`**: Funziona solo se la directory è vuota.
2. **`rm -r [nome]`**: Rimuove la directory e tutto il suo contenuto (ricorsivo).



---

## 3. Proprietà e Permessi

Linux gestisce la sicurezza attraverso l'appartenenza dei file a utenti e gruppi specifici.

### Cambiare Gruppo (`chgrp`)
Cambia il gruppo proprietario di un file. Spesso richiede privilegi di root.
* **Esempio:** `chgrp root puddy`

### Cambiare Proprietario (`chown`)
Cambia l'utente proprietario del file.
* **Esempio:** `chown root puddy`

> **Tip:** È possibile cambiare sia utente che gruppo in un colpo solo con:
> `chown utente:gruppo nome_file`

---

## 4. Tabella Riassuntiva

| Comando | Azione | Note |
| :--- | :--- | :--- |
| **`cp`** | Copia | Mantiene l'originale. |
| **`rm`** | Rimuove | Azione irreversibile. |
| **`mv`** | Sposta/Rinomina | Non duplica il file. |
| **`mkdir`** | Crea cartella | Make Directory. |
| **`rmdir`** | Rimuove cartella | Solo se vuota. |
| **`chgrp`** | Cambia Gruppo | Change Group. |
| **`chown`** | Cambia Proprietario | Change Owner. |

---

## 5. Script Bash di Riepilogo

```bash
# --- ESEMPIO PRATICO MANUTENZIONE FILE ---

# 1. Creazione file e directory
touch george
mkdir test_folder

# 2. Copia e Rinomina
cp george david
mv david puddy

# 3. Spostamento
mv puddy test_folder/

# 4. Cambio proprietà (richiede sudo se non sei proprietario)
# sudo chown root:root test_folder/puddy

# 5. Pulizia (Rimozione ricorsiva della directory creata)
rm -rf test_folder
rm george

echo "Operazioni completate con successo."
```

# Documentazione Tecnica: Comandi per la Visualizzazione di File

Questa lezione analizza i comandi fondamentali per ispezionare il contenuto dei file di testo in Linux, variando dalla visualizzazione completa a quella parziale o paginata.

---

## 1. Visualizzazione Completa e Paginata

### Il comando `cat`
Il comando **`cat`** (concatenate) stampa l'intero contenuto di un file sullo standard output.
* **Uso:** Ideale per file brevi.
* **Limite:** Se il file è molto lungo, il testo scorrerà velocemente fino alla fine, rendendo difficile la lettura delle prime righe.

### I comandi `more` e `less`
Questi comandi permettono la visualizzazione "un'unica pagina alla volta".
* **`more`**: Utility classica. Indica la percentuale di lettura in fondo. Si usa la `Barra Spaziatrice` per scendere.
* **`less`**: Più potente di `more`. Permette di navigare sia in avanti che all'indietro.
    * `Freccia Su/Giù` o `J/K` per muoversi riga per riga.
    * `Barra Spaziatrice` per muoversi pagina per pagina.
    * `Q` per uscire dalla visualizzazione.



---

## 2. Visualizzazione Parziale (Top & Bottom)

Spesso, specialmente con i file di log che contengono milioni di righe, è necessario visualizzare solo l'inizio o la fine del documento.

### Il comando `head`
Visualizza le prime righe di un file (di default le prime 10).
* **Sintassi:** `head -n [nome_file]` dove `n` è il numero di righe desiderate.
* **Esempio:** `head -2 messages` (mostra solo le prime due righe).

### Il comando `tail`
Visualizza le ultime righe di un file (di default le ultime 10).
* **Sintassi:** `tail -n [nome_file]`.
* **Esempio:** `tail -5 messages` (mostra le ultime 5 righe).

---

## 3. Note sui permessi e Log di Sistema
Molti file di log, come `/var/log/messages` (che contiene informazioni sulla salute del sistema, errori e avvisi), sono leggibili solo dall'utente **root**. 
Per manipolarli o copiarli, è necessario elevare i propri privilegi:
1. Usare `su -` per diventare root.
2. Oppure usare `sudo` davanti ai comandi.



---

## 4. Tabella Riassuntiva

| Comando | Funzione Principale | Navigazione |
| :--- | :--- | :--- |
| **`cat`** | Mostra tutto il file | Nessuna (scorrimento totale) |
| **`more`** | Visualizzazione paginata | Solo in avanti |
| **`less`** | Visualizzazione avanzata | Avanti e indietro, riga per riga |
| **`head`** | Mostra l'inizio del file | Prime *n* righe |
| **`tail`** | Mostra la fine del file | Ultime *n* righe |

---

## 5. Esercizio di Riepilogo (Script Bash)

```bash
# --- ESEMPIO PRATICO VISUALIZZAZIONE FILE ---

# 1. Creazione di un file di test con più righe
echo -e "Riga 1: Inizio\nRiga 2\nRiga 3\nRiga 4\nRiga 5: Fine" > test_file.txt

# 2. Visualizzazione con cat
echo "--- Output CAT ---"
cat test_file.txt

# 3. Visualizzazione delle prime 2 righe
echo -e "\n--- Output HEAD (prime 2) ---"
head -2 test_file.txt

# 4. Visualizzazione delle ultime 2 righe
echo -e "\n--- Output TAIL (ultime 2) ---"
tail -2 test_file.txt

# 5. Esempio con file di sistema (richiede privilegi)
# sudo head -10 /var/log/messages | less
```

# Documentazione Tecnica: Filtri e Processori di Testo

Questa lezione introduce i "filtri", una categoria di comandi estremamente potenti che permettono di manipolare, filtrare e trasformare il testo. Questi strumenti rappresentano uno dei maggiori vantaggi competitivi di Linux nella gestione di dati e log.

---

## 1. Panoramica dei Comandi Principali

I filtri agiscono sullo standard input (o sul contenuto di un file) e restituiscono un output elaborato secondo regole specifiche.



* **`cut`**: Permette di "tagliare" e selezionare parti specifiche di ogni riga di un file o di un output.
* **`awk`**: Un processore di testo avanzato che lavora principalmente per colonne (campi). È quasi un linguaggio di programmazione a sé stante.
* **`grep` / `egrep`**: Strumenti di ricerca. Permettono di isolare solo le righe che contengono una specifica parola chiave o un pattern (es. cercare tutti i "Warning" in un log).
* **`sort`**: Ordina l'output in ordine alfabetico o numerico.
* **`uniq`**: Elimina le righe duplicate. Spesso usato in combinazione con `sort` poiché `uniq` confronta solitamente righe adiacenti.
* **`wc` (Word Count)**: Conta il numero di linee, parole e caratteri (byte) presenti in un file o ricevuti tramite pipe.

---

## 2. Tabella dei Filtri e Funzioni

| Comando | Funzione | Caso d'uso tipico |
| :--- | :--- | :--- |
| **`cut`** | Selezione campi | Estrarre solo i nomi utenti da un elenco. |
| **`awk`** | Elaborazione colonne | Stampare la prima e la terza colonna di una tabella. |
| **`grep`** | Ricerca stringhe | Trovare errori specifici nei log di sistema. |
| **`sort`** | Ordinamento | Mettere in ordine una lista di nomi non strutturata. |
| **`uniq`** | Rimozione duplicati | Pulire un file da voci ripetute. |
| **`wc`** | Conteggio | Sapere quante righe totali ha un file di configurazione. |

---

## 3. Filosofia dei Filtri
Questi comandi danno il meglio di sé quando vengono **concatenati tramite pipe (`|`)**. Ad esempio, è possibile leggere un file, cercare una parola, ordinare i risultati e infine contare quanti sono, tutto in un'unica riga di comando.

---

## 4. Script Bash di Esempio (Esercitazione Preliminare)

```bash
# --- INTRODUZIONE AI FILTRI ---

# 1. Creazione di un file disordinato con duplicati
echo -e "Zebra\nArancia\nZebra\nMela\nArancia" > frutti.txt

# 2. Utilizzo di SORT per ordinare
echo "--- Lista Ordinata ---"
sort frutti.txt

# 3. Utilizzo di SORT e UNIQ per rimuovere i duplicati
echo -e "\n--- Lista Unica (Senza duplicati) ---"
sort frutti.txt | uniq

# 4. Utilizzo di GREP per cercare una parola specifica
echo -e "\n--- Ricerca 'Zebra' ---"
grep "Zebra" frutti.txt

# 5. Utilizzo di WC per contare le righe totali
echo -e "\n--- Conteggio righe totali ---"
wc -l < frutti.txt
```

# Documentazione Tecnica: Il Comando `cut`

Il comando **`cut`** è un'utility a riga di comando fondamentale per il processamento del testo. Permette di estrarre sezioni specifiche (colonne, caratteri o byte) da ogni riga di un file o da dati ricevuti tramite pipe, stampando il risultato sullo standard output.

---

## 1. Informazioni Preliminari
Prima di utilizzare `cut`, è importante ricordare che non può essere eseguito senza opzioni.

* **Verifica Versione:** `cut --version`
* **Guida Rapida:** `cut --help` oppure `man cut`

### Preparazione ambiente
Per gli esempi successivi, si assume l'esistenza di un file chiamato `seinfeld-characters` con i nomi dei personaggi dello show (uno per riga).

---

## 2. Selezione per Caratteri (`-c`)

Questa opzione permette di estrarre caratteri specifici basandosi sulla loro posizione nella riga.



* **Singolo carattere:** `cut -c1 nome_file` (Estrae solo la prima lettera di ogni riga).
* **Caratteri multipli:** `cut -c1,3,5 nome_file` (Estrae il 1°, 3° e 5° carattere).
* **Range di caratteri:** `cut -c1-5 nome_file` (Estrae dal 1° al 5° carattere).
* **Range multipli:** `cut -c1-3,6-8 nome_file` (Estrae i caratteri dall'1 al 3 e dal 6 all'8).

---

## 3. Selezione per Delimitatore e Campi (`-d` e `-f`)

L'opzione più potente di `cut` permette di dividere la riga in "campi" (fields) utilizzando un carattere separatore chiamato **delimitatore**.

* **`-d`**: Specifica il delimitatore (es. `:`, `,`, `;`).
* **`-f`**: Specifica quale campo (colonna) estrarre.

**Esempio classico: il file `/etc/passwd`**
In Linux, il file degli utenti usa il due punti `:` come separatore. Per estrarre la cartella home degli utenti (6° campo):
`cut -d: -f6 /etc/passwd`

---

## 4. Selezione per Byte (`-b`)

Simile alla selezione per caratteri, l'opzione `-b` estrae byte specifici.
* **Esempio:** `cut -b1-3 nome_file`
* *Nota:* Per i file di testo standard (ASCII), i byte corrispondono ai caratteri, ma la differenza diventa rilevante con codifiche multibyte (come UTF-8).

---

## 5. Utilizzo con Pipe (`|`)

`cut` non lavora solo su file fisici, ma è perfetto per elaborare l'output di altri comandi.

**Esempio: estrarre i permessi dai risultati di `ls -l`**
`ls -l | cut -c2-4` (Estrae i bit relativi ai permessi dell'utente proprietario).

---

## 6. Script Bash di Riepilogo (Esercitazione)

```bash
# --- ESEMPIO PRATICO COMANDO CUT ---

# 1. Creazione file di test
echo "Jerry:Seinfeld:Attore" > characters.txt
echo "George:Costanza:Impiegato" >> characters.txt
echo "Elaine:Benes:Editor" >> characters.txt

# 2. Estrazione dei primi 3 caratteri di ogni riga
echo "--- Primi 3 caratteri ---"
cut -c1-3 characters.txt

# 3. Estrazione del primo campo usando il delimitatore ':' (Il nome)
echo -e "\n--- Estrazione Nomi (Campo 1) ---"
cut -d: -f1 characters.txt

# 4. Estrazione di più campi (Nome e Occupazione)
echo -e "\n--- Estrazione Nome e Occupazione (Campi 1 e 3) ---"
cut -d: -f1,3 characters.txt

# 5. Utilizzo in pipe per vedere i permessi della cartella corrente
echo -e "\n--- Permessi utente dai file correnti ---"
ls -l | cut -c2-4 | head -5
```

# Documentazione Tecnica: Il Comando `awk`

**`awk`** è un potente linguaggio di programmazione per l'elaborazione di testi e dati. A differenza di `cut`, che è limitato dalla posizione dei caratteri, `awk` analizza i file riga per riga, suddividendole in campi (colonne) in base a uno spazio vuoto o a un delimitatore specifico.

---

## 1. Struttura dei Campi in `awk`

`awk` assegna automaticamente una variabile a ogni colonna trovata nella riga:
* **`$1`**: Primo campo (es. Nome).
* **`$2`**: Secondo campo (es. Cognome).
* **`$n`**: n-esimo campo.
* **`$0`**: L'intera riga di testo.
* **`NF`**: Variabile che contiene il numero totale di campi nella riga corrente.
* **`$NF`**: Riferimento all'ultimo campo della riga.



---

## 2. Comandi Fondamentali ed Esempi

### Estrazione di Colonne
Se abbiamo un file `seinfeld-characters` con "Nome Cognome":
* **Estrarre solo il nome:** `awk '{print $1}' seinfeld-characters`
* **Estrarre solo il cognome:** `awk '{print $2}' seinfeld-characters`

### Utilizzo con l'Output di altri Comandi
`awk` è perfetto per filtrare i risultati di `ls -l`.
* **Esempio:** `ls -l | awk '{print $1, $3}'` (Mostra solo i permessi e il proprietario).
* **Ottenere l'ultima colonna senza contare:** `ls -l | awk '{print $NF}'`

### Filtri Avanzati e Ricerca
* **Ricerca (come grep):** `awk '/Jerry/ {print $0}' seinfeld-characters`
* **Filtro per lunghezza:** `awk 'length($0) > 15' seinfeld-characters` (Mostra solo le righe più lunghe di 15 caratteri).

---

## 3. Delimitatori e Sostituzioni

### Cambiare il Delimitatore (`-F`)
Per file con separatori diversi (es. i `:` nel file delle password):
`awk -F: '{print $1, $6}' /etc/passwd`

### Sostituzione "al volo"
Puoi modificare il valore di una colonna durante la stampa:
`echo "Hello Tom" | awk '{$2="Adam"; print $0}'`
*Output:* `Hello Adam`

---

## 4. Tabella Riassuntiva Variabili

| Variabile | Descrizione |
| :--- | :--- |
| **`$1 ... $n`** | Rappresenta la colonna specifica. |
| **`$0`** | Rappresenta l'intera riga. |
| **`NF`** | Numero di campi (colonne) nella riga attuale. |
| **`$NF`** | Il contenuto dell'ultima colonna. |
| **`-F`** | Opzione per definire un delimitatore personalizzato. |

---

## 5. Script Bash di Riepilogo (Esercitazione)

```bash
# --- ESEMPIO PRATICO COMANDO AWK ---

# 1. Creazione file di test
echo -e "Jerry Seinfeld 100\nGeorge Costanza 50\nElaine Benes 80" > punti.txt

# 2. Stampare Nome e Punteggio ($1 e $3)
echo "--- Classifica ---"
awk '{print $1, $3}' punti.txt

# 3. Trovare righe con punteggio superiore a 60
echo -e "\n--- Punteggi Alti ---"
awk '$3 > 60 {print $0}' punti.txt

# 4. Sostituire il cognome di George in tempo reale
echo -e "\n--- Modifica Cognome ---"
awk '/George/ {$2="TheLord"; print $0}' punti.txt

# 5. Contare quante colonne ha ls -l
echo -e "\n--- Analisi Colonne ls ---"
ls -l | head -n 2 | awk '{print "La riga ha " NF " colonne. L ultima è: " $NF}'
```

# Documentazione Tecnica: `grep` e `egrep`

Il comando **`grep`** (Global Regular Expression Print) è lo strumento di ricerca standard in Linux. Elabora il testo riga per riga e stampa ogni riga che corrisponde a un determinato pattern o parola chiave.

---

## 1. Comandi di Verifica e Aiuto
Prima di iniziare, è buona norma verificare l'ambiente di lavoro:
* **Versione:** `grep --version`
* **Manuale rapido:** `grep --help`
* **Manuale completo:** `man grep`

> **Nota dell'Istruttore:** In molti sistemi aziendali il prompt non mostra utente e hostname. Usa sempre `whoami`, `hostname` e `pwd` per orientarti prima di eseguire modifiche.

---

## 2. Opzioni Fondamentali di `grep`

L'uso di `grep` diventa estremamente potente grazie alle sue opzioni:

| Opzione | Funzione | Descrizione |
| :--- | :--- | :--- |
| **`-i`** | Ignore Case | Ignora la differenza tra maiuscole e minuscole (es. 'A' = 'a'). |
| **`-c`** | Count | Invece di mostrare le righe, restituisce il numero totale di occorrenze. |
| **`-n`** | Line Number | Mostra il contenuto della riga preceduto dal suo numero di riga nel file. |
| **`-v`** | Invert Match | Esclude le righe che contengono la parola chiave (mostra tutto il resto). |



---

## 3. Esempi Pratici

### Ricerca in un file
`grep -i "Seinfeld" seinfeld-characters`
*Cerca la parola "Seinfeld" ignorando il case nel file specificato.*

### Ricerca nell'output di un comando (Pipe)
`ls -l | grep -i "Desktop"`
*Filtra l'elenco dei file per mostrare solo la riga relativa alla cartella Desktop.*

### Combinazione di filtri
È possibile concatenare più comandi per raffinare la ricerca:
`grep -vi "Seinfeld" seinfeld-characters | awk '{print $1}' | cut -c1-3`
*Questo comando: esclude i Seinfeld, estrae il primo nome e ne mostra solo le prime 3 lettere.*

---

## 4. Ricerca Avanzata con `egrep`

Il comando **`egrep`** (Extended Grep) permette di utilizzare espressioni regolari estese. È particolarmente utile per cercare più parole contemporaneamente utilizzando l'operatore OR (`|`).

**Sintassi:**
`egrep -i "parola1|parola2" nome_file`

**Esempio:**
`egrep -i "Seinfeld|Costanza" seinfeld-characters`
*Estrae tutte le righe che contengono o "Seinfeld" o "Costanza".*

---

## 5. Script Bash di Riepilogo (Esercitazione)

```bash
# --- ESEMPIO PRATICO GREP ed EGREP ---

# 1. Ricerca semplice (Case Sensitive)
echo "--- Ricerca 'Jerry' ---"
grep "Jerry" seinfeld-characters

# 2. Ricerca con conteggio delle occorrenze
echo -e "\n--- Quanti 'Seinfeld' ci sono? ---"
grep -c "Seinfeld" seinfeld-characters

# 3. Ricerca con numero di riga
echo -e "\n--- Dove si trova 'George'? ---"
grep -n "George" seinfeld-characters

# 4. Esclusione (Mostra tutto tranne 'Jerry')
echo -e "\n--- Tutti tranne Jerry ---"
grep -v "Jerry" seinfeld-characters

# 5. Ricerca multipla con egrep
echo -e "\n--- Ricerca multipla (Seinfeld o Benes) ---"
egrep "Seinfeld|Benes" seinfeld-characters
```

