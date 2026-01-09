# Risoluzione dei Problemi di Connessione SSH tramite Putty

In questa lezione vengono analizzate le problematiche comuni che possono impedire il collegamento tramite Putty a una macchina virtuale (VM) Linux e i passaggi necessari per la loro risoluzione.

---

## 1. Configurazione della Scheda di Rete (NIC) in Oracle VirtualBox

Il primo controllo deve essere effettuato a livello di virtualizzatore. Se la scheda di rete non è configurata correttamente, la VM non potrà comunicare con l'host.

### Procedura:
1. Aprire **Oracle VirtualBox**.
2. Selezionare la macchina virtuale e cliccare su **Impostazioni** (Settings).
3. Accedere alla sezione **Rete** (Network).
4. Verificare che l'opzione **Abilita scheda di rete** sia selezionata.
5. In **Connesso a**, sono disponibili due opzioni principali:
   * **Scheda solo host (Host-Only Adapter):** Permette la comunicazione esclusiva tra il PC host e la VM (consigliato per test locali senza accesso a Internet).
   * **Scheda con bridge (Bridged Adapter):** La VM riceve un IP direttamente dal router fisico, permettendo l'accesso a Internet e la visibilità nella rete locale.

*Nota: Se la macchina è accesa, alcune opzioni potrebbero apparire in grigio (non modificabili). In tal caso, spegnere la VM prima di procedere.*

---

## 2. Verifica dell'Interfaccia a Livello Sistema Operativo

Anche se le impostazioni di VirtualBox sono corrette, l'interfaccia di rete all'interno di Linux potrebbe essere disattivata.

### Comandi di verifica:
Per controllare lo stato dell'interfaccia e l'indirizzo IP assegnato, utilizzare:
```bash
ifconfig
# oppure
ip addr
```

L'interfaccia principale è solitamente denominata enp0s3. È necessario prestare particolare attenzione a questa sezione dell'output. Altre interfacce, come "lo" o "virbr0", possono essere trascurate in questa fase, poiché verranno trattate nei moduli successivi del corso dedicati alle diverse opzioni di rete.

Se, nonostante la corretta configurazione in VirtualBox, non venisse visualizzato un indirizzo IP (voce "inet"), l'interfaccia potrebbe essere inattiva. In questo caso, è possibile procedere al riavvio del servizio di rete o alla riattivazione manuale dell'interfaccia.

### Comandi per la gestione dell'interfaccia:
Per disattivare e riattivare l'interfaccia specifica:
```bash
ifdown enp0s3
ifup enp0s3
```

Una volta eseguito il comando di attivazione, il sistema dovrebbe confermare l'avvenuta connessione. Eseguendo nuovamente `ifconfig` o `ip addr`, sarà possibile verificare la presenza dell'indirizzo IP associato alla voce `inet` (ad esempio: `192.168.56.101`).

In alternativa, per assicurarsi che tutte le modifiche siano applicate, è possibile procedere al riavvio completo dei servizi di rete attraverso il comando:

```bash
systemctl restart network.service
```

### Configurazione e Accesso tramite Putty
Dopo aver confermato l'indirizzo IP della macchina virtuale, si può procedere all'avvio dell'applicazione Putty. Inserendo l'indirizzo IP identificato nel campo "Host Name" e cliccando su "Open", verrà visualizzato il prompt di login, stabilendo così la comunicazione tra il sistema host e la macchina Linux.

### Analisi delle Modalità di Rete
* **Host-Only Adapter:** Questa opzione viene utilizzata per creare una rete privata tra il computer fisico e la macchina virtuale. È la modalità ideale per scopi didattici, sebbene non garantisca alla macchina virtuale l'accesso alla rete Internet.
* **Bridged Adapter:** Tale configurazione permette alla macchina virtuale di operare come un dispositivo fisico aggiuntivo all'interno della rete locale, ottenendo l'indirizzo IP direttamente dal router o modem. Questa scelta è indispensabile qualora sia necessario scaricare pacchetti o navigare sul web dal sistema Linux.

Per ottimizzare la compatibilità nelle impostazioni avanzate, si consiglia di verificare che il tipo di adattatore selezionato sia "Intel PRO/1000 MT Desktop" e che l'opzione "Cavo collegato" sia spuntata.

### Verifica del Servizio SSH
Qualora la rete risulti correttamente configurata ma la connessione venga comunque rifiutata, è opportuno verificare che il demone SSH sia attivo e in ascolto. Il controllo del processo può essere effettuato tramite il seguente comando:

```bash
ps -ef | grep ssh
```

L'output deve mostrare il processo `/usr/sbin/sshd -D`, a conferma che il demone o il processo incaricato di accettare il traffico in entrata sia regolarmente in esecuzione. 

Qualora il servizio non risultasse attivo, è possibile procedere al riavvio forzato tramite il comando:

```bash
systemctl restart sshd
```

Sebbene la probabilità che il servizio SSH non sia attivo di default sia molto bassa, tale verifica rientra nelle procedure standard di diagnostica per garantire che il sistema sia pronto a ricevere connessioni esterne.

