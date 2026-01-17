# 🛠️ Modulo: System Lifecycle & Storage Management

In questa sezione analizziamo come "pensa" un sistema Linux: dal momento in cui riceve energia (Boot), alla gestione del suo stato operativo (Run Levels), fino all'organizzazione fisica e logica dei dati (Storage).

---

### **Q1: Run Level e Target - Come ottimizzare un server di produzione?**

**Scenario:** Ti viene chiesto di gestire un server database che mostra rallentamenti. Noti che il sistema carica un'interfaccia grafica (GUI) che non viene mai usata, consumando RAM e cicli di CPU preziosi.

* **Ragionamento Infrastrutturale:** Un server deve essere "snello". Ogni servizio inutile è una potenziale falla di sicurezza e uno spreco di risorse. L'architettura Linux permette di definire "stati" (Run Levels) per caricare solo lo stretto necessario.
* **Azione:** Passare dal livello 5 (Grafico) al livello 3 (Testuale/Rete).
* **Comandi Chiave:**
    ```bash
    # Verificare il livello attuale
    who -r 
    
    # Passare immediatamente alla modalità testuale
    init 3 
    
    # Impostare il boot testuale come predefinito (sistemi systemd)
    systemctl set-default multi-user.target
    ```
* **Perché si fa?** Per garantire che il 100% della potenza della CPU e della RAM sia dedicata al servizio critico (es. il database) e non al rendering di icone o finestre.

---

### **Q2: Boot Process - Perché il sistema non trova il disco?**

**Scenario:** Un server non si avvia più dopo un blackout. Sullo schermo appare "No bootable device found".

* **Ragionamento Logico:** Bisogna isolare in quale fase del Bootstrap si trova il problema.
    1. **Firmware (BIOS/UEFI):** Se non vedi nemmeno il logo del produttore, è un guasto hardware.
    2. **MBR:** Se il BIOS non trova il Master Boot Record, non sa dove sia il "cervello" del sistema.
    3. **GRUB:** Se vedi un prompt `grub>`, il disco è letto ma la configurazione di avvio è corrotta.
* **Architettura:** Nelle infrastrutture moderne si usa **UEFI + GPT** invece di **BIOS + MBR**. Perché? Perché l'MBR ha un limite fisico di **2 Terabyte** e non può gestire dischi più grandi.
* **Soluzione di Emergenza:** Utilizzare un disco di ripristino per reinstallare il bootloader:
    ```bash
    # Esempio concettuale di ripristino GRUB
    grub2-install /dev/sda
    grub2-mkconfig -o /boot/grub2/grub.cfg
    ```



---

### **Q3: Performance di Avvio - Come giustificare un investimento in SSD?**

**Scenario:** Il management ritiene che i server siano troppo lenti a riavviarsi durante le finestre di manutenzione. Ti servono prove concrete per richiedere hardware più veloce.

* **Ragionamento Analitico:** Non si può migliorare ciò che non si può misurare. Usiamo `systemd-analyze` per scomporre il boot.
* **Comandi di Analisi:**
    ```bash
    # Analisi del tempo totale diviso per aree (Kernel vs User Space)
    systemd-analyze time
    
    # Identificare i servizi "colpevoli" di rallentamento
    systemd-analyze blame
    ```
* **Perché questo comando?** Se `systemd-analyze` mostra che il Kernel impiega 20 secondi, il collo di bottiglia è il caricamento dei driver dal disco rigido (meccanico). Se lo User Space impiega molto, il problema è la configurazione dei servizi (es. un DNS che non risponde).

---

### **Q4: MOTD Dinamico - Come monitorare 100 server al login?**

**Scenario:** Gestisci un parco macchine numeroso. Vuoi che chiunque acceda veda subito lo stato di salute del server (CPU, RAM, Hostname) senza dover digitare comandi.

* **Ragionamento di Scalabilità:** Invece di scrivere manualmente in `/etc/motd`, usiamo uno script che interroga il sistema ad ogni accesso.
* **Architettura:** Sfruttiamo la directory `/etc/profile.d/`, dove ogni script `.sh` viene eseguito automaticamente all'apertura della sessione.
* **Implementazione:**
    ```bash
    # Creazione dello script dinamico
    vi /etc/profile.d/welcome.sh
    
    # Contenuto dello script (uso della sostituzione di comando)
    echo "Benvenuto su $(hostname)"
    echo "Il Kernel in uso è: $(uname -r)"
    echo "Uptime del sistema: $(uptime -p)"
    ```
