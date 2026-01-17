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

# 🌐 Linux Infrastructure Services: DNS, Time, Mail & Web

Questa sezione copre i servizi che permettono a una rete di funzionare in modo coordinato, sincronizzato e comunicativo.

---

## 1. DNS (Domain Name System): La Rubrica della Rete

Il DNS traduce i nomi leggibili (google.com) in indirizzi IP. 

### Record Fondamentali da Conoscere
* **A Record:** Nome ➡️ IP (es. `server1` ➡️ `192.168.1.10`).
* **PTR Record:** IP ➡️ Nome (Reverse Lookup, essenziale per i server mail).
* **CNAME:** Alias (es. `www` è un alias di `webserver-prod-01`).

### Componenti BIND (Berkeley Internet Name Domain)
* **Servizio:** `named`
* **Configurazione:** `/etc/named.conf`
* **Database (Zone):** `/var/named/`



---

## 2. Sincronizzazione Temporale: NTP & Chrony

In un'infrastruttura, il tempo è tutto. Se i server non sono sincronizzati, i log sono inutili e l'autenticazione (es. Kerberos/Active Directory) fallisce.

| Strumento | Descrizione | Comando di Controllo |
| :--- | :--- | :--- |
| **NTPD** | Il demone tradizionale (legacy). | `ntpq -p` |
| **Chrony** | Moderno, veloce, standard in RHEL/CentOS 8+. | `chronyc sources -v` |
| **timedatectl** | L'interfaccia universale di systemd. | `timedatectl status` |



---

## 3. Scenari Reali: Ragionamento Logico e Soluzioni

### Scenario I: "Raggiungo il sito solo tramite IP"
**Il problema:** Un utente riesce a navigare su un server interno digitando `10.0.0.50`, ma riceve "Sito non trovato" se usa `app.lab.local`.
* **Cosa farei io?** 1. Verificherei il file `/etc/resolv.conf` del client per vedere se punta al server DNS corretto.
    2. Userei `dig app.lab.local` per vedere se il server DNS restituisce un record A.
* **Ragionamento logico:** Se l'IP risponde, la rete funziona. Il problema è puramente di traduzione. Se `dig` fallisce, il record non esiste sul server; se `dig` funziona ma il browser no, il client sta usando il DNS sbagliato.

### Scenario J: Errori di Log-in misteriosi
**Il problema:** Gli utenti non riescono a loggarsi nel dominio aziendale. Controllando i log, noti che i timestamp dei server sono sfasati di 10 minuti.
* **Cosa farei io?** Eseguirei `chronyc sources -v` su tutti i server coinvolti.
* **Ragionamento logico:** Molti protocolli di sicurezza rifiutano connessioni se la differenza di tempo (skew) è superiore a 5 minuti per evitare attacchi di tipo "replay". Mi assicurerei che tutti i server puntino allo stesso **NTP Pool** o al Gateway aziendale.

### Scenario K: Il Web Server mostra la pagina di test di Apache
**Il problema:** Hai installato Apache (`httpd`), ma invece di vedere il tuo sito, vedi la pagina predefinita di CentOS/Red Hat.
* **Cosa farei io?** Controllerei la directory `/var/www/html/` per verificare la presenza del file `index.html`.
* **Ragionamento logico:** Apache cerca un file chiamato esattamente `index.html` (o quello definito in `httpd.conf`). Se la cartella è vuota o il file ha un nome diverso (es. `home.html`), Apache mostra la sua pagina di cortesia.

---

## 4. Servizi di Comunicazione: Postfix (Mail)

Postfix è il postino di Linux. In azienda si usa spesso come **Relay Host**.

* **Perché il Relay?** Invece di far uscire ogni singolo server su internet (rischioso), tutti i server interni inviano le mail a un unico server "Relay" autorizzato.
* **Configurazione:** Si imposta nel file `/etc/postfix/main.cf` alla voce `relayhost = [smtp.aziendale.it]`.



---

## 5. Toolbox: Troubleshooting dei Servizi

### DNS
* `named-checkconf`: Controlla se hai fatto errori di sintassi (punto e virgola mancanti) nel config.
* `nslookup [dominio]`: Test rapido.
* `dig [dominio]`: Test dettagliato (mostra il TTL e i server autorevoli).

### TEMPO
* `timedatectl set-timezone Europe/Rome`: Cambia il fuso orario istantaneamente.
* `chronyc tracking`: Mostra quanto è preciso l'orologio rispetto alla sorgente.