In conclusione, l'applicazione di questi passaggi metodologici permette di identificare e risolvere la maggior parte delle criticità legate alla comunicazione tra il client Putty e l'ambiente Linux. Qualora, nonostante l'attuazione di tali procedure, dovessero persistere difficoltà tecniche, si consiglia di produrre una documentazione dettagliata (screenshot) delle impostazioni di rete e dei messaggi di errore riscontrati, al fine di agevolare un'analisi tecnica più approfondita e mirata.

---

# Gestione dei Permessi Predefiniti nella Creazione di File: il comando umask

In ambiente Linux, la creazione di un nuovo file o di una directory tramite comandi come `touch`, `cp` o attraverso il salvataggio da un editor, comporta l'assegnazione automatica di un set di permessi predefiniti. Questa lezione analizza come visualizzare e modificare tali parametri.

---

## 1. Il comando umask
Il comando **umask** (user mask) permette di stabilire i permessi predefiniti per ogni nuovo file o directory creato dall'utente.

### Sintassi Simbolica
È possibile impostare la maschera utilizzando la notazione simbolica:
* **u**: utente (owner)
* **g**: gruppo
* **o**: altri
* **r/w/x**: lettura, scrittura, esecuzione

**Esempio di comando:**
```bash
umask u=rw,g=r,o=
```

In questo caso, i nuovi file verranno creati con permessi di lettura e scrittura per l'utente, sola lettura per il gruppo e nessun permesso per gli altri utenti. Qualora si desiderasse applicare una configurazione specifica durante la sessione di lavoro, il comando deve essere eseguito direttamente nel terminale; tale impostazione rimarrà valida per tutti i nuovi file generati da quel momento in poi.

### Meccanismo di Assegnazione dei Permessi
Il sistema operativo imposta i permessi di default consultando i file di configurazione durante la fase di login. Il processo segue una gerarchia precisa:
1. Viene inizialmente controllato il file locale `.bashrc` nella home directory dell'utente.
2. In assenza di parametri specifici, il sistema consulta il file globale `/etc/bashrc`.

### Esempio Pratico e Verifica
Se si crea un file utilizzando il comando `touch js`, è possibile verificarne i permessi tramite `ls -ltr`. Se l'output mostra, ad esempio, `-rw-rw-r--`, significa che il proprietario e il gruppo hanno permessi di lettura e scrittura, mentre gli altri hanno solo il permesso di lettura. 

Per modificare questo comportamento senza dover ricorrere ogni volta al comando `chmod` dopo la creazione, si interviene sul valore di `umask`. Ad esempio, per garantire che il gruppo non abbia mai il permesso di scrittura, si può digitare:

```bash
umask g-w
```

### Configurazione Permanente
Le modifiche apportate direttamente tramite la riga di comando hanno una validità limitata alla sessione corrente; al termine del collegamento o alla chiusura del terminale, tali impostazioni andranno perse. Per fare in modo che un valore specifico di `umask` venga applicato automaticamente a ogni accesso, è necessario intervenire sui file di configurazione della shell.

#### Procedura di modifica:
1. **Identificazione del file:** Poiché i file di configurazione sono nascosti, occorre utilizzare il comando `ls -la` per individuarli nella propria directory home. Il file principale da modificare è solitamente `.bashrc`.
2. **Editing del file:** È possibile utilizzare un editor di testo come `vi` o `nano` per modificare il file:
   ```bash
   vi ~/.bashrc
   ```
3. **Inserimento del comando:** All'interno del file, deve essere aggiunta una riga specifica che definisca il valore di `umask` desiderato. Ad esempio, è possibile inserire `umask 022` (utilizzando la notazione ottale) oppure una forma simbolica come `umask u=rw,g=r,o=`. Questa direttiva istruirà la shell ad applicare tali restrizioni a ogni nuovo oggetto creato nel file system.
4. **Salvataggio e applicazione:** Una volta salvate le modifiche e chiuso l'editor, le nuove impostazioni diventeranno operative al successivo accesso al sistema. Qualora si desiderasse rendere attive le modifiche nella sessione corrente senza effettuare il logout, è possibile utilizzare il comando `source ~/.bashrc`.

Attraverso questa configurazione permanente, si garantisce che la creazione di file e directory avvenga sempre secondo criteri di sicurezza predefiniti, rispondendo correttamente ai requisiti applicativi o alle policy utente senza necessità di interventi manuali successivi.

---

# Guida alla Creazione di una Macchina Virtuale su VMware Workstation

In questa lezione vengono illustrati i passaggi necessari per configurare una nuova macchina virtuale (VM) Linux utilizzando il software VMware Workstation.

---

## 1. Configurazione Iniziale e Selezione del Supporto
Dopo aver avviato VMware Workstation, è necessario selezionare l'opzione **"Create a New Virtual Machine"** per avviare la procedura guidata.

### Selezione dell'Immagine ISO
Il sistema permette di scegliere la sorgente del sistema operativo:
* **Installer disc:** Opzione utilizzata se è presente un supporto fisico (CD/DVD) nel computer.
* **Installer disc image file (iso):** È la modalità standard. Occorre cliccare su **"Browse"** e selezionare il file ISO del sistema operativo (ad esempio CentOS) precedentemente scaricato.

