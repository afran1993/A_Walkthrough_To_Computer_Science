# 📂 Modulo 2: System Access & Filesystem Management

## 1. Fondamentali di Linux: Le Regole d'Oro

Prima di digitare comandi, è essenziale comprendere la filosofia del sistema.

* **L'Utente Root (Superuser):** Identificato dal simbolo `#`. Ha potere assoluto. 
    > [!CAUTION]
    > Un comando errato come `root` può distruggere l'intero OS. Usalo con estrema cautela.
* **Case Sensitivity:** Linux distingue tra `File.txt` e `file.txt`. Sono entità diverse.
* **Naming Convention:** Evita gli spazi. Usa l'underscore `_` o il trattino `-`.
* **CLI vs GUI:** In ambito Enterprise, Linux si gestisce via **CLI** (Command Line Interface) perché è più leggera, veloce e facilmente automatizzabile.

---

## 2. Accesso al Sistema: Locale vs Remoto

Esistono due modi per interagire con il kernel:



### A. Accesso Console (Fisico)
È il collegamento diretto (Monitor/Tastiera). In ambiente virtuale, è la finestra di **VirtualBox**. Si usa per la configurazione iniziale o il recupero del sistema.

### B. Accesso Remoto (SSH)
Il protocollo **SSH (Secure Shell)** sulla porta **22** è lo standard professionale.
* **Windows 10/11:** Usa il terminale nativo: `ssh username@IP_LINUX`.
* **Windows (Legacy):** Si usa **PuTTY**, un client leggero e gratuito.
* **macOS/Linux:** Usa il terminale integrato: `ssh -l username IP_LINUX`.

---

## 3. Anatomia del Command Prompt

Il prompt ti fornisce informazioni vitali sul tuo contesto operativo:

`root@LinuxServer [/etc]#`

1.  **Username:** `root` (chi sei).
2.  **Hostname:** `LinuxServer` (su quale macchina sei).
3.  **Directory:** `/etc` (dove ti trovi).
4.  **Simbolo Finale:** * `#` = Privilegi di Root.
    * `$` = Utente standard.

> [!TIP]
> **Terminale Bloccato?** Se un comando non restituisce il controllo, premi `CTRL + C` per inviare un segnale di interruzione (**SIGINT**).

---

## 4. Il Filesystem Linux: La Gerarchia Standard (FHS)

A differenza di Windows (C:\, D:\), Linux usa un'unica radice chiamata **Root** (`/`).



| Directory | Descrizione |
| :--- | :--- |
| **`/`** | **Radice:** Il punto di partenza di tutto il sistema. |
| **`/etc`** | **Configurazione:** Contiene i file per gestire servizi e sistema. |
| **`/root`** | **Home di Root:** La cartella privata dell'amministratore. |
| **`/home`** | **Home Utenti:** Dove risiedono i dati degli utenti comuni. |
| **`/var/log`**| **Log:** I registri delle attività e degli errori del sistema. |
| **`/bin`** | **Binari:** Comandi essenziali utilizzabili da tutti gli utenti. |
| **`/dev`** | **Dispositivi:** Rappresentazione hardware (dischi, tastiere, etc.). |

---

## 5. Navigazione e Proprietà dei File

Per muoverti nel sistema, devi padroneggiare i **Tre Comandi Re**:

1.  `pwd` (Print Working Directory): "Dove sono?".
2.  `ls -l` (List): "Cosa c'è qui?". Mostra anche i permessi.
3.  `cd` (Change Directory): "Spostami lì".

### Decodificare `ls -l`
Quando elenchi i file, il primo carattere indica il **Tipo**:
* `-` : File regolare.
* `d` : Directory (cartella).
* `l` : Link (collegamento).
* `c` / `b` : Dispositivi hardware.

> [!IMPORTANT]
> **Password Invisibile:** Quando digiti la password dopo il comando `passwd` o durante il login, **non vedrai asterischi**. Il cursore non si muoverà. Digita e premi Invio.

---

## 6. Percorsi Assoluti vs Relativi

* **Percorso Assoluto:** Inizia sempre con `/`. Funziona ovunque tu sia. 
    * *Esempio:* `cd /var/log`
* **Percorso Relativo:** Parte dalla tua posizione attuale. 
    * *Esempio:* Se sei in `/var`, digita `cd log`.

---
**Esercizio Pratico:** Prova a navigare fino a `/etc` usando un percorso assoluto, verifica la tua posizione con `pwd` e visualizza i file di configurazione con `ls -l`. Quale file ti sembra più interessante?