### WEB (Apache)
* `apachectl configtest`: Verifica errori nel file `httpd.conf` prima di riavviare.
* `tail -f /var/log/httpd/error_log`: Guarda in tempo reale perché il sito crasha.

### MAIL
* `mail -s "Oggetto" utente@mail.it`: Invia una mail rapida da terminale.
* `postqueue -p`: Visualizza la coda delle mail (se una mail non parte, è bloccata qui).

---

## 💡 Conclusione Strategica
Ricorda la gerarchia delle dipendenze:
1. Senza **Tempo** sincronizzato, i certificati SSL del Web e l'autenticazione falliscono.
2. Senza **DNS**, gli utenti non troveranno mai il tuo server Web o Mail.
3. Se il **Firewall** è attivo, ricorda di aprire le porte: **80/443 (Web)**, **53 (DNS)**, **123 (NTP)** e **25 (Mail)**.

---

# 🚀 Advanced Services, Monitoring & System Hardening

Questa sezione copre l'uso di Nginx come Proxy, la centralizzazione dei log, il monitoraggio con Nagios e le strategie per rendere un server Linux inattaccabile.

---

## 1. Nginx: Oltre il Web Server (Reverse Proxy)

Nginx è il "coltellino svizzero" del traffico web. Mentre Apache è un ottimo server per contenuti, Nginx eccelle nel gestire migliaia di connessioni come **Reverse Proxy**.

### Perché usare un Reverse Proxy?
1. **Sicurezza:** Nasconde l'identità e l'IP dei server reali (Backend).
2. **Performance:** Può fare da "Load Balancer" smistando il traffico tra più server.
3. **Flessibilità:** Gestisce i certificati SSL in un unico punto.



---

## 2. Centralized Logging: Rsyslog

In un'azienda con molti server, controllare i log macchina per macchina è inefficiente e insicuro (un hacker potrebbe cancellare le tracce locali).

| Componente | Protocollo | Porta | Caratteristica |
| :--- | :--- | :--- | :--- |
| **UDP (@)** | UDP | 514 | Veloce, ma può perdere pacchetti. |
| **TCP (@@)** | TCP | 514 | Affidabile, garantisce la ricezione dei log. |

> **Logica Forense:** Inviare i log a un server centrale significa che, anche se un server viene compromesso o distrutto, le prove dell'attacco rimangono al sicuro sul Central Logger.



---

## 3. Monitoraggio Professionale: Nagios Core

Nagios è il "guardiano" che non dorme mai. Ti avvisa prima che l'utente si accorga che il servizio è giù.

* **Check Attivi:** Nagios interroga il server (es. "Sei vivo? Hai spazio su disco?").
* **Notifiche:** Email o SMS quando uno stato passa da OK a CRITICAL.
* **Plugin:** La forza di Nagios; esistono script per monitorare qualsiasi cosa (database, temperatura, ventole).



---

## 4. Scenari Reali: Ragionamento Logico e Soluzioni

### Scenario L: Nginx restituisce "502 Bad Gateway"
**Il problema:** Nginx è configurato come proxy, ma invece del sito vedi un errore 502. Nei log leggi: `Permission denied while connecting to upstream`.
* **Cosa farei io?** Controllerei subito SELinux con `getsebool httpd_can_network_connect`. Se è OFF, lo abiliterei con `setsebool -P`.
* **Ragionamento logico:** Per impostazione predefinita, SELinux impedisce ai server web di fare connessioni verso l'esterno. Se Nginx deve parlare con un backend, devi esplicitamente dirgli che è autorizzato a farlo.

### Scenario M: Un utente ha cancellato i propri log per errore?
**Il problema:** Devi indagare su un errore avvenuto ieri, ma il file `/var/log/messages` sul server è vuoto o corrotto.
* **Cosa farei io?** Andrei sul server **Central Logger** e cercherei nella directory dedicata a quel client.
* **Ragionamento logico:** Grazie a Rsyslog, ogni riga di log viene duplicata istantaneamente sul server centrale. La cancellazione locale (accidentale o dolosa) non influisce sulla copia remota.

### Scenario N: SSH è sotto attacco Brute-Force
**Il problema:** Noti migliaia di tentativi di accesso falliti nei log provenienti da IP sconosciuti sulla porta 22.
* **Cosa farei io?** 1. Cambierei la porta SSH da 22 a una alta (es. 1022). 2. Imposterei `PermitRootLogin no`.
* **Ragionamento logico:** Gli hacker scansionano principalmente la porta 22. Spostando il servizio e obbligando l'uso di un utente standard (che poi farà `sudo`), rendi il server un bersaglio molto più difficile e tracciabile.

