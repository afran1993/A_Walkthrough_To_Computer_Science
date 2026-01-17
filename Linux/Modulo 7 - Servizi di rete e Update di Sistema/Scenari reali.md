# 🌐 Linux Networking: Architettura, Configurazione e Troubleshooting

Questo documento riassume i concetti chiave del networking in ambiente Linux, trasformando la teoria in logica operativa per amministratori di sistema.

---

## 1. Il Modello Client-Server & Networking di Base
L'infrastruttura si basa sulla richiesta di risorse (**Client**) e sulla loro erogazione (**Server**). 

### Concetti Chiave
* **IP (Internet Protocol):** L'indirizzo logico univoco.
* **MAC Address:** L'identità fisica della scheda (NIC), immutabile.
* **Default Gateway:** Il "punto di uscita" della rete locale verso internet.
* **DNS:** Traduce nomi (www.google.it) in IP.

---

## 2. Configurazione e File Critici
In Linux, la rete si governa tramite file di testo in `/etc/`.

| File | Funzione |
| :--- | :--- |
| `/etc/hosts` | Risoluzione nomi locale (priorità su DNS). |
| `/etc/resolv.conf` | Configurazione dei server DNS. |
| `/etc/nsswitch.conf` | Ordine di ricerca (es. prima file hosts, poi DNS). |
| `/etc/sysconfig/network-scripts/` | Configurazione interfacce (IP statico vs DHCP). |



---

## 3. Strategie Avanzate: Alta Disponibilità e Performance

### NIC Bonding & Teaming
Unire più schede fisiche in un'unica interfaccia logica (`bond0` o `team0`).
* **Obiettivo 1: Ridondanza.** Se un cavo si rompe, il server resta online.
* **Obiettivo 2: Aggregazione di Banda.** Combinare due schede da 1Gbps per ottenere 2Gbps.

> **Nota Tecnica:** Il **Teaming** (introdotto con NetworkManager) è la versione moderna e più efficiente del vecchio Bonding.

---

## 4. Scenari Reali e Ragionamento Logico

### Scenario A: La VM è "isolata"
**Situazione:** Hai creato una macchina virtuale Linux. Riesci a fare il ping verso Google, ma dal tuo PC Windows non riesci a collegarti via SSH alla VM.
* **Cosa farei io?** Controllerei le impostazioni di VirtualBox. Se la rete è in **NAT**, la cambierei in **Bridged Adapter**.
* **Perché?** Il NAT crea una rete privata "nascosta" dietro l'host. Il Bridged Adapter mette la VM direttamente nella tua rete domestica/aziendale, assegnandole un IP visibile a tutti gli altri dispositivi.

### Scenario B: Database lento e critico
**Situazione:** Un server database SQL satura costantemente la banda di rete e non può permettersi downtime.
* **Cosa farei io?** Configurerei un **NIC Bonding in Modalità 5 (Balance-TLB)** o Modalità 4 (LACP).
* **Perché?** La Modalità 5 bilancia il traffico in uscita in base al carico delle schede. Se una scheda fallisce, il traffico passa sull'altra senza interrompere le transazioni SQL.

### Scenario C: Il sito non carica ma il server risponde
**Situazione:** Il comando `ping 192.168.1.50` risponde correttamente, ma se provi ad aprire il sito web ospitato su quel server, ricevi un errore.
* **Cosa farei io?** Userei `ss -tln` sul server e `curl -I [IP]` dal client.
* **Perché?** Il `ping` verifica solo se la macchina è accesa (Layer 3). `ss -tln` mi dice se il servizio Web è effettivamente in ascolto sulla porta 80/443. Se `ss` mostra la porta aperta ma `curl` fallisce, il problema è probabilmente un firewall che blocca il traffico applicativo.



---

## 5. Toolbox del Sistemista (Comandi Essenziali)

### Diagnostica Rapida
* `ip addr` / `ifconfig`: Verifica l'IP e lo stato dell'interfaccia.
* `ethtool [interfaccia]`: Verifica se il cavo è collegato (`Link detected: yes`) e la velocità reale della scheda.
* `ping -c 4 [IP]`: Test base di raggiungibilità.

### Analisi dei Socket con `ss` (Sostituto di netstat)
* `ss -t`: Mostra connessioni TCP attive.
* `ss -l`: Mostra servizi in ascolto (Listening).
* `ss -n`: Mostra porte numeriche (es. 22 invece di ssh).

### Trasferimento File via CLI
* `wget [URL]`: Scarica file in background.
* `curl -O [URL]`: Scarica file (ottimo per testare header HTTP).

---

## 6. Riassunto Strategico per l'Apprendimento
Per dominare il networking Linux non serve ricordare 3600 righe, ma seguire questo flusso logico di analisi:
1.  **Layer Fisico:** La scheda è accesa? (`ethtool`)
2.  **Layer IP:** La macchina ha l'indirizzo corretto? (`ip addr`)
3.  **Layer Routing:** La macchina sa dove uscire? (`ip route` / Gateway)
4.  **Layer Applicativo:** Il servizio è in ascolto sulla porta? (`ss -l`)

# 📂 Linux Data Management: Trasferimento, Pacchetti e Connettività