*Nota: A differenza di Oracle VirtualBox, dove l'immagine ISO viene solitamente collegata dopo la creazione, VMware consente di associare il file ISO contemporaneamente alla configurazione della VM.*

---

## 2. Personalizzazione della Macchina Virtuale
Proseguendo nella configurazione, è necessario definire i parametri fondamentali:

* **Nome della VM:** Si può assegnare un nome identificativo (es. `VMware-LinuxVM`).
* **Percorso (Location):** Si specifica la cartella del computer host dove verranno salvati i file della macchina virtuale.
* **Capacità del Disco:** Per l'installazione di un sistema operativo Linux moderno, si consiglia un valore di almeno 10-20 GB. 
* **Gestione dei file disco:** Si raccomanda di selezionare l'opzione **"Store virtual disk as a single file"** per mantenere le prestazioni ottimali, evitando la frammentazione del disco virtuale in più file.



### Personalizzazione Hardware (Opzionale)
Attraverso il pulsante **"Customize Hardware"**, è possibile aumentare le risorse allocate prima della creazione definitiva, intervenendo su:
1. Quantità di memoria RAM.
2. Numero di core della CPU.

---

## 3. Risoluzione di Errori Comuni e Avvio
Al termine della configurazione, la macchina virtuale tenterà l'avvio automatico. 

### Gestione dell'errore "MKS"
Qualora apparisse l'errore *"Unable to connect to the MKS: Too many sockets"*, significa che il sistema richiede un riavvio del computer host a seguito dell'installazione di VMware Workstation. In questo caso, è necessario:
1. Chiudere VMware.
2. Riavviare il PC/Laptop.
3. Riavviare VMware e procedere con il **"Power On"** della VM creata.

---

## 4. Installazione del Sistema Operativo
Una volta avviata la macchina virtuale, verrà visualizzata la schermata di installazione del sistema operativo (es. CentOS). 

### Suggerimenti per l'interazione:
* **Rilascio del cursore:** In VMware, per liberare il mouse dalla finestra della VM e tornare al sistema host, è necessario premere contemporaneamente i tasti `Ctrl + Alt`. 
* **VMware Tools:** Durante l'installazione potrebbe apparire un avviso per il download dei "VMware Tools". Si consiglia di procedere solo dopo aver completato l'installazione del sistema operativo Linux.

A questo punto, è possibile selezionare **"Install CentOS"** e seguire la procedura guidata per completare l'operazione.

---

# Guida all'Installazione di Oracle VirtualBox su macOS

In questa lezione vengono illustrati i passaggi necessari per scaricare e installare correttamente Oracle VirtualBox su un sistema operativo macOS. VirtualBox è un software di virtualizzazione che permette di eseguire diversi sistemi operativi (come Linux) all'interno dell'ambiente Apple.

---