---

## 5. Checklist per l'Hardening (Messa in Sicurezza)

Un server "appena installato" non è sicuro. Ecco i passaggi obbligatori per un sistemista:

### 🛡️ Livello Identità
- [ ] **UID Elevati:** Assegna UID > 10.000 per utenti umani per distinguerli da quelli di sistema.
- [ ] **Password Policy:** Minimo 12-13 caratteri (configurabile in `/etc/login.defs`).
- [ ] **No Root Login:** Impedisci l'accesso diretto via SSH come root.

### 🛡️ Livello Servizi
- [ ] **Minimalismo:** Rimuovi ogni pacchetto inutile (`yum remove`). Meno software = meno bug da sfruttare.
- [ ] **Port Monitoring:** Controlla costantemente chi sta ascoltando con `netstat -tunlp`.

### 🛡️ Livello Kernel & Rete
- [ ] **SELinux Enforcing:** Non disabilitarlo mai in produzione. Usa `audit2allow` per risolvere i blocchi.
- [ ] **Firewalld:** Politica "Deny All" (blocca tutto) e apri solo lo stretto necessario (80, 443, porta SSH custom).



---

## 6. Toolbox Rapido

### Nginx & Servizi
* `nginx -t`: **Sempre** prima di riavviare (controlla gli errori di virgole o parentesi).
* `lsof -i :80`: Chi sta occupando la porta del Web Server?

### Log & Debug
* `logger "Messaggio di test"`: Genera un log manuale per vedere se Rsyslog lo spedisce correttamente.
* `tail -f /var/log/nginx/error.log`: La tua finestra principale sul mondo dei problemi web.

### Nagios
* `/usr/local/nagios/bin/nagios -v /.../nagios.cfg`: Verifica che i file di configurazione siano corretti.

---

# 🆔 Identity, Remote Access & Firewall Defense

In questa sezione esploreremo come gestire migliaia di utenti con OpenLDAP, come rendere SSH un fortino inattaccabile e come configurare i "muri" di protezione del sistema (Firewalls).

---

## 1. Gestione Centralizzata: OpenLDAP

Mentre un database locale (`/etc/passwd`) va bene per un singolo server, **OpenLDAP** è la soluzione per le grandi infrastrutture. È il "motore di ricerca" delle identità aziendali.

### Concetti Chiave
* **DIT (Directory Information Tree):** I dati in LDAP non sono tabelle, ma rami di un albero.
* **NSS (Name Service Switch):** È il vigile urbano nel file `/etc/nsswitch.conf`. Dice al sistema: "Prima guarda se l'utente è locale nei `files`, se non lo trovi, chiedi a `ldap`".



---

## 2. Diagnostica di Rete: Traceroute

Se il `ping` ti dice *se* una destinazione è viva, `traceroute` ti dice *dove* la connessione si interrompe.

* **Hop (Salto):** Ogni router che il pacchetto attraversa.
* **Latenza:** Se un hop intermedio mostra tempi altissimi (es. >200ms), hai trovato il collo di bottiglia.
* **Firewall Silenziosi:** Se vedi `* * *`, significa che quel router scarta i pacchetti di diagnostica per sicurezza.



---

## 3. SSH: La Porta del Regno (Security & Keys)

SSH è il servizio più critico. Se un hacker entra qui, ha il controllo totale.

### Hardening Strategico
1. **No Password:** Usa le chiavi RSA. La chiave privata è la tua impronta digitale, la chiave pubblica è la serratura sul server.
2. **PermitRootLogin no:** Obbliga gli amministratori a entrare come utenti normali. Questo crea un "audit trail" (si sa chi ha fatto cosa prima di diventare root).
3. **Timeout:** Chiudi le sessioni dimenticate aperte (`ClientAliveInterval`).



---

## 4. Cockpit: L'Interfaccia Moderna

Per chi preferisce la gestione visiva, **Cockpit** trasforma il browser in una console di amministrazione completa. È ideale per monitorare graficamente il carico di lavoro senza dover ricordare ogni singolo comando `top` o `iostat`.

---

## 5. Il Muro: Iptables vs Firewalld

Linux offre due modi per gestire i filtri dei pacchetti. Entrambi agiscono sul kernel, ma hanno "filosofie" diverse.

