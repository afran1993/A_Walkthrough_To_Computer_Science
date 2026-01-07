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

