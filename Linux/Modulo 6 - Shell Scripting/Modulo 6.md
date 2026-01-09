### Il Kernel di Linux

In ogni distribuzione Linux (o più correttamente GNU/Linux), il **Kernel** costituisce il nucleo fondamentale del sistema. Esso agisce come un'interfaccia logica tra l'hardware fisico e il software applicativo.

#### 1. Architettura del Sistema
L'architettura di un sistema operativo può essere visualizzata come una serie di strati sovrapposti:

* **Hardware**: Rappresenta lo strato fisico alla base del sistema, composto da CPU, memoria RAM, dischi rigidi e periferiche esterne.
* **Kernel**: È un programma residente nel sistema operativo che rimane costantemente in esecuzione. La sua funzione principale è ricevere le istruzioni provenienti dalla shell e tradurle in operazioni comprensibili per l'hardware.
* **Shell**: È l'ambiente che riceve i comandi dall'utente. Può presentarsi come interfaccia grafica (**GUI**) o come interfaccia a riga di comando (**CLI**) tramite terminali che eseguono linguaggi come Bash o C-Shell.
* **Applicazioni**: Strato superiore che include software come browser, client di posta, calcolatrici e suite per l'ufficio.
* **Utenti**: Il livello finale, ovvero l'operatore umano che interagisce con le applicazioni.



#### 2. Definizione di Sistema Operativo
L'insieme della **Shell** e del **Kernel**, integrati in un unico pacchetto software, costituisce ciò che definiamo **Sistema Operativo** (come Linux, UNIX, macOS o Windows). 

Qualsiasi sistema operativo necessita del Kernel per poter comunicare con le componenti fisiche del computer. Mentre le applicazioni e la shell sono software scritti (spesso in linguaggio C) per facilitare l'interazione, il Kernel è il motore essenziale che gestisce le risorse hardware per conto di tutto il sistema.

---

### Introduzione alla Shell in Linux

La **Shell** è una componente fondamentale di un sistema operativo. Può essere immaginata come un "contenitore" che racchiude l'ambiente di lavoro, le informazioni e le configurazioni dell'utente, agendo da intermediario tra l'utente stesso e il **Kernel**.

#### 1. Funzionamento e Ruolo
La Shell fornisce la piattaforma necessaria per eseguire comandi. Esistono principalmente due tipi di interfacce Shell:
* **GUI (Interfaccia Grafica)**: Presente in sistemi come Windows o ambienti Linux come KDE. In questo caso, l'utente interagisce con la Shell tramite icone e finestre (es. doppio clic su un programma).
* **CLI (Interfaccia a Riga di Comando)**: Tipica degli ambienti server Linux, dove l'interazione avviene tramite un terminale o un prompt dei comandi.



#### 2. La Shell nell'Architettura del Sistema
Come illustrato nel diagramma delle gerarchie di sistema, il flusso di comunicazione segue questo ordine:
1.  L'**Applicazione** o l'**Utente** invia un comando alla **Shell**.
2.  La **Shell** interpreta il comando e lo trasmette al **Kernel**.
3.  Il **Kernel** traduce le istruzioni per l'**Hardware**, che esegue l'operazione fisica.

#### 3. Identificazione e Configurazione in Linux
In un sistema Linux, è possibile gestire e identificare la Shell utilizzata attraverso specifici comandi e file di configurazione:

* **Identificare la Shell corrente**: Eseguendo il comando `echo $0` nel terminale, il sistema restituisce il nome della Shell in uso (ad esempio, `bash`).
* **Elencare le Shell disponibili**: Il file `/etc/shells` contiene l'elenco di tutte le Shell installate nella distribuzione (es. `sh`, `bash`, `zsh`). Si può visualizzare con:
    ```bash
    cat /etc/shells
    ```
* **Shell assegnata all'utente**: Il tipo di Shell predefinita per ogni utente è specificato nell'ultima parte della riga corrispondente nel file `/etc/passwd`. Ad esempio:
    ```bash
    cat /etc/passwd | grep [nome_utente]
    ```
    L'output indicherà il percorso della Shell assegnata, come ad esempio `/bin/bash`.

Sebbene sia possibile cambiare la Shell assegnata, è prassi comune mantenere la coerenza con gli altri utenti del sistema per facilitare l'automazione tramite script.

---

### Tipologie di Shell in Ambiente Linux

In un sistema operativo Linux, sono disponibili diverse tipologie di shell che l'utente può utilizzare per interagire con il sistema. Queste si dividono principalmente in interfacce grafiche (GUI) e interfacce a riga di comando (CLI).

#### 1. Shell Grafiche (GUI)
Sebbene Linux sia noto per la sua riga di comando, è possibile installare e utilizzare ambienti grafici. Durante l'installazione del sistema, le due opzioni principali sono:
* **GNOME**: Un ambiente desktop grafico che permette di interagire con il sistema operativo tramite tastiera e mouse (clic, trascinamento, ecc.).
* **KDE**: Un'altra popolare interfaccia grafica che funge da shell per l'utente.

#### 2. Shell a Riga di Comando (CLI)
Quando il sistema viene utilizzato senza interfaccia grafica, l'utente interagisce tramite una shell testuale. La shell predefinita viene assegnata al momento della creazione dell'account utente ed è riportata nel file `/etc/passwd`.

Di seguito le principali shell testuali disponibili:

| Shell | Nome Esteso | Caratteristiche Principali |
| :--- | :--- | :--- |
| **sh** | Bourne Shell | Sviluppata da Stephen Bourne (AT&T Bell Labs) nel 1977. È una delle shell originali di Unix. Supporta ridirezione I/O, variabili e cicli. |
| **bash** | Bourne Again Shell | Evoluzione compatibile della shell `sh`. È la shell predefinita nella maggior parte delle distribuzioni Linux grazie alla sua flessibilità e facilità d'uso. |
| **csh / tcsh** | C Shell | Sviluppata da Bill Joy (Università di Berkeley), utilizza una sintassi simile ai linguaggi C e C++. La versione `tcsh` è un'evoluzione migliorata di `csh`. |
| **ksh** | Korn Shell | Sviluppata da David Korn, è compatibile con `sh` e `bash`. Introduce funzionalità avanzate come il controllo dei job e l'aritmetica in virgola mobile. |

#### 3. Considerazioni sull'uso
È fondamentale conoscere la differenza tra le shell per i seguenti motivi:
* **Compatibilità degli Script**: Uno script scritto per `bash` potrebbe non funzionare correttamente in `tcsh`, poiché le sintassi differiscono sensibilmente.
* **Ambienti Specifici**: Ad esempio, la `KornShell` è molto diffusa in ambienti Unix Solaris, mentre `bash` è lo standard de facto nel mondo Linux.
* **Verifica del Sistema**: Per visualizzare l'elenco completo delle shell installate sul proprio sistema, è possibile utilizzare il comando:
    ```bash
    cat /etc/shells
    ```

---

### Introduzione allo Shell Scripting

Lo **Shell Scripting** consiste nella creazione di file eseguibili contenenti una serie di comandi che vengono elaborati in sequenza dal sistema. Rappresenta uno strumento fondamentale per l'automazione dei compiti (task) in ambiente Linux.

#### 1. Struttura di uno Shell Script
Un file di script segue un ordine di esecuzione sequenziale: il primo comando viene eseguito per primo, seguito dal secondo, dal terzo e così via. Gli elementi principali di uno script sono:

* **Shebang**: La prima riga dello script deve sempre definire quale shell utilizzerà il sistema per interpretare i comandi. La sintassi standard per la Bash è:
    ```bash
    #!/bin/bash
    ```
    *(Nota: Il simbolo `!` viene comunemente chiamato "bang" in ambito Linux).*
* **Commenti**: Utilizzati per descrivere il funzionamento dello script. Iniziano sempre con il simbolo cancelletto (`#`) e vengono ignorati dal sistema durante l'esecuzione.
* **Comandi**: Le istruzioni effettive che lo script deve eseguire.
* **Istruzioni Condizionali e Cicli**: Strutture logiche (come `if`, `then`, `while`, `for`) che permettono di stabilire condizioni e ripetizioni.

#### 2. Requisiti per l'Esecuzione
Affinché un file di testo diventi uno script funzionante, devono essere soddisfatti alcuni criteri:

1.  **Permessi di Esecuzione**: In Linux, ogni nuovo file è inizialmente un semplice file di testo. Per renderlo uno script, è necessario assegnargli il permesso di esecuzione (`x`). Senza questo bit attivo, il sistema non potrà avviare il processo.
2.  **Percorso di Esecuzione**: Uno script può essere richiamato in due modi:
    * **Percorso Assoluto**: Definendo il percorso completo (es: `/home/utente/mio_script.sh`).
    * **Percorso Relativo**: Se ci si trova nella stessa directory del file, si utilizza la sintassi:
        ```bash
        ./nome_script.sh
        ```

---

### Primi Passi con lo Shell Scripting

Lo scripting permette di automatizzare i comandi quotidiani per ottenere risultati specifici in modo rapido ed efficiente. In questa fase, vengono illustrati i passaggi per creare, configurare ed eseguire i primi script personalizzati.

#### 1. Preparazione dell'Ambiente
Prima di iniziare, è buona norma organizzare il proprio spazio di lavoro:
1.  Verificare la posizione attuale con il comando `pwd`.
2.  Creare una directory dedicata ai propri file: `mkdir myscripts`.
3.  Accedere alla directory: `cd myscripts`.

#### 2. Esempio 1: Output a Schermo (Hello World)
Per creare uno script che mostri un messaggio a video, si utilizza l'editor di testo `vi`:

```bash
vi output-screen
```

All'interno del file, inserire le seguenti righe:

```bash
#!/bin/bash
# Questo script mostra un messaggio a schermo
echo "Hello World"
```

#### 3. Gestione dei Permessi ed Esecuzione
In Linux, un nuovo file creato con un editor di testo non possiede automaticamente i diritti di esecuzione. Se si tenta di avviare lo script con il comando `./output-screen`, il sistema restituirà l'errore **"Permission denied"** (permesso negato). 

Per rendere il file operativo, è necessario modificare i suoi permessi:

* **Assegnazione dei permessi**: `chmod +x output-screen` (aggiunge il bit di esecuzione).
* **Verifica**: Utilizzando `ls -ltr`, il file apparirà solitamente di un colore diverso (spesso verde), indicando che è ora eseguibile.
* **Esecuzione (percorso relativo)**: `./output-screen` (utilizzato quando ci si trova nella stessa directory del file).
* **Esecuzione (percorso assoluto)**: `/home/[nome_utente]/myscripts/output-screen` (utilizzato per avviare lo script da qualsiasi posizione nel sistema).