| Caratteristica | Iptables (Old School) | Firewalld (Moderno) |
| :--- | :--- | :--- |
| **Logica** | Catene rigide (INPUT, OUTPUT) | Zone di fiducia (Public, Internal) |
| **Flessibilità** | Richiede riavvio per ogni modifica | Modifiche "Runtime" e "Permanent" |
| **Complessità** | Alta, sintassi granulare | Media, basata su servizi (es. `--add-service=http`) |

### Il Flusso dei Pacchetti
Immagina il firewall come un checkpoint doganale:
* **INPUT:** Controllo bagagli in entrata.
* **FORWARD:** Transito verso un'altra destinazione.
* **OUTPUT:** Controllo merci in uscita.



---

## 6. Scenari Reali: Troubleshooting Avanzato

### Scenario O: "Raggiungo il server ma non vedo il sito"
**Il problema:** Il server risponde al `ping` e a `ssh`, ma il browser dà "Timeout".
* **Cosa farei io?** Controllerei il firewall: `firewall-cmd --list-all`. Se il servizio `http` o la porta `80` non sono in elenco, li aggiungerei.
* **Ragionamento logico:** Il ping usa ICMP, SSH usa la porta 22. Se questi funzionano, la rete è ok. È il firewall che sta bloccando specificamente il traffico web.

### Scenario P: "Lo script non riesce a copiare i file sul server remoto"
**Il problema:** Hai uno script di backup che usa `scp`, ma si blocca perché chiede la password.
* **Cosa farei io?** Genererei una coppia di chiavi con `ssh-keygen` (senza passphrase) e userei `ssh-copy-id` verso il server remoto.
* **Ragionamento logico:** Gli script non possono "digitare" una password. L'autenticazione basata su chiavi è l'unico modo sicuro per permettere a due macchine di parlarsi in autonomia.

### Scenario Q: "Perché il server è lento a rispondere via SSH?"
**Il problema:** Quando ti connetti via SSH, passano 10-20 secondi prima che appaia la richiesta della password.
* **Cosa farei io?** Disabiliterei il DNS inverso in SSH impostando `UseDNS no` in `/etc/ssh/sshd_config`.
* **Ragionamento logico:** Spesso SSH cerca di capire il nome dell'IP da cui ti colleghi. Se il DNS del server è lento o mal configurato, la sessione rimane "appesa" in attesa della risposta.

---

## 7. Toolbox Rapido di Difesa

* `firewall-cmd --add-port=9090/tcp --permanent`: Apre la porta di Cockpit per sempre.
* `iptables -L -n -v`: Mostra esattamente quanti pacchetti sono stati bloccati da ogni regola.
* `ssh-keygen -t rsa -b 4096`: Genera una chiave ultra-sicura a 4096 bit.
* `lastb`: Mostra gli ultimi tentativi di accesso falliti (utile per individuare attacchi brute-force).

---

# ⚡ Optimization, Containers & Massive Automation

Questa sezione esplora come spremere ogni goccia di potenza dal server, come isolare le applicazioni con i container e come gestire migliaia di macchine premendo un solo tasto.

---

## 1. Ottimizzazione delle Performance: Tuned e Priorità

Un server non è mai "pari" a un altro. Un server database ha bisogno di velocità di scrittura, un server web di velocità di risposta.

### Tuned: L'Ottimizzatore Intelligente
Invece di cambiare manualmente centinaia di parametri del kernel, usiamo **Tuned**.
* **Profilo `latency-performance`:** Ideale per server che devono rispondere istantaneamente.
* **Profilo `throughput-performance`:** Ideale per server che spostano grandi masse di dati.

### Nice & Renice: La "Cortesia" dei Processi
Il valore di **Nice** determina quanto un processo è "gentile" con gli altri:
* **Valore -20:** "Maleducato". Prende tutta la CPU che può.
* **Valore +19:** "Gentilissimo". Aspetta che tutti gli altri abbiano finito.



---

## 2. La Rivoluzione dei Container (Podman & Docker)

Il container è l'evoluzione della Virtual Machine. Mentre una VM simula un intero hardware (molto pesante), il container isola solo l'applicazione e le sue librerie.

### Perché usare i Container?
1. **Portabilità:** "Funziona sulla mia macchina" ora significa "Funziona ovunque".
2. **Leggerezza:** Puoi avviare un container in meno di un secondo.
3. **Isolazione:** Se un container viene compromesso o crasha, gli altri rimangono intatti.