* **Vantaggio:** Questo approccio è "Infrastructure as Code". Puoi copiare lo stesso script su 1000 server e ognuno mostrerà i propri dati corretti.

---

### **Q5: SAN vs NAS - Quale storage per un database ad alte prestazioni?**

**Scenario:** Devi scegliere dove salvare i file di un database SQL che gestisce migliaia di transazioni al secondo.

* **Ragionamento di Architettura:** * **NAS (Network Attached Storage):** Comunicazione a livello di **File** (IP/Ethernet). Facile da gestire ma più lenta.
    * **SAN (Storage Area Network):** Comunicazione a livello di **Blocchi** (Fibra Ottica). Il server "crede" che il disco sia infilato fisicamente dentro di sé.
* **Decisione:** Per un database si sceglie la **SAN**. 
* **Perché?** Perché la latenza è quasi zero e il sistema può gestire il file system (XFS/EXT4) direttamente, senza l'overhead dei protocolli di rete come NFS o Samba.



---

### **Q6: Gestione Dischi - Perché il server si blocca al riavvio?**

**Scenario:** Hai aggiunto un nuovo disco `/dev/sdb`, lo hai montato manualmente in `/data` e tutto funziona. Al riavvio, però, il server entra in "Emergency Mode".

* **Ragionamento Tecnico:** Il comando `mount` è temporaneo. Per rendere persistente un disco, Linux legge `/etc/fstab` all'avvio. Se c'è un errore di sintassi o il disco non viene trovato, il boot fallisce per sicurezza.
* **Azione Corretta:**
    ```bash
    # 1. Trovare l'identificativo unico del disco (UUID è più sicuro di /dev/sdb1)
    blkid /dev/sdb1
    
    # 2. Aggiungere la riga in /etc/fstab
    # UUID=...  /data  xfs  defaults  0 0
    
    # 3. TEST FONDAMENTALE (prima di riavviare!)
    mount -a
    ```
* **Perché `mount -a`?** Questo comando simula ciò che accadrà al boot. Se non restituisce errori, il server si riavvierà correttamente. Se dà errore, hai la possibilità di riparare il file prima del disastro.

---

# 💾 Modulo: Advanced Storage - LVM, Swap & Stratis

In questo modulo analizziamo come gestire lo storage in modo elastico e professionale, superando i limiti dei dischi fisici statici.

---

### **Q1: Perché LVM è considerato lo standard industriale rispetto al partizionamento standard?**

**Scenario:** Il server database di un cliente sta finendo lo spazio sulla partizione `/data`. Il disco fisico è pieno, ma hai uno slot libero nel server.

* **Ragionamento Architetturale:** Con il partizionamento standard, dovresti spegnere il server, clonare il disco su uno più grande e allargare la partizione (rischiando la perdita di dati). Con **LVM**, il disco fisico è solo un "ingrediente" di un pool più grande.
* **Il Flusso Logico:** 1. Aggiungi un nuovo **PV** (Physical Volume).
    2. Lo aggiungi al **VG** (Volume Group) esistente.
    3. Estendi il **LV** (Logical Volume) e il file system "a caldo".
* **Comandi per il Troubleshooting (I "tre pilastri"):**
    ```bash
    pvs # Scansione dei dischi fisici
    vgs # Scansione dei gruppi (il pool di spazio)
    lvs # Scansione dei volumi logici (le partizioni virtuali)
    ```



---

### **Q2: Qual è la sequenza esatta per aggiungere e attivare un nuovo volume LVM?**

**Scenario:** Hai appena inserito un nuovo disco da 1GB (`/dev/sdc`) e devi renderlo disponibile per l'applicazione Oracle in `/oracle`.