## 1. Download del Software
Il primo passaggio consiste nell'acquisizione del pacchetto di installazione:
1. Accedere al sito ufficiale: [virtualbox.org](https://www.virtualbox.org).
2. Cliccare sul pulsante **"Download VirtualBox"**.
3. Selezionare il pacchetto specifico per macOS, solitamente indicato come **"OS X hosts"**.
4. Attendere il completamento del download del file con estensione `.dmg`.

---

## 2. Procedura di Installazione
Una volta scaricato il file, procedere come segue:
1. Fare doppio clic sul file `.dmg` per montare l'immagine disco.
2. Nella finestra che appare, fare doppio clic sull'icona del pacchetto di installazione (**VirtualBox.pkg**).
3. Seguire le istruzioni della procedura guidata cliccando su **"Continua"** (Continue) e successivamente su **"Installa"** (Install).
4. Quando richiesto, inserire la **password del sistema operativo** per autorizzare l'installazione dei componenti.



---

## 3. Gestione delle Autorizzazioni di Sicurezza (Fondamentale)
A causa delle restrizioni di sicurezza di macOS, l'installazione dei driver di sistema necessari per VirtualBox potrebbe essere inizialmente bloccata. Qualora apparisse un avviso di "Estensione di sistema bloccata", è necessario procedere come segue:

1. Aprire le **Preferenze di Sistema** (System Preferences).
2. Selezionare la voce **Sicurezza e Privacy** (Security & Privacy).
3. Nella scheda **Generale**, individuare l'avviso riguardante il software di "Oracle America, Inc.".
4. Cliccare sull'icona del lucchetto in basso a sinistra per abilitare le modifiche.
5. Cliccare sul pulsante **"Consenti"** (Allow) accanto al messaggio di blocco del software.

*Nota: In alcuni casi, potrebbe essere richiesto il riavvio del Mac per rendere effettive le autorizzazioni.*

---

## 4. Installazione dell'Extension Pack (Opzionale ma Consigliato)
Per ottenere funzionalità avanzate (come il supporto USB 2.0/3.0 e la crittografia), si consiglia di scaricare l'**Extension Pack** dalla stessa pagina di download di VirtualBox:
1. Scaricare il file indicato come **"All supported platforms"**.
2. Una volta completato il download, fare doppio clic sul file.
3. VirtualBox si aprirà automaticamente chiedendo conferma per l'installazione; cliccare su **"Installa"** e accettare la licenza.

---

## 5. Verifica Finale
Al termine della procedura, è possibile avviare VirtualBox dalle **Applicazioni**. Se l'interfaccia principale viene visualizzata correttamente e non appaiono errori relativi ai driver di sistema, il software è pronto per la creazione della prima macchina virtuale.

---

# Installazione di Oracle VM VirtualBox Guest Additions

Le **Guest Additions** sono un insieme di strumenti e driver di sistema progettati per ottimizzare le prestazioni delle macchine virtuali e migliorare l'interazione tra il sistema host (fisico) e il sistema guest (virtuale).

---

## 1. Vantaggi delle Guest Additions
L'installazione di questi strumenti offre numerosi benefici operativi, tra cui:
* **Integrazione del mouse:** Permette di muovere liberamente il cursore tra la VM e il desktop host senza dover premere il tasto "Host" (solitamente il tasto Ctrl destro) per liberarlo dalla console.
* **Ottimizzazione video:** Migliore accelerazione grafica e supporto per il ridimensionamento automatico della finestra della VM.
* **Cartelle condivise:** Possibilità di scambiare file facilmente tra host e guest.
* **Sincronizzazione oraria:** Allineamento preciso dell'orologio della VM con quello del computer fisico.

---

## 2. Prerequisiti e Preparazione del Sistema
Prima di procedere all'installazione, è necessario che la macchina virtuale disponga di una connessione Internet attiva e che il kernel sia aggiornato all'ultima versione disponibile.

### Verifica della connettività
È possibile confermare l'accesso alla rete tramite un test di comunicazione:
```bash
ping [www.google.com](https://www.google.com)
```

### Aggiornamento del Kernel e delle Dipendenze
Per garantire la piena compatibilità dei driver, è fondamentale che il sistema sia aggiornato all'ultima versione del kernel disponibile e che siano presenti gli strumenti di compilazione. Le seguenti operazioni devono essere eseguite con privilegi di amministratore (**root**).

#### Procedura di aggiornamento:
1. **Verifica della connettività:** Assicurarsi che la macchina virtuale abbia accesso a Internet (ad esempio tramite il comando `ping www.google.com`).
2. **Aggiornamento del kernel:** Eseguire l'aggiornamento dei pacchetti relativi al kernel di sistema:
   ```bash
   yum install kernel*
   ```
3. **Installazione dei pacchetti di sviluppo:** È indispensabile installare un insieme di strumenti e librerie (compilatori e file di intestazione) necessari per la generazione dei moduli delle Guest Additions. Il comando da eseguire è il seguente:
   ```bash
   yum install gcc kernel-devel kernel-headers dkms make bzip2 perl
   ```

*Nota: Durante l'esecuzione, il sistema risolverà le dipendenze necessarie. Occorre confermare l'operazione quando richiesto dal gestore dei pacchetti.* È importante considerare che l'output visualizzato potrebbe variare rispetto a quello mostrato negli esempi, poiché dipende dallo stato di aggiornamento del sistema, dal momento in cui è stato installato e dall'eventuale presenza di patch precedenti.

---

### Procedura di Montaggio e Installazione
Una volta completata l'installazione dei prerequisiti software e aggiornato il kernel, si può procedere con l'attivazione degli strumenti di Oracle:

1. **Inserimento del supporto:** Dalla barra dei menu della macchina virtuale (sia essa Linux o Windows), accedere alla sezione **Dispositivi** (Devices) e selezionare la voce **Inserisci l'immagine del CD delle Guest Additions**.
2. **Esecuzione automatica:** L'immagine del disco verrà collegata e il sistema rileverà il supporto virtuale richiedendo l'autorizzazione per l'avvio. Cliccare su **Esegui** (Run).
3. **Autenticazione:** Verrà richiesta l'immissione della password dell'utente **root** per autorizzare l'installazione dei driver di sistema.
4. **Conclusione del processo:** Verrà aperta una finestra di terminale dove i driver del Guest OS saranno compilati e installati automaticamente. Al termine della procedura, l'integrazione sarà completa: il mouse non risulterà più bloccato all'interno della finestra della macchina virtuale, consentendo uno spostamento fluido tra la console e il desktop del sistema host.

Questa procedura garantisce un'integrazione ottimale, migliorando l'efficienza complessiva e l'esperienza d'uso dell'ambiente virtualizzato.

---

# Definizione dei Colori nel File System Linux

In ambiente Linux, l'esecuzione di comandi per il listing dei file (come `ls -l` o `ls -ltr`) mostra spesso elementi di colori diversi. Questa codifica cromatica non è puramente estetica, ma serve a identificare immediatamente la natura e le proprietà di un file o di una directory.

---

## 1. Legenda dei Colori Predefiniti

Di seguito sono elencati i colori standard utilizzati nella maggior parte delle distribuzioni Linux (come CentOS, Ubuntu o Red Hat):

| Colore | Significato | Descrizione |
| :--- | :--- | :--- |
| **Blu** | **Directory** | Indica una cartella. Il primo bit dei permessi è identificato da una `d`. |
| **Verde** | **Eseguibile** | File con permessi di esecuzione (`x`). Spesso si tratta di script di shell. |
| **Azzurro** | **Link Simbolico** | Un collegamento che punta a un altro file o directory. |
| **Giallo** | **Dispositivo** | File hardware (es. mouse, dischi) presenti solitamente in `/dev`. |
| **Rosa** | **Immagine** | File grafici come `.jpg`, `.png`, `.gif`. |
| **Rosso** | **Archivio** | File compressi o pacchetti (es. `.tar`, `.zip`, `.rpm`). |
| **Rosso su Nero** | **Link Interrotto** | Un collegamento simbolico il cui file di origine è inesistente o errato. |



---

## 2. Analisi Dettagliata delle Tipologie

### File Eseguibili (Verde)
Un file appare in verde quando gli vengono assegnati i permessi di esecuzione. 
Esempio per rendere un file eseguibile:
```bash
chmod +x nome_file
```

Se si rimuovono i permessi di esecuzione tramite il comando `chmod -x`, il file perde la sua connotazione di script e il colore vira dal verde al bianco (o al colore predefinito per i file regolari). In Linux, il colore è strettamente legato ai metadati del file e ai suoi permessi.

### Approfondimento sulle Categorie di Colore

#### Collegamenti Simbolici e Percorsi Assoluti
La creazione di un link simbolico richiede l'uso del comando `ln -s`. È fondamentale specificare il percorso assoluto della sorgente per evitare la creazione di un **link interrotto**, che verrebbe visualizzato con un testo rosso su sfondo nero.

* **Esempio di link corretto (Azzurro):**
    ```bash
    ln -s /home/utente/documento.txt /tmp/link_documento
    ```
* **Esempio di link interrotto (Rosso/Nero):** Si verifica quando il file sorgente viene spostato o eliminato, o se il puntatore non è stato configurato correttamente.



#### File di Dispositivo (Giallo)
All'interno della directory `/dev`, i file che rappresentano l'hardware (come terminali `tty`, dischi o interfacce di rete) assumono una colorazione gialla con sfondo nero. Questi file sono caratterizzati dal bit `c` (dispositivi a caratteri) o `b` (dispositivi a blocchi) nella prima colonna della stringa dei permessi.

#### Archivi e Immagini
I file compressi o gli archivi creati con il comando `tar c-v-f` vengono visualizzati in **rosso**. I file grafici (come `.jpg` o `.gif`), invece, appaiono in **rosa**. Si ricorda che la visualizzazione del contenuto di un'immagine non è possibile tramite sessioni SSH testuali (come Putty), ma richiede un ambiente desktop con interfaccia grafica (GUI).

In conclusione, l'uso dei colori permette una diagnostica visiva rapida dello stato del sistema, facilitando l'identificazione di script eseguibili, directory o anomalie nei collegamenti.


---

# Risoluzione dei Problemi: Eliminazione, Copia e Spostamento di File in Linux

In ambiente Linux, le operazioni di eliminazione (`rm`), copia (`cp`), rinomina e spostamento (`mv`) sono strettamente correlate e dipendono dagli stessi fattori sistemistici. Questa lezione analizza le cause principali per cui tali operazioni possono fallire.

---

## 1. Fattori Critici e Diagnostica

Prima di eseguire un'operazione sui file, è necessario verificare i seguenti punti:

* **Esistenza del file:** Verificare che il nome del file sia corretto (Linux è *case-sensitive*) e che il percorso (assoluto o relativo) sia esatto.
* **Tipologia di oggetto:** Il comando `rm` standard non elimina le directory. Per le cartelle è necessario utilizzare `rm -r` (ricorsivo) o `rmdir` (se la cartella è vuota).
* **Sintassi del comando:** * Nella copia (`cp`) e nello spostamento (`mv`), la sintassi corretta è sempre: `comando [SORGENTE] [DESTINAZIONE]`.
    * L'omissione della destinazione comporterà un errore di tipo "missing destination file operand".

---

## 2. Gestione dei Permessi

La causa più comune di errore è il messaggio **"Permission denied"**. L'accesso ai file è regolato da una gerarchia di permessi (Utente, Gruppo, Altri).



### Permessi del File vs Permessi della Directory
Un errore concettuale frequente è ritenere che bastino i permessi di scrittura sul file per poterlo eliminare.
* **Regola d'oro:** Per eliminare o rinominare un file, non sono sufficienti i permessi sul file stesso; è **indispensabile** avere i permessi di scrittura (`w`) sulla **directory parente**.
* Se la directory superiore è di proprietà di `root` e non concede permessi di scrittura agli altri utenti, un utente standard non potrà rimuovere i file contenuti, anche se ne è il proprietario.

---

## 3. Esempi Pratici di Risoluzione

### Caso A: Impossibilità di Copiare un File
Se si tenta di copiare un file (es. `/var/log/messages`) verso una cartella di sistema (es. `/etc/`):
1.  **Sorgente:** L'utente deve avere almeno il permesso di lettura (`r`) sul file sorgente.
2.  **Destinazione:** L'utente deve avere il permesso di scrittura (`w`) sulla directory di destinazione.
Se uno dei due manca, l'operazione fallirà con "Permission denied".

### Caso B: Rinomina e Spostamento
In Linux, rinominare e spostare sono la stessa operazione logica eseguita dal comando `mv`. 
* Per rinominare `file1` in `file2`, il sistema deve poter modificare la tabella dei file della directory. 
* Se l'operazione fallisce, verificare i permessi della directory corrente con:
    ```bash
    ls -ld .
    ```

### Caso C: Applicazione Ricorsiva dei Permessi
Per risolvere problemi di accesso su intere strutture di cartelle, è possibile utilizzare l'opzione `-R` (Recursive) per propagare i permessi verso il basso:
```bash
chmod -R a+w nome_directory
```

*Nota: L'uso di permessi di scrittura globali (`a+w`) deve essere limitato esclusivamente ad ambienti di test o a directory temporanee specifiche, poiché espone il sistema a rischi di sicurezza permettendo a chiunque di modificare o eliminare i dati.*

---

## 4. Riepilogo Comandi e Struttura dei Permessi

Per comprendere correttamente perché un'operazione di gestione file fallisce, è necessario saper interpretare la stringa dei permessi restituita dal comando `ls -l`.



### Tabella dei Comandi Principali

| Azione | Comando | Requisiti di Permesso |
| :--- | :--- | :--- |
| **Eliminazione** | `rm <file>` | Scrittura (`w`) e Esecuzione (`x`) sulla **directory parente**. |
| **Copia** | `cp <sorgente> <dest>` | Lettura (`r`) sulla sorgente; Scrittura (`w`) sulla destinazione. |
| **Spostamento** | `mv <vecchio> <nuovo>` | Scrittura (`w`) sulla directory di origine e di destinazione. |
| **Rinomina** | `mv <nome> <nuovonome>` | Scrittura (`w`) sulla directory corrente. |

### Analisi della Gerarchia
Se un utente è proprietario di un file ma non della directory che lo contiene, l'eliminazione potrebbe essere inibita. La struttura gerarchica di Linux prevede che la directory funga da "contenitore": se il contenitore è sigillato (mancanza di permesso `w` per l'utente), non è possibile aggiungere o rimuovere gli elementi al suo interno, indipendentemente dai permessi dei singoli file.