| Caratteristica | Virtual Machine (VM) | Container (Podman/Docker) |
| :--- | :--- | :--- |
| **OS** | Ogni VM ha il suo OS completo | Condividono il Kernel dell'Host |
| **Peso** | Gigabyte | Megabyte |
| **Avvio** | Minuti | Secondi |



---

## 3. Automazione: Kickstart & Ansible

Un bravo amministratore di sistema è "pigro": non vuole ripetere lo stesso compito due volte.

### Kickstart: Installazione OS "Mani Libere"
Kickstart permette di installare Linux su centinaia di server contemporaneamente. Tutte le risposte (password, partizioni, fuso orario) sono scritte nel file `ks.cfg`.

### Ansible: Infrastructure as Code (IaC)
Ansible è il re dell'automazione moderna. È **Agentless**: non devi installare nulla sui server remoti, basta SSH.
* **Inventory:** La lista dei tuoi server.
* **Playbook:** La "ricetta" (in YAML) di cosa vuoi fare.



---

## 4. Scenari Reali: Ragionamento Logico e Soluzioni

### Scenario R: "Il processo di backup sta rallentando il sito web"
**Il problema:** Ogni notte il backup parte e il server web diventa lentissimo.
* **Cosa farei io?** Userei `renice` per aumentare il valore di nice del processo di backup (es. portarlo a +15).
* **Ragionamento logico:** Il backup è un'operazione che può metterci più tempo senza danni, mentre il server web deve essere reattivo. Dando "meno priorità" al backup, la CPU darà la precedenza alle richieste web.

### Scenario S: "Devo installare la stessa patch su 500 server entro un'ora"
**Il problema:** Una vulnerabilità critica richiede un aggiornamento immediato su tutta la flotta aziendale.
* **Cosa farei io?** Scriverei un Playbook Ansible di 5 righe e lo lancerei contro l'intero Inventory.
* **Ragionamento logico:** Farlo manualmente richiederebbe giorni e porterebbe a errori umani. Ansible garantisce che tutti i 500 server siano configurati esattamente nello stesso modo (Idempotenza).

### Scenario T: "Un container Podman non comunica con l'esterno"
**Il problema:** Hai lanciato un container Apache sulla porta 80, ma l'IP del server non risponde sulla porta 8080.
* **Cosa farei io?** Controllerei la mappatura delle porte nel comando `podman run -p 8080:80`. Verificherei anche se il firewall del server ospite permette il traffico sulla 8080.
* **Ragionamento logico:** Il container vive in una rete isolata. Se non "esponi" correttamente la porta verso l'esterno (`host_port:container_port`), il traffico non arriverà mai all'applicazione.

---

## 5. Checklist del Sistemista Automatizzato

### ⚙️ Ottimizzazione
- [ ] Il profilo Tuned è adatto al ruolo del server? (`tuned-adm active`)
- [ ] Ci sono processi critici che necessitano di un valore Nice negativo?

### 📦 Container
- [ ] Sto usando Podman (su RHEL/CentOS 8+) per maggiore sicurezza (rootless)?
- [ ] Ho pulito le immagini vecchie per non esaurire lo spazio disco? (`podman image prune`)

### 🤖 Automazione
- [ ] Il file Kickstart è accessibile via HTTP per le nuove installazioni?
- [ ] Ho testato i Playbook Ansible in un ambiente di "Staging" prima di lanciarli in produzione?

---

## 6. Toolbox Rapido

* `tuned-adm recommend`: Chiedi al sistema qual è la sua configurazione ideale.
* `podman ps -a`: Vedi tutti i container, anche quelli che si sono fermati per un errore.
* `ansible -m setup <hostname>`: Il comando "Grep" dei campioni; raccoglie ogni singola informazione hardware e software del server remoto.
* `ansible-playbook --check`: Esegue una simulazione del Playbook senza modificare nulla (Dry Run).

---

# 🌐 Network Services: VPN, DHCP & Proxy Filtering

In quest'ultimo modulo vedremo come trasformare un server Linux in un perno della connettività aziendale, garantendo sicurezza agli utenti remoti e controllo sulla navigazione interna.

---

## 1. Connessioni Sicure: OpenVPN

OpenVPN non è solo un software, è uno standard di sicurezza. Crea un "tunnel" crittografato che fa sentire il dipendente a casa come se fosse seduto in ufficio, protetto da occhi indiscreti.