* **Ragionamento Operativo:** Non puoi saltare i passaggi. Devi trasformare il "metallo" in "logica".
* **Procedura Certificata:**
    ```bash
    # 1. Prepara il disco (ID 8e è fondamentale per il riconoscimento LVM)
    fdisk /dev/sdc 
    
    # 2. Crea il Volume Fisico
    pvcreate /dev/sdc1
    
    # 3. Crea il Gruppo di Volumi (il contenitore)
    vgcreate oracle_vg /dev/sdc1
    
    # 4. Crea il Volume Logico (si consiglia di lasciare sempre un po' di spazio libero)
    lvcreate -n oracle_lv -L 1000M oracle_vg
    
    # 5. Formatta e Monta
    mkfs.xfs /dev/oracle_vg/oracle_lv
    mkdir /oracle
    mount /dev/oracle_vg/oracle_lv /oracle
    ```

---

### **Q3: Come si esegue un'estensione del disco "Zero-Downtime"?**

**Scenario:** La directory `/oracle` è piena. Hai aggiunto un secondo disco da 1GB (`/dev/sdd`). Devi raddoppiare lo spazio senza mai spegnere il database.

* **Ragionamento di Business Continuity:** Grazie a XFS e LVM, l'estensione può avvenire mentre gli utenti scrivono dati.
* **Azione Solutiva:**
    ```bash
    # Inizializza ed estendi il pool
    pvcreate /dev/sdd1
    vgextend oracle_vg /dev/sdd1
    
    # Estendi il Volume Logico al 100% dello spazio libero ora disponibile
    lvextend -l +100%FREE /dev/oracle_vg/oracle_lv
    
    # Fondamentale: informa il File System che lo spazio sotto di lui è aumentato
    xfs_growfs /oracle
    ```
* **Nota:** `xfs_growfs` agisce sul punto di montaggio, non sul device. È l'ultimo miglio dell'estensione.



---

### **Q4: Spazio di Swap - Quando la RAM finisce, come salviamo il sistema dal crash?**

**Scenario:** Un'applicazione ha un memory leak e la RAM è satura. Il sistema rallenta vistosamente. Hai bisogno di "memoria virtuale" immediata ma non puoi aggiungere hardware.

* **Ragionamento Tecnico:** Lo Swap è una "rete di sicurezza" su disco. È più lento della RAM, ma impedisce al kernel di attivare l'OOM (Out Of Memory) Killer che inizierebbe a terminare i processi a caso.
* **Creazione di uno Swap d'emergenza (File Swap):**
    ```bash
    # Crea un file "pesante" da 1GB pieno di zeri
    dd if=/dev/zero of=/newswap bs=1M count=1024
    
    # Sicurezza: solo root deve poter leggere la memoria virtuale
    chmod 600 /newswap
    
    # Trasforma il file in area di Swap e attivalo
    mkswap /newswap
    swapon /newswap
    
    # Verifica l'incremento
    free -m
    ```

---

### **Q5: Cos'è la "Geometria" di un File System e perché usare `xfs_info`?**

**Scenario:** Devi migrare dei dati e vuoi assicurarti che la dimensione dei blocchi sia ottimizzata per file grandi.

* **Ragionamento da Amministratore:** XFS divide il disco in **Allocation Groups (AG)**. Questo permette a più processori di scrivere contemporaneamente sul disco senza mettersi in coda l'uno con l'altro.
* **Diagnostica:**
    ```bash
    xfs_info /oracle
    ```
* **Perché è utile?** Ti mostra il `bsize` (block size). Se il tuo database scrive blocchi da 4KB e il file system è impostato a 4KB, hai la massima efficienza.

---

### **Q6: Stratis vs LVM - Qual è il futuro della gestione storage in RHEL/CentOS 8+?**

**Scenario:** Vuoi una gestione dello storage che si occupi automaticamente dell'espansione e del "Thin Provisioning" (usare solo lo spazio effettivamente scritto).

* **Ragionamento Evolutivo:** LVM è potente ma manuale. **Stratis** è un demone (`stratisd`) che gestisce LVM e XFS per te, offrendo un'interfaccia molto più semplice.
* **Workshop Rapido:**
    ```bash
    # Creazione di un pool con due dischi
    stratis pool create mypool /dev/sdb /dev/sdc
    
    # Creazione del file system (appare come 1TB ma occupa 0 sul disco fisico)
    stratis filesystem create mypool mydata
    
    # Montaggio con dipendenza dal servizio (critico per il boot!)
    # In /etc/fstab:
    # /dev/stratis/mypool/mydata /mnt xfs defaults,x-systemd.requires=stratisd.service 0 0
    ```