Questa guida esplora come muovere dati in sicurezza, gestire il software e garantire la continuità operativa del sistema.

---

## 1. Protocolli di Trasferimento: Quale scegliere?

In un'infrastruttura moderna, la scelta del protocollo dipende dal bilanciamento tra velocità e sicurezza.

| Protocollo | Porta | Caratteristica Chiave | Uso Consigliato |
| :--- | :--- | :--- | :--- |
| **FTP** | 21 | Standard, ma non criptato (insicuro). | Trasferimenti legacy in reti protette. |
| **SCP** | 22 | Sicuro (basato su SSH), semplice. | Copia rapida "one-shot" di singoli file. |
| **rsync** | 22 | Ultra-efficiente (trasferisce solo i delta). | Backup e sincronizzazione di grandi directory. |



---

## 2. Il Ciclo di Vita del Software: DNF vs RPM

Gestire i pacchetti significa capire la differenza tra un "esecutore" e un "organizzatore".

* **RPM (Basso livello):** Installa singoli file `.rpm`. Non scarica nulla da internet e non risolve le dipendenze (se manca un pezzo, si blocca).
* **DNF/YUM (Alto livello):** Il gestore intelligente. Dialoga con i **Repository** (archivi online), scarica tutto il necessario e risolve le dipendenze automaticamente.



---

## 3. Scenari Reali: Ragionamento Logico e Soluzioni

### Scenario E: Il Backup Notturno inefficiente
**Il problema:** Devi sincronizzare ogni notte una cartella di log da 50GB su un server remoto. Usando `scp`, il processo richiede ore anche se sono cambiati solo pochi MB di log.
* **Cosa farei io?** Sostituirei `scp` con `rsync -avz`.
* **Ragionamento logico:** `scp` ricopia tutto da zero ogni volta. `rsync` confronta i file e invia solo i bit modificati (delta transfer), riducendo il tempo di backup da ore a pochi minuti e risparmiando banda passante.

### Scenario F: Installazione in un Datacenter "Blindato" (Air-Gapped)
**Il problema:** Devi installare un server web su una macchina che, per motivi di sicurezza, non ha e non avrà mai accesso a Internet.
* **Cosa farei io?** Creerei un **Local Repository**. Monterei l'ISO di installazione di Linux e configurerei un file `.repo` in `/etc/yum.repos.d/` che punta al percorso del DVD (es. `baseurl=file:///mnt/cdrom`).
* **Ragionamento logico:** Anche senza internet, i pacchetti necessari sono spesso contenuti nell'ISO ufficiale. Indicizzando quei file con `createrepo`, permetto a `dnf` di funzionare localmente risolvendo le dipendenze dal disco invece che dal web.

### Scenario G: L'aggiornamento che rompe l'applicazione
**Il problema:** Dopo un `dnf update`, l'applicazione aziendale smette di funzionare a causa di una nuova versione di una libreria incompatibile.
* **Cosa farei io?** Se ho una VM, farei il **Revert allo Snapshot** scattato prima dell'update. Se sono su ferro (fisico), userei `dnf history undo [ID]`.
* **Ragionamento logico:** La cronologia di DNF permette di fare il "rollback" di una specifica transazione. Tuttavia, la best practice è sempre `update` (che mantiene le vecchie versioni) invece di `upgrade` (che le elimina), e l'uso di snapshot preventivi per il ripristino istantaneo del sistema.

### Scenario H: Troubleshooting di una connessione remota rifiutata
**Il problema:** Provi a connetterti via SSH a un server, ma ricevi "Connection Refused".
* **Cosa farei io?** 1. Verificherei lo stato del servizio con `systemctl status sshd`.
    2. Userei `telnet [IP] 22` (se installato) per vedere se la porta risponde.
* **Ragionamento logico:** "Connection Refused" significa che l'IP è raggiungibile (il ping risponde), ma non c'è nessun "postino" (demone SSH) pronto a ricevere sulla porta 22. Potrebbe essere il servizio spento o il firewall che non permette l'ingresso.



---

## 4. Toolbox: Comandi per la Manutenzione

### Gestione Pacchetti (DNF/RPM)
* `dnf repolist`: Mostra quali repository sono attivi.
* `rpm -qc [pacchetto]`: Trova i file di configurazione di un software (fondamentale per le modifiche).
* `rpm -qf /percorso/file`: "Di chi è questo file?". Identifica il pacchetto di origine di un comando.

### Trasferimento e Sincronizzazione
* `scp file.txt user@ip:/tmp`: Copia rapida.
* `rsync -ahvz --progress /src /dest`: Sincronizzazione avanzata con dettagli leggibili.

### Manutenzione e Patching
* `dnf history`: Mostra la lista di tutti gli aggiornamenti fatti.
* `cat /etc/redhat-release`: Verifica la versione esatta del sistema (fondamentale per il patching).

---

## 5. Sintesi per il System Administrator
Per gestire correttamente un parco server, ricorda:
1.  **Backup prima dei pacchetti:** Uno snapshot salva la carriera durante un `dnf update`.
2.  **Sicurezza sempre:** SSH è la norma; Telnet è solo un reperto archeologico per test diagnostici.
3.  **Efficienza rsync:** Non muovere mai più dati del necessario.