### La PKI (Public Key Infrastructure)
Immagina la PKI come un **Ufficio Passaporti digitale**:
* **CA (Certificate Authority):** L'ufficio che stampa i passaporti.
* **Certificato Server:** Il documento del server che dice "Sono davvero il tuo ufficio".
* **Certificato Client:** Il documento dell'utente che dice "Sono un dipendente autorizzato".



> **Nota Tecnica:** Il passaggio `net.ipv4.ip_forward = 1` è fondamentale. Senza di esso, il traffico arriva al server VPN ma "muore" lì, non riuscendo a raggiungere il resto della rete aziendale o internet.

---

## 2. Automazione degli Indirizzi: DHCP Server

Il DHCP è il "Vigile Urbano" della rete. Quando un dispositivo si collega, il server DHCP gli assegna una targa (IP), una mappa (Gateway) e una rubrica (DNS).

### Il processo DORA
Il protocollo lavora in quattro fasi:
1. **D**iscovery: Il client grida "C'è qualcuno?".
2. **O**ffer: Il server risponde "Io ho l'IP 192.168.1.50 libero".
3. **R**equest: Il client dice "Ok, prendo quello!".
4. **A**ck (Acknowledgment): Il server conferma "Segnato, è tuo per le prossime 2 ore".



---

## 3. Controllo e Velocità: Squid Proxy

Squid agisce come un **Filtro e una Memoria**. Se 50 dipendenti guardano lo stesso video di formazione su YouTube, Squid lo scarica una volta sola e lo serve a tutti dalla sua memoria interna (Cache), risparmiando banda.

### Le ACL (Access Control Lists)
Le ACL sono i "buttafuori" del server. Decidono chi può passare in base a:
* **Sorgente:** Solo il reparto Marketing può navigare.
* **Destinazione:** Nessuno può andare su siti di social media durante le ore di ufficio.
* **Orario:** Il blocco è attivo solo dalle 09:00 alle 18:00.



---

## 4. Scenari Reali: Troubleshooting dei Servizi di Rete

### Scenario U: "La VPN si connette, ma non navigo su Internet"
**Il problema:** L'utente vede l'icona "Connesso", ma non riesce ad aprire nessun sito.
* **Cosa farei io?** Verificherei il file `/etc/sysctl.conf` sul server per assicurarmi che `ip_forward` sia a `1`. Poi controllerei le regole di NAT nel firewall (iptables/firewalld).
* **Ragionamento logico:** Se il tunnel c'è, il problema è l'instradamento (*routing*). Il server riceve i pacchetti ma non sa che deve girarli verso il gateway internet.

### Scenario V: "In ufficio alcuni PC prendono l'IP sbagliato"
**Il problema:** I PC ricevono indirizzi IP come `192.168.100.x` invece dei soliti `10.0.0.x`.
* **Cosa farei io?** Cercherei un "Rogue DHCP Server" (un server DHCP abusivo). Spesso succede quando un dipendente attacca un router Wi-Fi domestico alla presa a muro dell'ufficio.
* **Ragionamento logico:** Il PC accetta la prima risposta (Offer) che riceve. Se il router abusivo è più veloce del server Linux, "vince" lui e manda in tilt la rete.

### Scenario W: "Sito bloccato su Squid, ma l'utente riesce ancora a vederlo"
**Il problema:** Hai aggiunto `.facebook.com` alla blocklist, ma l'utente naviga comunque.
* **Cosa farei io?** Controllerei se l'utente sta usando **HTTPS** o una **VPN/Proxy nel browser**. Verificherei anche l'ordine delle regole in `squid.conf`.
* **Ragionamento logico:** In Squid, le regole vengono lette dall'alto verso il basso. Se metti "Allow all" prima di "Deny Block_Sites", il blocco non verrà mai eseguito. Inoltre, Squid deve essere configurato appositamente per ispezionare il traffico HTTPS (SSL Bump).

---

## 5. Toolbox Finale del Network Admin

* `journalctl -u openvpn@server`: Per vedere i log in tempo reale di chi si connette alla VPN.
* `dhcping -s 192.168.1.1`: Per testare se un server DHCP sta rispondendo correttamente.
* `tail -f /var/log/squid/access.log`: Il comando fondamentale per vedere in diretta quali siti stanno visitando gli utenti (e se vengono bloccati).
* `nmtui`: L'ancora di salvezza per configurare IP statici senza impazzire con i file di testo.

---