* **Vantaggio Competitivo:** Stratis esegue il "Thin Provisioning" di serie. Se crei un file system, lui dice al sistema che è da 1 Terabyte, ma sulla SAN o sul disco consumerà solo i pochi MB effettivamente occupati dai tuoi file.

---

### **Tabella Comparativa Finale**

| Caratteristica | Partizionamento Standard | LVM | Stratis |
| :--- | :--- | :--- | :--- |
| **Flessibilità** | Nulla | Alta | Altissima |
| **Estensione FS** | Difficile / Offline | Manuale / Online | Automatica |
| **Snapshot** | No | Sì | Sì (Molto veloce) |
| **Complexity** | Bassa | Media | Bassa (Gestita da demone) |

---

# 🛡️ Modulo: Data Integrity, Backup & Network Sharing

In questo modulo affrontiamo la protezione fisica dei dati (RAID), la riparazione dei file system, le strategie di clonazione e la condivisione di risorse in rete.

---

### **Q1: RAID 0, 1 e 5 - Quale architettura scegliere per bilanciare costi e sicurezza?**

**Scenario:** Devi configurare lo storage per un nuovo server che ospiterà un database critico. Hai a disposizione 3 dischi da 1TB.

* **Ragionamento Architetturale:**
    1. **RAID 0 (Striping):** Somma le prestazioni. Se un disco muore, perdi tutto. *Escluso per database critici.*
    2. **RAID 1 (Mirroring):** Massima sicurezza (copia speculare). Con 3 dischi potresti fare un mirror a 3 vie, ma avresti solo 1TB utile. *Molto costoso.*
    3. **RAID 5 (Parità):** È il compromesso ideale. Con 3 dischi da 1TB, ottieni **2TB utili** e la sicurezza che, se un disco si guasta, il sistema continua a girare ricostruendo i dati dai restanti due.
* **Azione:** Scegliamo il **RAID 5**. 
* **Differenza Chiave:** Il RAID protegge dal guasto del "ferro" (disco fisso), mentre LVM gestisce la flessibilità logica (partizioni). In un server professionale, spesso LVM gira *sopra* un volume RAID.



---

### **Q2: `fsck` vs `xfs_repair` - Come riparare un file system dopo un crash?**

**Scenario:** Dopo un riavvio forzato, il sistema riporta errori di I/O sulla partizione `/data` (XFS). 

* **Ragionamento Logico:** Non tutti i file system sono uguali. Usare lo strumento sbagliato può danneggiare ulteriormente i dati.
* **Procedura di Emergenza:**
    1. **Identificazione:** `df -Th` per vedere se è EXT4 o XFS.
    2. **Smontaggio:** Mai riparare un disco montato! `umount /data`.
    3. **Riparazione:** Se XFS, usa `xfs_repair /dev/sdX`. Se EXT4, usa `fsck -y /dev/sdX`.
* **Perché XFS non si ripara al boot?** XFS è progettato per volumi enormi (Petabyte). Un controllo automatico al boot potrebbe bloccare il server per ore. Si preferisce far partire il sistema e lasciare all'admin il compito di riparare manualmente se necessario.

---

### **Q3: `dd` - Quando la copia dei file non basta e serve la "Clonazione"?**

**Scenario:** Devi migrare il sistema operativo da un vecchio disco meccanico a un nuovo SSD identico, preservando il bootloader e le partizioni nascoste.

* **Ragionamento Tecnico:** Comandi come `cp` o `tar` copiano i file, ma ignorano l'MBR (Master Boot Record) o la tabella delle partizioni. `dd` lavora a livello di bit, ignorando cosa siano i dati: copia la struttura fisica.
* **Comando Risolutivo:**
    ```bash
    # Clonazione disco-a-disco (Attenzione: OF sovrascrive TUTTO senza chiedere!)
    dd if=/dev/sda of=/dev/sdb bs=64K conv=noerror,sync
    ```