### Risoluzione rapida (Troubleshooting)
1.  **Identificare l'utente attuale:** Eseguire `whoami`.
2.  **Controllare i permessi del file:** Eseguire `ls -l <file>`.
3.  **Controllare i permessi della directory:** Eseguire `ls -ld .` (per la cartella corrente) o `ls -ld ..` (per la cartella superiore).
4.  **Verificare la proprietà:** Assicurarsi che l'utente o il suo gruppo siano indicati come proprietari, altrimenti sarà necessario l'intervento dell'amministratore (`root`).

Attraverso questa metodologia di verifica a ritroso (dal file alla directory parente), è possibile identificare e risolvere la quasi totalità dei blocchi operativi nel file system.

---

# Risoluzione dei Problemi: Errore "Cannot CD" (Cambio Directory Fallito)

In ambiente Linux, il fallimento del comando `cd` (Change Directory) può derivare da diverse cause. Di seguito vengono analizzati i passaggi diagnostici per isolare e risolvere il problema.

---

## 1. Inesistenza della Directory o Errori di Digitazione

La causa più banale è l'assenza fisica della cartella o un errore nel nome digitato. 

* **Case Sensitivity:** Linux distingue tra maiuscole e minuscole. Tentare di accedere a `Seinfeld` invece di `seinfeld` restituirà l'errore: `No such file or directory`.
* **Verifica Visiva:** Prima di cambiare directory, è consigliabile eseguire `ls -l` per confermare l'esatta ortografia del nome.

