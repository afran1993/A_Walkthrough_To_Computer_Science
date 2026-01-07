# Modulo 1 – Introduzione a Linux

## Linux nella vita quotidiana

Linux è molto più presente nella nostra vita di quanto immaginiamo. Spesso lavora dietro le quinte, senza che l’utente finale ne sia consapevole.

Ecco alcuni esempi concreti:

| Categoria                        | Descrizione                                                                                           |
| :------------------------------- | :---------------------------------------------------------------------------------------------------- |
| **Aereo**                        | I sistemi di intrattenimento di bordo (film, musica, mappe di volo) girano su sistemi Linux embedded. |
| **Accesso a Internet**           | La maggior parte dei router Wi‑Fi utilizza Linux per gestire rete, firewall e interfaccia web.        |
| **TV**                           | Le Smart TV eseguono spesso versioni personalizzate di Linux per l’interfaccia utente e le app.       |
| **Ricerca Google**               | L’infrastruttura backend che elabora le ricerche è basata quasi interamente su Linux.                 |
| **Siti web**                     | La maggior parte dei siti web mondiali è ospitata su server Linux.                                    |
| **Smartphone**                   | Android è basato sul kernel Linux; anche iOS deriva da un sistema Unix-like.                          |
| **Tablet ed e‑reader**           | Molti dispositivi integrano componenti software open source basati su Linux.                          |
| **Dischi di ripristino Windows** | Spesso sono distribuzioni Linux personalizzate avviabili da USB o DVD.                                |
| **Elettrodomestici**             | Forni, lavatrici e frigoriferi smart con touchscreen utilizzano Linux.                                |
| **Auto a guida autonoma**        | I sistemi di controllo e infotainment sono basati su Linux.                                           |

---

## Cos’è un sistema operativo

Un **sistema operativo** (OS – *Operating System*) è il software che fa da ponte tra l’hardware del computer e l’utente. Fornisce un’interfaccia per interagire con la macchina e gestisce tutte le risorse hardware affinché i programmi possano funzionare correttamente.

In altre parole, senza un sistema operativo l’hardware sarebbe inutilizzabile.

### Tipologie di sistemi operativi

Esistono diverse categorie di sistemi operativi:

* **Desktop**: Microsoft Windows, macOS, Linux (Ubuntu, Fedora, ecc.)
* **Server**: Windows Server, Red Hat Enterprise Linux, CentOS, Debian
* **Mobile**: Android, iOS
* **Embedded**: router, smart TV, automobili, elettrodomestici
* **Real-Time (RTOS)**: sistemi critici come dispositivi medici, avionica, difesa, firewall di rete, centraline elettroniche

---

## Cos’è Linux

Linux è un **sistema operativo libero e open source**. È simile a Windows e macOS per scopo, ma differisce profondamente per filosofia e modalità di utilizzo.

Le sue caratteristiche principali sono:

* **Gratuito e open source**
* **Stabile e sicuro**
* **Altamente personalizzabile**
* **Sviluppato e mantenuto da una community globale**

Linux non è una singola entità, ma un insieme di sistemi chiamati **distribuzioni** (*distro*), ognuna progettata per scopi specifici: desktop, server, cloud, sicurezza, embedded.

È il sistema operativo più utilizzato nel mondo **server e cloud** ed è fortemente orientato all’uso della **linea di comando (CLI)**.

Conoscere Linux significa:

* comprendere meglio il funzionamento dei sistemi operativi
* aumentare la sicurezza dei sistemi
* migliorare le prestazioni
* ampliare le opportunità professionali

---

## Breve storia di Linux

* **1970** – Nasce Unix, sviluppato da AT&T
* **1983** – Richard Stallman avvia il progetto **GNU** (*GNU’s Not Unix*)
* **1991** – Linus Torvalds sviluppa il **kernel Linux**
* L’unione del kernel Linux con gli strumenti GNU dà origine ai sistemi GNU/Linux

Da quel momento Linux si è evoluto rapidamente fino a diventare lo standard de facto per server, supercomputer e cloud computing.

---

## Differenze tra Unix e Linux