* **L'importanza dell'Exit Code:** Dopo un `fsck` o un backup, controlla sempre `$?`. Un valore `0` significa successo, mentre `4` o superiore indica che i dati potrebbero essere ancora corrotti.



---

### **Q4: NFS - Come condividere spazio tra server Linux in modo trasparente?**

**Scenario:** Hai un server centrale con molto spazio e vuoi che 10 server web leggano le immagini da lì come se fossero su un disco locale.

* **Ragionamento Infrastrutturale:** NFS è il protocollo nativo Unix. È veloce e permette di mappare utenti remoti su utenti locali.
* **Configurazione Critica (Server):**
    Nel file `/etc/exports`: `/shared_dir *(rw,sync,no_root_squash)`
    * **sync:** Garantisce che il server confermi la scrittura solo quando i dati sono al sicuro sul disco.
    * **no_root_squash:** Permette all'admin del client di avere poteri di root anche sulla cartella remota (utile per backup o installazioni).
* **Verifica Client:** `showmount -e <IP_SERVER>` per vedere cosa offre il server prima di tentare il mount.

---

### **Q5: Samba - Perché è indispensabile in un ufficio con Windows e Linux?**

**Scenario:** Devi creare una cartella "Scambio" dove i grafici (su Mac/Windows) e i programmatori (su Linux) possano depositare i file.

* **Ragionamento di Interoperabilità:** NFS non è ben supportato da Windows. Samba implementa il protocollo **SMB/CIFS**, parlando la lingua madre di Windows.
* **Punti di Attenzione:**
    1. **SELinux:** È la causa n.1 dei fallimenti di Samba. Se non configurato (o disabilitato), impedirà a Samba di leggere i file anche se i permessi Linux sono 777.
    2. **Sintassi UNC:** In Windows accederai con `\\<IP_SERVER>\anonymous`. In Linux con `mount -t cifs`.
* **Sicurezza:** A differenza di NFS (basato su IP), Samba gestisce meglio l'autenticazione basata su utente/password, rendendolo ideale per reti aziendali miste.



---

### **Q6: Qual è la differenza tra "Backup del File System" e "Disaster Recovery"?**

**Scenario:** Il CEO ti chiede se siamo protetti in caso di incendio nel data center.

* **Analisi dei Livelli di Protezione:**
    * **Backup File System (`tar`, `rsync`):** Utile se un utente cancella per errore un file. Non salva il sistema operativo.
    * **Backup Database (`mysqldump`, `RMAN`):** Protegge l'integrità dei dati applicativi.
    * **Disaster Recovery (Immagini `dd`, Veeam, Snapshots):** L'unico modo per far ripartire l'intera azienda in poche ore clonando l'intero stato della macchina (OS + App + Dati).
* **Consiglio Professionale:** Un vero admin non si fida del backup finché non ha provato con successo il **Restore**.

---

### **Checklist di Verifica Finale**
- [ ] Il RAID è attivo? Controlla `/proc/mdstat` (se software).
- [ ] Il file `/etc/fstab` è configurato per montare NFS/Samba al boot?
- [ ] Hai testato la riparazione file system su un volume di test?
- [ ] Gli snapshot sono stati eseguiti prima di modifiche strutturali?

---

# 🚀 Modulo: Enterprise Storage, Database & OS Evolution

In questa sezione finale analizziamo la messa in sicurezza delle condivisioni, l'hardware dei server, la gestione dei dati con MariaDB e l'evoluzione storica delle distribuzioni Red Hat.

---

### **Q1: Condivisioni Protette - Come limitare l'accesso ai soli utenti autorizzati?**

**Scenario:** La cartella `[secured_share]` sul server Samba contiene dati sensibili. Solo l'utente `finance` deve potervi accedere.

* **Ragionamento Logico:** L'accesso Guest (anonimo) è un rischio. Samba necessita di un proprio database di password, separato da quello di sistema, anche se l'utente deve esistere in Linux.
* **Azione Operativa:**
    ```bash
    # 1. Crea l'utente nel sistema (senza accesso alla shell per sicurezza)
    useradd -s /sbin/nologin finance
    
    # 2. Registra l'utente in Samba (imposta la password di rete)
    smpasswd -a finance
    
    # 3. Configura /etc/samba/smb.conf:
    # [secured_share]
    #    path = /samba/protected_data
    #    valid users = finance
    #    guest ok = no
    #    writable = yes
    ```
