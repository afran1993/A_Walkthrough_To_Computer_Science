# Modulo 1: Introduzione a Linux e Architettura del Sistema

## 1. Linux nella vita quotidiana
Linux non è solo un sistema per server; è la spina dorsale della tecnologia moderna.

| Categoria | Descrizione |
| :--- | :--- |
| **Aereo** | Sistemi di intrattenimento in volo (IFE) basati su Linux integrato. |
| **Networking** | La quasi totalità dei router domestici utilizza kernel Linux (es. OpenWrt). |
| **Smart TV** | Interfacce come Android TV o WebOS sono basate su kernel Linux. |
| **Web & Cloud** | Google, Facebook e Amazon girano quasi interamente su infrastrutture Linux. |
| **Smartphone** | **Android** usa il kernel Linux. **iOS** (Apple) è invece basato su kernel Darwin/XNU (derivato Unix BSD), ma è software proprietario. |
| **Automotive** | Dalle Tesla ai sistemi di guida autonoma, Linux è lo standard per l'automotive. |
| **Supercomputer** | Il 100% dei primi 500 supercomputer più potenti al mondo usa Linux. |

---

## 2. Cos'è un Sistema Operativo (OS)
Un sistema operativo è il software "ponte" tra l'hardware (i circuiti) e l'utente.

### Tipologie di OS:
* **Desktop:** Microsoft Windows, macOS, Linux (es. Ubuntu, Fedora).
* **Server:** RHEL, Debian, Windows Server.
* **Mobile:** Android (open kernel), iOS (closed).
* **Embedded:** Sistemi integrati in elettrodomestici, router e auto.
* **RTOS (Real-Time):** Sistemi critici dove il tempo di risposta è vitale (medicina, difesa, centraline auto).

---

## 3. Cos'è Linux e brevi cenni storici
Linux è un sistema operativo **Unix-like**, gratuito e open-source.

* **1970:** Nasce **Unix** (AT&T, Bell Labs). Potente ma costoso e proprietario.
* **1983:** Richard Stallman avvia il **progetto GNU** per creare un sistema libero, ma mancava il "cuore" (il kernel).
* **1991:** **Linus Torvalds** scrive il kernel Linux e lo rilascia sotto licenza GPL.
* **Oggi:** Quello che chiamiamo "Linux" è solitamente l'unione del kernel Linux e delle utility GNU (**GNU/Linux**).

---

## 4. Distribuzioni Linux (Le "Distro")
Essendo open-source, chiunque può "impacchettare" il kernel con diversi software.

| Distribuzione | Focus principale | Ideale per... |
| :--- | :--- | :--- |
| **Ubuntu** | Facilità d'uso e supporto. | Principianti e Desktop. |
| **Debian** | Stabilità estrema e software libero. | Server e infrastrutture critiche. |
| **RHEL / Rocky** | Supporto aziendale e certificazioni. | Grandi aziende e Banche. |
| **Fedora** | Innovazione e pacchetti recentissimi. | Sviluppatori che vogliono l'avanguardia. |
| **Arch Linux** | Minimalismo e controllo totale. | Utenti esperti (Rolling Release). |
| **Kali Linux** | Sicurezza informatica. | Penetration Testing e Cybersec. |
| **Alpine** | Leggerezza estrema (pochi MB). | Container Docker e Microservizi. |

---

## 5. Architettura: Kernel, Shell e Utility

Un sistema operativo Linux è strutturato a "cipolla":



1.  **Kernel:** Il nucleo. Gestisce la memoria, i processi e parla con l'hardware.
2.  **Shell:** L'interprete di comandi. Prende i tuoi input e li spiega al kernel.
    * Esempi: `bash` (la più comune), `zsh`, `fish`.
3.  **Utility:** I programmi di sistema (es. `ls`, `cp`, `mount`).
4.  **Applicazioni:** Software utente (LibreOffice, Firefox, Docker).

---

## 6. Gestione della Memoria e Swap

Linux utilizza la **Memoria Virtuale** per permettere al sistema di eseguire programmi più grandi della RAM fisica disponibile.

### Lo Spazio di Swap
Quando la RAM è piena, il kernel sposta i dati meno usati sul disco rigido (nello "Swap").
* **Partizione di Swap:** Una sezione dedicata del disco (più performante).
* **File di Swap:** Un file (es. `/swapfile`) facile da ridimensionare.

> [!IMPORTANT]
> **Paging vs Swapping:** In termini tecnici, Linux effettua principalmente **Paging** (sposta piccole pagine di memoria da 4KB), anche se nel gergo comune si continua a usare il termine "Swap".

---

## 7. Linux vs Windows: Differenze Chiave

| Funzionalità | Linux | Windows |
| :--- | :--- | :--- |
| **Kernel** | Monolitico modulare | Ibrido |
| **Costo** | Gratuito (nella maggior parte dei casi) | Licenza a pagamento |
| **Configurazione** | Principalmente tramite file di testo | Registro di sistema e GUI |
| **Case Sensitivity** | `File.txt` e `file.txt` sono DIVERSI | Sono considerati uguali |
| **Gestione Software** | Gestore pacchetti (`apt`, `dnf`, `pacman`) | Installatori eseguibili (`.exe`, `.msi`) |
| **File System** | `ext4`, `XFS`, `Btrfs` | `NTFS`, `FAT32` |