| Unix                         | Linux                                     |
| :--------------------------- | :---------------------------------------- |
| Nato negli anni ’70          | Nato nel 1991                             |
| Proprietario e a pagamento   | Gratuito e open source                    |
| Sviluppato da AT&T e altri   | Creato da Linus Torvalds e community      |
| Usato in Solaris, AIX, HP‑UX | Usato in Red Hat, Debian, Ubuntu, ecc.    |
| Supporto hardware limitato   | Supporta una vastissima gamma di hardware |

Linux è oggi molto più diffuso grazie alla sua flessibilità e al costo nullo.

---

## Principali distribuzioni Linux

| Distribuzione     | Descrizione                           | Scopo                     |
| :---------------- | :------------------------------------ | :------------------------ |
| **Ubuntu**        | Basata su Debian, pronta all’uso      | Desktop, sviluppo         |
| **Fedora**        | Tecnologie all’avanguardia            | Sperimentazione, sviluppo |
| **Debian**        | Estrema stabilità                     | Server                    |
| **RHEL**          | Distribuzione enterprise con supporto | Aziende, banche           |
| **CentOS Stream** | Anteprima continua di RHEL            | Test e studio             |
| **Arch Linux**    | Minimalista e rolling release         | Utenti esperti            |
| **openSUSE**      | Configurazione avanzata con YaST      | Desktop e server          |
| **Linux Mint**    | User‑friendly                         | Utenti Windows            |
| **Gentoo**        | Compilazione dai sorgenti             | Prestazioni e studio      |
| **Slackware**     | Filosofia Unix pura                   | Puristi                   |
| **Alpine Linux**  | Leggera e sicura                      | Container Docker          |
| **Kali Linux**    | Tool di sicurezza preinstallati       | Cybersecurity             |

---

## Utenti di Linux

Linux è utilizzato da:

* sviluppatori e sistemisti
* università e centri di ricerca
* governi e agenzie pubbliche
* aziende tecnologiche
* cloud provider e data center
* supercomputer e infrastrutture di rete
* piattaforme media come Netflix

---

## Linux vs Windows

| Linux                                 | Windows                 |
| :------------------------------------ | :---------------------- |
| Open source                           | Proprietario            |
| Gratuito                              | A pagamento             |
| CLI centrale                          | GUI predominante        |
| Gestione pacchetti (APT, YUM, Pacman) | Installer `.exe`        |
| Molto sicuro                          | Più esposto a malware   |
| Alte prestazioni e stabilità          | Può degradare nel tempo |
| ext4, XFS, Btrfs                      | NTFS, FAT32             |

---

## Parti di un sistema operativo

Un sistema Unix-like è composto da:

* **Kernel**
* **Programmi di sistema**
* **Applicazioni**

Il **kernel** è il cuore del sistema: gestisce memoria, processi, filesystem, rete e dispositivi hardware. I programmi di sistema e le applicazioni operano in **user mode**, utilizzando i servizi del kernel tramite le **system call**.

---

## Componenti principali del kernel Linux

* Gestione dei processi
* Gestione della memoria
* Driver hardware
* File system
* Networking

Le componenti più critiche sono la gestione della memoria e dei processi, che permettono il multitasking e l’uso efficiente delle risorse.

---

## Memoria virtuale e swap

Linux utilizza la **memoria virtuale**, estendendo la RAM tramite lo spazio di **swap** su disco.

* I dati meno usati vengono temporaneamente spostati su disco
* Il processo è trasparente per l’utente
* Lo swap è più lento della RAM ma permette maggiore flessibilità

### Swap: partizione vs file

* **Partizione di swap**: più veloce, meno flessibile
* **File di swap**: facile da creare e ridimensionare

Linux consente l’uso simultaneo di più swap.

---

## Dentro Linux

### Kernel

* Caricato all’avvio del sistema
* Sempre residente in memoria
* Gestisce CPU, memoria, dispositivi e permessi

### Shell

* Interprete dei comandi
* Interfaccia tra utente e kernel
* Include un linguaggio di scripting

Shell più comuni:

* sh, csh, ksh, tcsh
* **bash** (la più diffusa)

### Utility

* Centinaia di comandi standard
* Programmi modulari
* Combinabili tramite pipe per creare flussi complessi

---

Questo modulo fornisce le basi teoriche necessarie per comprendere Linux e il suo ruolo centrale nei sistemi moderni.