* **Perché `smbpasswd`?** Samba non può leggere direttamente `/etc/shadow` per motivi di sicurezza e compatibilità di protocollo (NTLM/Kerberos).

---

### **Q2: NAS vs Server Linux - Quando preferire un dispositivo dedicato?**

**Scenario:** Un'azienda ha bisogno di 50TB di spazio condiviso tra uffici Windows e Linux, ma non ha un sistemista dedicato a tempo pieno.

* **Ragionamento Architetturale:** Un **NAS** (Network Attached Storage) offre un'interfaccia web semplificata e gestione RAID hardware/software integrata.
* **Flusso di Lavoro sul NAS:**
    1. **RAID:** Solitamente RAID 5 o 6 per tollerare il guasto di 1 o 2 dischi.
    2. **Protocolli:** Si abilitano contemporaneamente **NFS** (per i server Linux) e **SMB** (per i PC Windows).
* **Comando di Montaggio (Client Linux):**
    ```bash
    mount -t nfs 192.168.1.56:/volume1/peanuts /mnt/nas_data
    ```



---

### **Q3: SATA vs SAS - Quale disco scegliere per un Database ad alto traffico?**

**Scenario:** Devi acquistare i dischi per un server che gestirà migliaia di transazioni al secondo.

* **Ragionamento Hardware:** * **SATA:** Economico, alta capacità, ma comunicazione "half-duplex" (o legge o scrive). Ideale per archivio file.
    * **SAS:** Costoso, ma "full-duplex" (legge e scrive contemporaneamente) e molto più affidabile (MTBF superiore).
* **Analogia:** SATA è una strada a senso unico alternato; SAS è un'autostrada a più corsie. Per un Database, la scelta obbligata è **SAS**.

---

### **Q4: MariaDB Security - Quali sono i primi passi dopo l'installazione?**

**Scenario:** Hai appena installato MariaDB su un server web. Il database è attivo ma "aperto" a potenziali attacchi.

* **Ragionamento di Security Hardening:** L'installazione di default ha utenti anonimi e database di test che sono praterie per gli hacker.
* **Comando Critico:**
    ```bash
    mysql_secure_installation
    ```
* **Cosa risolve?**
    1. Imposta la password di root del DB.
    2. Rimuove l'accesso root da remoto (costringe a loggarsi solo dal server).
    3. Elimina gli utenti anonimi e il database `test`.



---

### **Q5: Evoluzione RHEL/CentOS - Perché è cambiato tutto con la versione 7 e 8?**

**Scenario:** Devi gestire un vecchio server CentOS 6 e uno nuovo CentOS 8. Ti accorgi che i comandi non funzionano allo stesso modo.

* **Tabella Rapida dei Cambiamenti:**

| Caratteristica | CentOS 6 (Legacy) | CentOS 7/8 (Modern) |
| :--- | :--- | :--- |
| **Init System** | SysVinit (`service`) | **systemd** (`systemctl`) |
| **Rete** | `ifconfig` (net-tools) | **`ip addr`** (iproute2) |
| **Orario** | `ntpd` | **`chronyd`** |
| **Gestore Pacchetti** | `yum` | **`dnf`** (su CentOS 8) |

* **Il grande cambio (CentOS Stream):** Dal 2021 CentOS non è più la "copia carbone" di RHEL (Downstream), ma è diventata la sua anteprima (Upstream). Per la massima stabilità aziendale ora si guarda a alternative come **Rocky Linux** o **AlmaLinux**, create dagli stessi fondatori di CentOS.



---

### **Riassunto dei Comandi Fondamentali di questo Modulo**

| Task | Comando |
| :--- | :--- |
| **Samba Password** | `smbpasswd -a utente` |
| **Check Mount** | `df -h` |
| **Check Protocollo** | `df -Th` |
| **Install DB** | `dnf install mariadb-server` |
| **Secure DB** | `mysql_secure_installation` |
| **Login DB** | `mysql -u root -p` |