#### 4. Esempio 2: Automazione di Comandi Multipli
Uno script può raggruppare diversi comandi di sistema per eseguire task sequenziali e fornire un report rapido.

```bash
#!/bin/bash
# Descrizione: Script per la visualizzazione di informazioni di sistema
# Autore: Imran Afzal

whoami      # Mostra l'utente attivo
echo ""     # Inserisce una riga vuota per migliorare la leggibilità
pwd         # Restituisce la posizione attuale nel file system
hostname    # Mostra il nome host della macchina
ls -ltr     # Elenca i file con dettagli e ordinamento temporale
```

#### 5. Esempio 3: Utilizzo delle Variabili e Troubleshooting

Le variabili permettono di memorizzare dati all'interno dello script per riutilizzarli tramite il simbolo `$`.

```bash
#!/bin/bash
# Definizione delle variabili
a="Imran"
b="Afzal"
c='Linux Class' # L'uso degli apici è fondamentale in presenza di spazi

echo "Il mio nome è $a"
echo "Il mio cognome è $b"
echo "La mia classe è $c"
```
Nota sul Troubleshooting: 
Qualora lo script non dovesse funzionare correttamente, la shell indicherà la riga esatta in cui è presente il problema (ad esempio: Line 6: command not found). 
Un errore comune riguarda la gestione degli spazi: se una variabile contiene più parole, è necessario racchiuderne il valore tra apici singoli (' ') affinché il sistema non interpreti la seconda parola come un comando a sé stante.


### Gestione dell'Input e dell'Output negli Script

In questa lezione viene analizzato come creare uno script interattivo capace di attendere un input dall'utente, memorizzarlo e riutilizzarlo per generare un output personalizzato.

#### 1. I Comandi Fondamentali
Per gestire l'interazione con l'utente, si utilizzano due comandi principali:
* **`read`**: Sospende l'esecuzione dello script finché l'utente non digita qualcosa e preme Invio. L'input viene salvato in una variabile (o "contenitore").
* **`echo`**: Visualizza testo o il contenuto delle variabili sullo schermo.



#### 2. Esempio Pratico: Script Interattivo
Si consideri la creazione di un file denominato `input-script`. Di seguito è riportata la struttura per richiedere il nome dell'utente:

```bash
#!/bin/bash
# Script per testare l'input dell'utente

echo "Ciao, il mio nome è Imran Afzal."
echo "Qual è il tuo nome?"

# Il sistema attende l'input e lo salva nella variabile 'nome_utente'
read nome_utente

echo ""
echo "Piacere di conoscerti, $nome_utente!"
```

Durante l'esecuzione, lo script non restituirà il prompt della shell finché l'utente non avrà fornito una risposta, poiché il comando `read` mantiene il processo in attesa.



#### 3. Integrazione di Comandi di Sistema nelle Variabili
È possibile assegnare l'output di un comando di sistema a una variabile utilizzando i **backtick** (accenti gravi: `` ` ``), situati solitamente sotto il tasto ESC o ottenibili con combinazioni di tasti specifiche.

```bash
#!/bin/bash

# Assegnazione dell'output del comando 'hostname' alla variabile A
A=`hostname`

echo "Il nome di questo server è: $A"
echo "Inserisci il tuo nome:"
read B

echo "Benvenuto $B sul server $A."
```

> **Importante**: Se si omettono i backtick (es. `A=hostname`), la variabile memorizzerà la parola letterale "hostname" anziché il risultato dell'esecuzione del comando.



#### 4. Creazione di un "Wizard" Multiplo
Lo scripting può essere utilizzato per raccogliere più informazioni sequenzialmente, simulando una vera e propria procedura guidata (wizard):

```bash
#!/bin/bash

# Raccolta dati sequenziale
echo "Qual è la tua professione?"
read professione

echo "Qual è il tuo show preferito?"
read show

# Elaborazione e output finale
echo "--- Riepilogo Informazioni ---"
echo "Lavorare come $professione è un'ottima scelta."
echo "Concordo, $show è un programma molto interessante."
```

#### 5. Applicazioni Comuni e Troubleshooting
Questo modello di "input-elaborazione-output" è alla base di molti strumenti di amministrazione del sistema, permettendo di trasformare semplici script in veri e propri assistenti interattivi.

* **Procedure di installazione**: Richiesta di conferma (S/N) o inserimento di percorsi di directory specifici per il deployment di software.
* **Script di configurazione**: Raccolta di parametri di rete, nomi utente o impostazioni di database per la creazione automatizzata di account o servizi.
* **Gestione degli errori**: Durante lo sviluppo, se lo script non produce l'output atteso, è fondamentale verificare che i nomi delle variabili definiti nel comando `read` corrispondano esattamente a quelli richiamati successivamente con il simbolo `$`.



Inoltre, lo scripting interattivo funge da interfaccia semplificata per processi complessi che avvengono "dietro le quinte", come l'invio di dati a database o la generazione di email automatiche basate sulle risposte fornite.

---

### Scripting Avanzato: Le Istruzioni Condizionali If-Then