---

## 2. Percorso Assoluto vs Percorso Relativo

L'errore spesso risiede in una confusione sulla posizione attuale nel file system.

* **Percorso Assoluto:** Inizia sempre dalla radice `/` (es. `/root/seinfeld/jerry`). Se si omette una sottocartella intermedia nel percorso completo, il sistema non troverà l'oggetto.
* **Percorso Relativo:** Parte dalla posizione attuale. Se ci si trova in `/root` e si digita `cd /seinfeld`, il sistema cercherà la cartella `seinfeld` direttamente sotto la radice globale `/`, non all'interno di `/root`.
    * *Suggerimento:* In caso di errore, procedere "un gradino alla volta" (es. `cd /root`, poi `ls`, poi `cd seinfeld`).



---

## 3. Gestione dei Permessi di Accesso

Per poter entrare in una directory, non è sufficiente il permesso di lettura; il bit fondamentale è quello di **Esecuzione (`x`)**.

### Analisi della Stringa dei Permessi
Eseguendo `ls -l`, la prima colonna mostra i permessi (es. `drwxr-xr-x`):

* **Il primo carattere (`d`):** Indica che l'oggetto è effettivamente una directory. Se compare un trattino `-`, si tratta di un file regolare e il comando `cd` fallirà sempre.
* **Il bit di esecuzione (`x`):** Rappresenta il diritto di "attraversare" la directory. Se l'utente (o il gruppo di appartenenza) non ha il permesso `x`, riceverà il messaggio `Permission denied`.



