# 🛠️ Metodologie di Progettazione del Laboratorio

Per l'apprendimento pratico, è necessario un ambiente isolato. Esistono due strade principali:

### 1. Virtualizzazione Locale (Host-Based)
Sfrutta l'hardware del tuo PC fisico per creare "computer virtuali".
* **Hypervisor:** Software come **Oracle VirtualBox** o **VMware Player**.
* **Vantaggio:** Controllo totale della rete e nessuna dipendenza da internet dopo il download.
* **Ideale per:** Corrispondenza esatta con gli standard di laboratorio.

### 2. Infrastruttura Cloud (IaaS)
Delega l'hardware a provider esterni (AWS, Google Cloud).
* **Metodo:** Creazione di istanze EC2 o Compute Engine.
* **Vantaggio:** Nessun carico sul tuo PC locale.
* **Ideale per:** Chi ha hardware datato o scarsa memoria RAM.

---

# 📦 Analisi di Oracle VirtualBox

VirtualBox è un **Type-2 Hypervisor**: si installa sopra un sistema operativo esistente (Host) per gestire sistemi ospiti (Guest).



### Architettura Logica
1.  **Livello Hardware:** CPU, RAM, Disco fisico.
2.  **OS Host:** Il tuo Windows/macOS/Linux principale.
3.  **VirtualBox:** Lo strato di astrazione.
4.  **OS Guest:** Le istanze isolate di CentOS, Ubuntu o RHEL.

> [!CAUTION]
> **Avviso di Rete:** Durante l'installazione di VirtualBox, la connessione internet dell'host verrà interrotta momentaneamente per configurare i bridge di rete virtuali.

---

# 🐧 Il Mondo CentOS: Da Downstream a Stream

È fondamentale capire cosa stai installando, poiché il ruolo di CentOS nell'ecosistema Red Hat è cambiato radicalmente nel 2021.

### L'Evoluzione della Pipeline
Storicamente, CentOS era una copia carbone di RHEL (Downstream). Oggi è il campo di prova per le versioni future (Upstream).



| Fase | Distribuzione | Ruolo |
| :--- | :--- | :--- |
| **Innovazione** | Fedora | Funzionalità sperimentali (Bleeding Edge). |
| **Preview** | **CentOS Stream** | Anteprima della prossima release RHEL. |
| **Stabilità** | **RHEL** | Standard Enterprise (Enterprise Grade). |

---

# 🚀 Guida Rapida all'Installazione (Best Practices)

Indipendentemente dalla versione (7, 8 o 9), segui questi standard per un laboratorio efficiente:

### 1. Configurazione Hardware Virtuale
Per far girare Linux con interfaccia grafica (GUI) senza rallentamenti, imposta questi parametri minimi:
* **RAM:** 2048 MB (2 GB).
* **Disco:** 20 GB (Allocazione dinamica VDI).
* **Rete:** Imposta su **"Bridged Adapter"** (Scheda con Bridge).

### 2. Check-list dell'Installer (Anaconda)
Durante l'installazione grafica, non dimenticare di:
* **Software Selection:** Cambiare "Minimal Install" in **"Server with GUI"**.
* **Network:** Attivare l'interruttore della scheda Ethernet (deve apparire "Connected").
* **Utenti:** Crea un utente standard e spunta **"Make this user administrator"**.

---

# 💡 Confronto tra Distribuzioni nel Corso

| Distro | Perché usarla? | Difficoltà Download |
| :--- | :--- | :--- |
| **CentOS 7** | Stabilità legacy, molto diffuso. | Semplice. |
| **CentOS 9 Stream** | Il futuro, allineato alle ultime tecnologie. | Semplice. |
| **RHEL 9** | Standard aziendale assoluto. | Media (richiede account Developer). |
| **Ubuntu** | Estremamente user-friendly, ottima community. | Molto semplice. |

---

# 🛠️ Risoluzione Problemi Comuni

> [!TIP]
> **Il mouse è bloccato nella VM?**
> Premi il tasto **CTRL Destro** sulla tastiera per rilasciare il puntatore del mouse dalla finestra di VirtualBox.

> [!NOTE]
> **VMware vs Broadcom**
> Dopo l'acquisizione di Broadcom, scaricare VMware Player è più complesso. Se sei alle prime armi, **Oracle VirtualBox** rimane la scelta più immediata e documentata.

---
**Prossimo Passo:** Una volta completata l'installazione, clicca su "Activities" e apri il **Terminale**. Sei pronto per inserire i tuoi primi comandi Linux!