Le istruzioni condizionali permettono di elevare il livello di complessità e utilità degli script. Attraverso la logica **if-then-else** (se-allora-altrimenti), è possibile definire condizioni specifiche: se una condizione è soddisfatta, lo script esegue un'azione; in caso contrario, ne esegue una differente.



#### 1. Sintassi Fondamentale: Esempio con Variabili Numeriche
Un esempio classico consiste nel verificare se il valore di una variabile corrisponde a un numero specifico.

```bash
#!/bin/bash
# Definizione della variabile
count=100

if [ $count -eq 100 ]
then
    echo "Il conteggio è 100."
else
    echo "Il conteggio non è 100."
fi
```

* **if**: Inizia la condizione.
* **then**: Indica l'azione da compiere se la condizione è vera.
* **else**: (Opzionale) Indica l'azione da compiere se la condizione è falsa.
* **fi**: Segna la chiusura del blocco condizionale (è la parola "if" scritta al contrario).



#### 2. Verifica dell'Esistenza di un File
Una delle applicazioni più comuni per gli amministratori di sistema è il controllo della presenza di file (come file di log o di errore) per monitorare lo stato delle applicazioni.

L'opzione `-e` viene utilizzata all'interno delle parentesi quadre per verificare l'esistenza di un percorso specifico nel file system.

```bash
#!/bin/bash
clear # Pulisce lo schermo prima dell'esecuzione

if [ -e /home/utente/error.txt ]
then
    echo "Il file esiste."
else
    echo "Il file non esiste."
fi
```

#### 3. Utilità Pratica e Automazione
L'impiego delle istruzioni condizionali trasforma semplici elenchi di comandi in strumenti decisionali complessi, essenziali per la gestione dei sistemi:

* **Integrazione con CronJob**: Automatizzazione di controlli periodici (es. ogni ora) per verificare lo stato di salute del sistema.
* **Monitoraggio delle Applicazioni**: Rilevamento automatico di file di errore generati dai software; se il file esiste, lo script può inviare notifiche o riavviare servizi.
* **Script Wizard e Interattivi**: Sviluppo di interfacce che guidano l'utente attraverso scelte multiple (es. "Vuoi procedere con l'installazione? [s/n]").



#### 4. Troubleshooting e Best Practices
Per garantire il corretto funzionamento degli script `if-then`, è necessario prestare attenzione ad alcuni dettagli tecnici critici:

* **Sintassi delle Parentesi**: È obbligatorio inserire uno spazio tra le parentesi quadre e l'espressione interna (es. `[ $count -eq 100 ]`). L'assenza di spazi è uno degli errori più comuni.
* **Struttura di Chiusura**: Ogni blocco `if` deve terminare con `fi`. In caso contrario, lo script restituirà un errore di sintassi "unexpected end of file".
* **Permessi di Esecuzione**: Prima dell'avvio, assicurarsi di aver assegnato i privilegi necessari con il comando `chmod +x nome_script.sh`.
* **Uso dei Ticks**: Quando si assegna l'output di un comando a una variabile, utilizzare i backtick (`` `comando` ``) per l'esecuzione corretta.

---

### Scripting Avanzato: I Cicli For

I cicli **For** rappresentano un livello avanzato di scripting. Questa struttura permette di automatizzare compiti ripetitivi eseguendo un blocco di comandi per un numero specificato di volte o per ogni elemento contenuto in una lista.



#### 1. Funzionamento del Ciclo For
Un ciclo `for` continua a girare finché non esaurisce gli elementi definiti in una variabile o in un elenco.
* Se si definisce un intervallo da 1 a 10, lo script verrà eseguito 10 volte.
* Se la lista contiene i colori "verde, blu, rosso", lo script verrà eseguito tre volte, una per ogni colore.

Questo approccio semplifica drasticamente il lavoro: se è necessario creare 100 file, non serve digitare il comando 100 volte; il ciclo `for` lo farà automaticamente.

#### 2. Esempio 1: Output Numerico Iterativo
Si consideri uno script chiamato `abc.sh` per visualizzare un messaggio di benvenuto ripetuto:

```bash
#!/bin/bash
# Esempio di ciclo for numerico semplice

for i in 1 2 3 4 5
do
    echo "Benvenuto per la volta numero $i"
done
```

* **for i in ...**: Definisce la variabile `i` come un "contenitore" che ospita i numeri da 1 a 5.
* **do**: Inizia l'azione da ripetere.
* **done**: Indica la fine del ciclo. Una volta raggiunto l'ultimo elemento, lo script termina.



#### 3. Esempio 2: Iterazione su una Lista di Parole
I cicli possono operare anche su stringhe di testo. In questo esempio, lo script `xyz.sh` associa diverse azioni a un soggetto:

```bash
#!/bin/bash
# Autore: Imran Afzal
# Descrizione: Questo script mostra diverse azioni in sequenza

for i in mangia corre salta gioca
do
    echo "Guarda Imran che $i"
done
```

In questo caso, la variabile $i assume di volta in volta il valore delle parole "mangia", "corre", "salta" e "gioca", visualizzandole a schermo in sequenza.

#### 4. Utilità nel Mondo Reale
In un ambiente aziendale, lo scripting dei cicli è fondamentale per trasformare ore di lavoro manuale in pochi secondi di esecuzione automatizzata. Alcuni casi d'uso includono:

* **Gestione massiva di file**: Rinominare, spostare o eliminare centinaia di file contemporaneamente (es. file di log vecchi).
* **Amministrazione Utenti**: Automatizzare la creazione di account multipli partendo da un elenco di nomi predefinito.
* **Manutenzione del sistema**: Eseguire backup o rotazione dei log su diverse directory in un unico passaggio, garantendo che nessuna cartella venga dimenticata.



#### 5. Troubleshooting Comuni
Per evitare errori durante l'esecuzione dei cicli, è utile tenere a mente quanto segue:

* **No such file or directory**: Questo errore si verifica spesso quando si tenta di avviare lo script digitando il nome in modo errato (es. dimenticando l'estensione o scambiando le lettere come `xzy` invece di `xyz`).
* **Permessi negati**: Se lo script non si avvia, è quasi certamente dovuto alla mancanza del bit di esecuzione. Il comando `chmod +x nome_script` risolve il problema.
* **Cicli Infiniti**: Se la lista di variabili non è definita correttamente, lo script potrebbe non fermarsi mai o non partire affatto. Verificare sempre che la sintassi `do` e `done` racchiuda correttamente il blocco di codice.

---

### Scripting Avanzato: I Cicli Do-While

I cicli **Do-While** presentano forti analogie con i cicli `for`, ma si distinguono per la loro logica di esecuzione: un blocco di comandi viene eseguito continuamente **finché una determinata condizione rimane vera** o viene soddisfatta.



#### 1. Concetto e Applicazioni
Questa struttura è ideale per processi che richiedono un monitoraggio costante o un'esecuzione prolungata nel tempo. Alcuni esempi includono:
* **Daemon e Processi di Background**: Servizi di sistema che devono rimanere attivi "per sempre" o finché non si verifica un evento specifico.
* **Timer e Scadenze**: Eseguire uno script fino a un orario prestabilito (es. "continua l'esecuzione fino alle 14:00, poi esci").
* **Conto alla rovescia**: Eseguire un'operazione per un numero definito di secondi prima di arrestare un servizio.

#### 2. Sintassi ed Esempio Pratico: Conto alla Rovescia
Si consideri uno script denominato `script-dowhile` progettato per simulare l'arresto di un processo dopo un intervallo di tempo.

```bash
#!/bin/bash
# Descrizione: Script di esempio do-while per il countdown di arresto
# Autore: Imran Afzal

count=0
num=10

while [ $count -lt 10 ]
do
    echo "Secondi rimanenti all'arresto del processo: $((num - count))"
    sleep 1 # Sospende l'esecuzione per 1 secondo
    ((count++))
done

echo "Processo arrestato con successo."
```
* **while [ condizione ]**: Verifica se la condizione è vera (nell'esempio, se `count` è minore di 10).
* **do**: Esegue i comandi contenuti nel blocco (echo, sleep, incremento).
* **done**: Conclude il blocco. Quando la condizione diventa falsa (ovvero quando `count` raggiunge 10), il ciclo termina e lo script prosegue oltre.



#### 3. Analisi dei Comandi Utilizzati
* **`sleep`**: Un comando fondamentale che mette in pausa lo script per il numero di secondi specificato. È essenziale nei cicli per gestire le tempistiche ed evitare un consumo eccessivo di risorse della CPU.
* **Espressioni aritmetiche**: Come `((count++))`, permettono di aggiornare il valore della variabile a ogni iterazione del ciclo. Senza un meccanismo di incremento o modifica della variabile di controllo, lo script non raggiungerebbe mai la condizione di uscita.

#### 4. Troubleshooting e Consigli
* **Cicli Infiniti**: Se la condizione all'interno di `while` resta sempre vera (ad esempio, se si dimentica di incrementare la variabile di controllo), lo script continuerà a girare senza fine. In questi casi, è possibile forzare l'interruzione manuale premendo **`CTRL+C`**.
* **Utilizzo come Daemon**: Molti processi di sistema (chiamati "demoni") utilizzano questa logica per restare costantemente in esecuzione e monitorare eventi o file in background.
* **Esercitazione**: Per acquisire piena dimestichezza, si consiglia di testare diverse varianti dello script modificando i tempi di `sleep` o i parametri della condizione. La pratica con almeno 5 o 6 esempi differenti aiuterà a comprendere meglio il flusso logico del comando.

* ### Scripting Avanzato: L'Istruzione Case

L'istruzione **Case** permette di creare script interattivi fornendo all'utente un menu di opzioni tra cui scegliere. È una struttura logica che semplifica la gestione di molteplici condizioni: in base all'opzione selezionata (A, B, C, ecc.), lo script esegue un'azione specifica associata a quella variabile.



#### 1. Struttura e Funzionamento
Il comando `case` confronta una variabile con diversi modelli (pattern). Quando viene trovata una corrispondenza, vengono eseguiti i comandi associati fino al simbolo di chiusura del caso (`;;`).

**Esempio di menu interattivo:**
Lo script visualizza un elenco di scelte e utilizza il comando `read` per acquisire la selezione dell'utente.

```bash
#!/bin/bash
# Script per la gestione di un menu tramite istruzione CASE

echo ""
echo "Scegli una delle opzioni seguenti:"
echo "a = Visualizza data e ora"
echo "b = Elenca file e directory"
echo "c = Elenca utenti connessi"
echo "d = Verifica uptime del sistema"
echo ""

read scelta

case $scelta in
    a) date;;
    b) ls;;
    c) who;;
    d) uptime;;
    *) echo "Scelta non valida - Arrivederci";;
esac
```

* **`case $variabile in`**: Inizia l'analisi del valore contenuto nella variabile.
* **`a)`**: Definisce il modello (pattern) da confrontare; se il valore è "a", esegue il comando successivo.
* **`;;`**: Termina l'azione per quella specifica opzione (fondamentale per separare i casi).
* **`*)`**: Rappresenta il caso predefinito (wildcard). Viene eseguito se l'input dell'utente non corrisponde a nessuna delle opzioni elencate (es. se viene digitata la lettera "F").
* **`esac`**: Chiude l'istruzione case (è la parola "case" scritta al contrario).



#### 2. Automazione e Interattività
L'utilità principale dell'istruzione `case` è la semplificazione di task amministrativi. Invece di costringere l'operatore a digitare comandi Linux complessi ogni volta, lo script funge da interfaccia amichevole:
* **Esecuzione facilitata**: Basta una lettera per richiamare comandi come `uptime`, `ls` o `who`.
* **Riduzione errori**: L'utente è guidato tra opzioni predefinite, riducendo il rischio di digitare comandi errati.
* **Flessibilità**: È possibile associare qualsiasi comando di sistema o funzione personalizzata a ogni lettera del menu.

#### 3. Troubleshooting e Note Tecniche
* **Permessi**: Lo script non funzionerà finché non vengono assegnati i permessi di esecuzione tramite `chmod +x nome_script.sh`.
* **Case Sensitivity**: Bash distingue tra maiuscole e minuscole. Se lo script prevede `a)` e l'utente digita `A`, verrà attivata l'opzione di default `*)` (Scelta non valida).
* **Gestione degli errori**: L'uso del simbolo `*` alla fine è una best practice fondamentale per gestire input imprevisti senza far crashare lo script.



---

### Scripting Professionale: Monitoraggio della Connettività di Rete

In infrastrutture IT di grandi dimensioni, è fondamentale verificare costantemente se i server remoti sono raggiungibili. Sebbene esistano soluzioni di monitoraggio avanzate, saper creare uno script personalizzato per il controllo della connettività tramite il comando `ping` è una competenza essenziale per ogni amministratore di sistema.



#### 1. Logica dello Script di Ping
Uno script di monitoraggio della rete combina istruzioni condizionali (`if-else`) e cicli (`for`) per determinare lo stato di un host. Il cuore dello script si basa sull'**exit status** dell'ultimo comando eseguito, rappresentato dalla variabile speciale `$?`:
* **Status 0**: Il comando è stato eseguito con successo (l'host risponde al ping).
* **Status diverso da 0**: Si è verificato un errore (l'host è irraggiungibile).

#### 2. Esempio 1: Controllo di un Singolo Host
In questo esempio, si verifica la connettività di un singolo indirizzo IP (ad esempio, il gateway predefinito `192.168.1.1`).

```bash
#!/bin/bash
# Descrizione: Verifica la connettività verso un singolo host remoto

HOST="192.168.1.1"

# -c1 indica di inviare un solo pacchetto di ping
# &> /dev/null reindirizza l'output in un "buco nero" per pulire la schermata
ping -c1 $HOST &> /dev/null

if [ $? -eq 0 ]
then
    echo "L'host $HOST è raggiungibile (OK)"
else
    echo "L'host $HOST non risponde (NOT OK)"
fi
```

#### 3. Esempio 2: Monitoraggio Massivo (Multi-Host)
Per monitorare decine o centinaia di server, è inefficiente inserire gli indirizzi IP manualmente nel codice. La soluzione professionale consiste nell'utilizzare un file di testo esterno (ad esempio `myhosts`) contenente l'elenco degli IP o dei nomi host da verificare.



**Script per il monitoraggio multiplo:**
```bash
#!/bin/bash
# Descrizione: Scansiona una lista di host e ne verifica la connettività
# Autore: Imran Afzal

# Definizione della variabile con il percorso del file contenente gli IP
LISTA_HOST="/home/iafzal/ps/myhosts"

for IP in $(cat $LISTA_HOST)
do
    # Esegue un singolo ping (-c1) e nasconde l'output standard
    ping -c1 $IP &> /dev/null
    
    if [ $? -eq 0 ]
    then
        echo "L'host $IP è raggiungibile (OK)"
    else
        echo "L'host $IP non risponde (NOT OK)"
    fi
done
```

* **`for IP in $(cat $LISTA_HOST)`**: Questo ciclo legge ogni riga del file esterno e assegna temporaneamente l'indirizzo IP alla variabile `$IP`. Il comando `cat` viene eseguito all'interno della sostituzione di comando per fornire la lista al ciclo.
* **`do ... done`**: Racchiude il blocco di istruzioni (`ping` e `if-else`) che verranno ripetute per ogni server presente nell'elenco.
* **Scalabilità**: Grazie a questa struttura, non è necessario modificare lo script per aggiungere nuovi server; è sufficiente aggiornare il file di testo `myhosts`.



#### 4. Best Practices e Troubleshooting
* **Silenziare l'output**: L'operatore `&> /dev/null` è essenziale negli script professionali. Reindirizza sia l'output standard che gli errori in un dispositivo nullo, permettendo all'utente di vedere solo i messaggi personalizzati (`echo`) anziché il dettaglio tecnico del ping.
* **Gestione dei tempi**: Il comando `ping` attende per impostazione predefinita una risposta per diversi secondi. Se un host è offline, lo script potrebbe apparire "bloccato"; è possibile aggiungere l'opzione `-W 1` per limitare il tempo di attesa (timeout) a un solo secondo.
* **Pianificazione (CronJob)**: Questi script raggiungono il massimo della loro utilità quando vengono pianificati. Ad esempio, è possibile impostare il sistema affinché esegua il controllo ogni 5 minuti in totale autonomia.

---
```bash
#!/bin/bash

ping -c1 192.168.1.1
        if [ $? -eq 0 ]
        then
        echo OK
        else
        echo NOT OK
        fi

Change the IP to 192.168.1.235


Don't show the output
ping -c1 192.168.1.1 &> /dev/null
        if [ $? -eq 0 ]
        then
        echo OK
        else
        echo NOT OK
        fi


Define variable
#!/bin/bash

hosts="192.168.1.1"
ping -c1 $hosts &> /dev/null
        if [ $? -eq 0 ]
        then
        echo $hosts OK
        else
        echo $hosts NOT OK
        fi

Change the IP to 192.168.1.235




Multiple IPs
#!/bin/bash

IPLIST="path_to_the_Ip_list_file"


for ip in $(cat $IPLIST)

do
   ping -c1 $ip &> /dev/null
   if [ $? -eq 0 ]
   then
   echo $ip ping passed
   else
   echo $ip ping failed
   fi
done
```

### Cosa sono gli Alias in Linux?

In informatica, proprio come nella vita reale, un **alias** è un soprannome o un nome abbreviato che viene utilizzato al posto di uno più lungo o complesso. L'obiettivo principale è rendere i comandi più facili da ricordare e da digitare, riducendo il tempo speso in operazioni ripetitive e tediose.

In Linux, il comando `alias` viene utilizzato per creare scorciatoie personalizzate per stringhe di comandi lunghe o complesse.

---

#### 1. Esempi Pratici di Alias
Di seguito vengono analizzati alcuni casi d'uso comuni per semplificare l'interazione con il terminale.

* **Abbreviare i parametri di visualizzazione**:
    Spesso si desidera vedere l'elenco completo dei file, inclusi quelli nascosti, con dettagli estesi (`ls -al`). È possibile creare una scorciatoia chiamata `l`:
    ```bash
    alias l="ls -al"
    ```
* **Combinare più comandi (Semicolon)**:
    Si possono eseguire due o più comandi in sequenza separandoli con il punto e virgola. Ad esempio, per stampare la cartella corrente (`pwd`) e subito dopo elencarne i file (`ls`), si può creare l'alias `pl`:
    ```bash
    alias pl="pwd; ls"
    ```
* **Filtrare l'output (Grep)**:
    Per visualizzare solo le directory presenti nella cartella corrente, si può filtrare l'output di `ls -l` cercando le righe che iniziano con la lettera "d":
    ```bash
    alias dir="ls -l | grep ^d"
    ```

#### 2. Gestione dei Caratteri Speciali
Quando si creano alias che contengono variabili o simboli speciali (come `$`), è necessario utilizzare il carattere di escape (il backslash `\`) per evitare che la shell interpreti la variabile nel momento sbagliato.

**Esempio complesso**: Estrarre solo i nomi delle partizioni dall'output di `df -h`.
```bash
alias d="df -h | awk '{print \$6}' | cut -c1-4"
```

> **Nota**: Il simbolo `\$` assicura che il comando `awk` riceva correttamente il parametro `$6` senza che la shell lo interpreti preventivamente come una variabile d'ambiente (che risulterebbe vuota), permettendo così ad `awk` di processare correttamente la sesta colonna dell'output.

---

#### 3. Visualizzazione e Rimozione
Una volta definiti, gli alias possono essere gestiti facilmente attraverso i seguenti strumenti:

| Comando | Descrizione |
| :--- | :--- |
| **`alias`** | Visualizza l'elenco completo degli alias attualmente attivi nella sessione. |
| **`unalias [nome]`** | Elimina un alias specifico (ad esempio: `unalias d`). |

---

#### 4. Considerazioni Tecniche e Persistenza
* **Natura Temporanea**: Gli alias creati direttamente nella riga di comando sono volatili. Ciò significa che verranno eliminati automaticamente alla chiusura del terminale o della sessione SSH.
* **Rendere gli Alias Permanenti**: Per conservare gli alias, è necessario trascriverli nel file di configurazione del profilo utente, solitamente il file `~/.bashrc` (o `~/.bash_profile`), in modo che vengano caricati a ogni nuovo accesso.
* **Produttività**: L'utilizzo strategico degli alias è un segno distintivo di un amministratore di sistema esperto, poiché riduce drasticamente il rischio di errori di battitura in comandi critici e accelera il flusso di lavoro quotidiano.

---

### Gestione degli Alias Utente e Globali in Linux

La creazione di un alias tramite riga di comando è un'operazione temporanea: non appena viene terminata la sessione (logout dalla console o chiusura di una connessione PuTTY), l'alias viene eliminato. Per rendere queste scorciatoie permanenti, è necessario configurarle all'interno di specifici file di sistema.

Esistono due tipologie principali di alias permanenti:
1.  **Alias Utente**: Applicabili esclusivamente a uno specifico profilo utente.
2.  **Alias Globali**: Disponibili per ogni utente registrato nel sistema.

---

#### 1. Configurazione degli Alias Utente
Per fare in modo che un alias sia disponibile ogni volta che un determinato utente effettua l'accesso, occorre modificare il file di configurazione nella sua home directory.

* **File da modificare**: `~/.bashrc` (Il punto iniziale indica che si tratta di un file nascosto).
* **Procedura**:
    1.  Accedere alla home directory dell'utente.
    2.  Aprire il file con un editor di testo: `vi .bashrc`.
    3.  Aggiungere in coda al file la riga dell'alias: `alias hh='hostname'`.
    4.  Salvare e uscire.



> **Nota**: Le modifiche avranno effetto solo sulle nuove sessioni. Per renderle attive immediatamente nella sessione corrente, utilizzare il comando `source .bashrc`.

---

#### 2. Configurazione degli Alias Globali
Se l'obiettivo è rendere un comando abbreviato disponibile a tutti gli utenti del sistema (incluso l'utente root e i nuovi utenti creati in futuro), è necessario intervenire sui file di configurazione globale.

* **File da modificare**: `/etc/bashrc` (o in alcune distribuzioni `/etc/bash.bashrc`).
* **Requisiti**: È necessario disporre dei privilegi di **root** per modificare i file nella directory `/etc`.
* **Procedura**:
    1.  Accedere come root o utilizzare `sudo`.
    2.  Aprire il file: `vi /etc/bashrc`.
    3.  Aggiungere l'alias alla fine del file: `alias hh='hostname'`.
    4.  Salvare il file.

---

#### 3. Confronto e Verifica delle Sessioni
Il comportamento degli alias varia a seconda di dove vengono definiti. La tabella seguente riassume la visibilità in base alla configurazione:

| Tipo di Alias | File di Configurazione | Visibilità |
| :--- | :--- | :--- |
| **Temporaneo** | Nessuno (solo terminale) | Solo la sessione corrente |
| **Utente** | `~/.bashrc` | Solo l'utente specifico |
| **Globale** | `/etc/bashrc` | Tutti gli utenti del sistema |



#### Esempio di Verifica
Dopo aver aggiunto un alias globale come `hh`:
1.  Effettuando il login come utente standard (es. *James*), digitando `hh` verrà restituito l'hostname.
2.  Effettuando il login come `root`, lo stesso comando `hh` sarà riconosciuto e funzionante.

Senza l'inserimento in questi file, ogni nuovo terminale aperto ripartirebbe da un ambiente "pulito", costringendo l'operatore a digitare nuovamente i comandi in forma estesa.

---

### La Cronologia della Shell (History)

In un sistema operativo Linux, ogni comando digitato nel terminale viene registrato in una cronologia. Questa funzione è estremamente preziosa, non solo per la produttività quotidiana, ma soprattutto durante le fasi di **troubleshooting** (risoluzione dei problemi). 

Attraverso l'analisi della cronologia è possibile determinare quali azioni hanno causato un crash del sistema, chi ha eliminato un file specifico o quali comandi errati sono stati eseguiti.

---

#### 1. Visualizzare la Cronologia
Per accedere all'elenco dei comandi eseguiti dall'utente corrente, si utilizza il comando:
```bash
history
```

Se l'elenco è molto lungo, è consigliabile scorrere l'output una pagina alla volta utilizzando un "pipe" con il comando more o less:
```bash
history | more
```

* **Nota**: La cronologia registra ogni tentativo, inclusi i comandi digitati in modo errato o quelli che non sono stati eseguiti con successo.
* **Numerazione**: Ogni riga della cronologia è contrassegnata da un numero identificativo univoco.

---

#### 2. Eseguire Nuovamente un Comando (Bang!)
Una delle funzioni più utili della cronologia è la possibilità di richiamare ed eseguire un comando precedente senza doverlo riscrivere o copiare manualmente.

**Procedura**:
1.  Identificare il numero del comando desiderato nell'elenco `history` (ad esempio, il numero `406`).
2.  Utilizzare il simbolo del punto esclamativo (**!**), chiamato in gergo "bang", seguito dal numero:
    ```bash
    !406
    ```



Il sistema visualizzerà il comando associato a quel numero e lo eseguirà immediatamente. Questa tecnica è ideale per comandi lunghi e complessi che contengono pipe (`|`), `awk` o `sort`.

---

#### 3. Ricerca nella Cronologia
Invece di scorrere l'intero elenco, è possibile filtrare la cronologia per trovare comandi specifici utilizzando `grep`. 

* **Esempio**: Per trovare tutti i comandi eseguiti che contenevano la parola "awk":
    ```bash
    history | grep awk
    ```
* **Esempio**: Per trovare tutti i comandi relativi alla modifica dei permessi (`chmod`):
    ```bash
    history | grep chmod
    ```

#### 4. Buone Pratiche
* **Consultare il Manuale**: Per scoprire opzioni avanzate (come la cancellazione della cronologia o la modifica del numero di comandi salvati), è utile consultare la guida ufficiale:
    ```bash
    man history
    ```
* **Sicurezza**: Poiché la cronologia registra tutto, è bene evitare di digitare password direttamente come argomenti dei comandi nella riga di comando.

---