> **Nota sulla differenza tra `r` e `x`:** > * Senza `x`: Non è possibile entrare nella directory (`cd` fallisce).
> * Senza `r`: È possibile entrare nella directory (se si ha `x`), ma non è possibile vederne il contenuto tramite il comando `ls`.

---

## 4. Permessi della Directory Parente

L'accesso a una sottocartella è subordinato ai permessi di tutte le cartelle superiori. Se la "porta principale" (la directory genitrice) è bloccata (mancanza di permesso `x`), non sarà possibile accedere a nessuna delle sottocartelle in essa contenute, indipendentemente dai permessi di queste ultime.

---

## 5. Directory Nascoste e Tipo di File

* **File Nascosti:** Le directory che iniziano con un punto (es. `.simpsons`) sono nascoste. Non appaiono con un semplice `ls`, ma richiedono il comando `ls -a`. Per accedervi è necessario includere il punto nel comando: `cd .simpsons`.
* **Verifica del Tipo di File:** Se si tenta di eseguire `cd` su un file (creato ad esempio con `touch`), il sistema restituirà l'errore `Not a directory`. Verificare sempre che il primo bit della stringa dei permessi sia `d`.

---

## Riepilogo Diagnostico

1.  Eseguire `pwd` per confermare la posizione attuale.
2.  Eseguire `ls -la` per verificare l'esistenza, l'ortografia e se la cartella è nascosta.
3.  Verificare che il primo bit sia `d` e che sia presente il bit `x` per il proprio utente.
4.  Controllare i permessi della cartella superiore se l'accesso è ancora negato.

---

# Guida alla Gestione e Ripristino del File System Corrotto

In ambiente Linux, la corruzione del file system può causare rallentamenti del sistema, errori di input/output o il mancato avvio dell'operatività. Questa lezione illustra come identificare, isolare e risolvere tali anomalie.

---

## 1. Cos'è un File System?
Il file system è la struttura logica utilizzata dall'operativo per gestire l'archiviazione e il recupero dei dati.
* **Tipi comuni:** `ext3`, `ext4`, `xfs` (Linux); `NTFS`, `FAT32` (Windows).
* **Partizionamento:** Suddividere un disco in diverse partizioni (es. `/var`, `/etc`, `/home`) permette di isolare eventuali corruzioni. Se la partizione `/var` si corrompe, l'impatto resterà limitato a quella sezione, senza compromettere l'intero sistema (`/`).



---

## 2. Identificazione del Problema
Prima di intervenire, è necessario confermare lo stato dei dischi e individuare l'errore.

* **Comandi di verifica:**
    * `df -h`: Mostra le partizioni montate, lo spazio utilizzato e i punti di montaggio.
    * `fdisk -l`: Elenca tutti i dischi fisici e le partizioni disponibili.
    * **Log di sistema:** Controllare `/var/log/messages` o `/var/log/syslog`. Errori come "bad block", "sector error" o richieste esplicite di eseguire `fsck` indicano una corruzione fisica o logica.

---

## 3. Ripristino con FSCK (File System Check)
Il comando principale per la riparazione è `fsck`. Tuttavia, esistono regole ferree per il suo utilizzo:

> **ATTENZIONE:** Non eseguire mai `fsck` su una partizione montata. Ciò può causare danni permanenti ai dati.

### Procedura di base:
1. **Smontare la partizione:** `umount /punto_di_montaggio` (es. `umount /sparedata`).
2. **Eseguire il controllo sul dispositivo a blocchi:** Usare il percorso del dispositivo, non il punto di montaggio.
   ```bash
   fsck /dev/sdb1
   ```
3. **Opzione automatica:** Per rispondere automaticamente "sì" a tutte le richieste di correzione durante la scansione, è possibile utilizzare il flag `-y`. Questo è particolarmente utile per evitare l'inserimento manuale in caso di numerosi errori:
   ```bash
   fsck -y /dev/sdb1
   ```
## 4. Casi Speciali: File System XFS
Sui sistemi Linux moderni (come RHEL o CentOS 7 e versioni successive) che adottano il file system `XFS`, il comando standard `fsck` agisce esclusivamente come "wrapper" e non esegue riparazioni dirette. Per intervenire su un file system XFS danneggiato, è necessario utilizzare lo strumento specifico:

```bash
xfs_repair /dev/sda1
```

## 5. Ripristino della Partizione Root (Modalità Rescue)
Qualora la corruzione interessi la partizione principale (`/`), l'operazione di smontaggio non può essere eseguita poiché il sistema operativo è in funzione su di essa. In tale scenario, è necessario avviare il sistema in modalità di emergenza.

### Procedura di Emergenza:
1.  **Boot da supporto esterno:** Avviare la macchina tramite un supporto ISO (CD/DVD o USB) di installazione della distribuzione in uso (es. CentOS o Red Hat).
2.  **Accesso a Troubleshooting:** Selezionare la voce `Troubleshooting` dal menu di avvio iniziale.
3.  **Modalità Rescue:** Selezionare l'opzione `Rescue a CentOS system` (o equivalente).



4.  **Selezione della Shell:** L'ambiente di rescue cercherà le installazioni Linux esistenti. Per riparare un file system corrotto, si consiglia di selezionare l'opzione **3 (Skip to shell)**. In questo modo si evita il montaggio automatico della partizione in `/mnt/sysimage`, condizione necessaria per eseguire i comandi di riparazione.
5.  **Identificazione del Dispositivo:** Una volta ottenuta la shell, individuare la partizione corretta (es. `/dev/sda1`) tramite il comando `fdisk -l` o `lsblk`.
6.  **Esecuzione della Riparazione:**
    * Per file system **EXT4**: `fsck -y /dev/sda1`
    * Per file system **XFS**: `xfs_repair /dev/sda1`
7.  **Riavvio:** Al termine delle operazioni, digitare `reboot`, rimuovere il supporto ISO e verificare il ripristino del sistema.



---

## Tabella Riepilogativa dei Comandi di Manutenzione

| Situazione | Comando Consigliato | Note Operative |
| :--- | :--- | :--- |
| **Verifica Spazio/Tipo** | `df -hT` | Permette di distinguere tra partizioni `xfs` ed `ext4`. |
| **Smontaggio Partizione** | `umount /punto_di_mount` | Obbligatorio prima di procedere con la riparazione. |
| **Riparazione EXT4** | `fsck -y /dev/sdXN` | Analizza e corregge le inconsistenze dei blocchi. |
| **Riparazione XFS** | `xfs_repair /dev/sdXN` | Specifica per la riparazione dei metadati XFS. |
| **Elenco Dischi** | `fdisk -l` | Fornisce la struttura delle partizioni del sistema. |

---

# Analisi e Risoluzione della Lentezza di Sistema in Linux

Quando un sistema risulta lento, è necessario seguire un approccio metodico per identificare il collo di bottiglia tra **CPU, Memoria, Disco o Rete**.

## 1. Fase Preliminare: Verifica del Sistema
Prima di iniziare, assicurarsi di operare sul server corretto (specialmente in ambienti con molti host simili).
* `hostname`: Verifica il nome dell'host su cui si è connessi.

---

## 2. Analisi del Disco (Spazio e Prestazioni)
Un disco pieno (oltre il 90-95%) può bloccare i processi che necessitano di scrivere log o file temporanei.
* `df -h`: Mostra lo spazio totale, usato e disponibile.
* `du -ah /percorso | sort -rh | head -n 10`: Identifica i file o le directory più grandi.
* `iostat -y 5`: Monitora le statistiche di input/output ogni 5 secondi per individuare dischi degradati o saturati.
* `lsof`: Elenca i file aperti (utile per identificare processi che bloccano lo smontaggio di un file system).



---

## 3. Analisi di CPU e Memoria
Identificare quali processi stanno consumando le risorse di calcolo.
* `top`: Fornisce una panoramica in tempo reale di CPU, memoria, uptime e carico medio (load average).
* `free -m`: Visualizza la memoria RAM e lo Swap utilizzata in Megabyte.
* `vmstat`: Analizza lo stato della memoria virtuale e dello swap.
* `pmap <PID>`: Mostra la mappa di memoria di un singolo processo specifico.

### Informazioni Hardware
* `lscpu` o `cat /proc/cpuinfo`: Dettagli sull'architettura e i core della CPU.
* `lsmem` o `cat /proc/meminfo`: Dettagli tecnici sulla memoria RAM.
* `dmidecode`: Fornisce informazioni hardware profonde (numeri di serie, versioni BIOS, produttore).

---

## 4. Analisi della Rete
Se il problema riguarda il trasferimento di file o la comunicazione tra server.
* `tcpdump -i <interfaccia>`: Cattura il traffico di rete in tempo reale per analisi dei pacchetti.
* `ss -plnt` (o il precedente `netstat -plnt`): Mostra le porte in ascolto e i socket aperti.
* `iftop`: Monitora l'utilizzo della banda per ogni connessione (richiede installazione tramite `epel-release`).



---

## 5. Log di Sistema e Uptime
I log sono la risorsa principale per trovare errori del kernel o guasti hardware.
* `uptime`: Controlla da quanto tempo il sistema è attivo e il carico medio negli ultimi 1, 5 e 15 minuti.
* **Log principali:** Consultare `/var/log/messages` o `/var/log/syslog` per messaggi d'errore del sistema.

---

## Strumenti Aggiuntivi Consigliati (Add-ons)
Se i tool standard non sono sufficienti, è possibile installare pacchetti extra:
* `htop`: Versione interattiva e visuale di `top`.
* `iotop`: Mostra quali processi stanno scrivendo/leggendo sul disco in tempo reale.

| Comando | Ambito di Analisi |
| :--- | :--- |
| `top` | CPU / Processi |
| `free` | Memoria RAM |
| `iostat` | Prestazioni Disco |
| `df` | Capacità Disco |
| `tcpdump` | Traffico Rete |

---

