Link utili: 

Seguire la documentazione a questo [link](https://help.sonatype.com/en/install-nexus-repository.html) o anche meglio a quest'altro [link](https://devopscube.com/how-to-install-latest-sonatype-nexus-3-on-linux/)

Questa guida illustra come installare Nexus Repository Manager su un server Linux, configurare la porta di ascolto, impostare i permessi corretti e avviare il servizio con systemd.

## 1. Creazione dell’utente Nexus

Non è consigliabile eseguire Nexus come `root`. Creiamo un utente dedicato:

```bash
sudo useradd -r -m -d /app/sonatype-work nexus
sudo passwd nexus
```

* -r: utente di sistema.
* -m: crea la home directory.
/app/sonatype-work: directory di lavoro di Nexus.

## 2. Scaricare e installare Nexus
Scarica l’ultima versione dal sito ufficiale:

Scarichiamo l’ultima versione di Nexus Repository Manager e la estraiamo nella cartella /app/nexus:

```bash
cd /tmp
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz -O nexus-latest.tar.gz
sudo mkdir -p /app
sudo tar -xvzf nexus-latest.tar.gz -C /app
sudo mv /app/nexus-* /app/nexus
```

* wget: scarica l’archivio ufficiale.
* tar -xvzf: estrae i file.
* mv: rinomina la cartella con un nome semplice /app/nexus.

## 3. Impostare i permessi corretti

Assicuriamoci che l’utente nexus sia proprietario dei file di Nexus e della directory di lavoro:

```bash
sudo chown -R nexus:nexus /app/nexus
sudo chown -R nexus:nexus /app/sonatype-work
```

* -R: applica ricorsivamente a tutte le sottodirectory.

## 4. Configurare la porta di ascolto

Modifichiamo le proprietà di Nexus per impostare la porta 8081 e ascoltare su tutte le interfacce:

```bash
sudo vi /app/sonatype-work/nexus3/etc/nexus.properties
```

Aggiungere o modificare le seguenti righe:

```bash
application-port=8081
application-host=0.0.0.0
# nexus-context-path=/
```

* application-port: porta HTTP del server.
* application-host: 0.0.0.0 indica che ascolta su tutte le interfacce.

## 5. Configurare SELinux (se presente)

Se SELinux è abilitato, dobbiamo consentire l’ascolto sulla porta 8081:

```bash
sudo semanage port -a -t http_port_t -p tcp 8081 || sudo semanage port -m -t http_port_t -p tcp 8081
sudo getenforce
```

* Il primo comando aggiunge o modifica la porta associata al tipo http_port_t.
* getenforce verifica lo stato di SELinux (Permissive o Enforcing).


## 6. Creare il servizio systemd

Creiamo un servizio systemd per gestire Nexus come servizio di sistema:

```bash
sudo vi /etc/systemd/system/nexus.service
```

Inseriamo:

```ini
[Unit]
Description=Nexus Repository Manager
After=network.target

[Service]
Type=forking
ExecStart=/app/nexus/bin/nexus start
ExecStop=/app/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

Poi ricarichiamo systemd e abilitiamo il servizio:

```bash
sudo systemctl daemon-reload
sudo systemctl enable nexus.service
sudo systemctl start nexus.service
```

## 7. Verificare che Nexus sia in ascolto

Controlliamo se la porta 8081 è aperta:

```bash
sudo ss -tulpen | grep 8081

```
Dovresti vedere qualcosa come:

```bash
tcp   LISTEN 0      128    0.0.0.0:8081    0.0.0.0:* ...

```

Se non compare, verifica nexus.properties, SELinux e i permessi dell’utente nexus.

## 8. Controllare i log di Nexus

I log di Nexus contengono informazioni utili per il debug:

```bash
sudo tail -f /app/sonatype-work/nexus3/log/nexus.log
```

Qui puoi monitorare eventuali errori all’avvio o problemi runtime.

## 9. Avvio e arresto manuale

È possibile avviare o fermare Nexus senza systemd:

```bash
sudo -u nexus /app/nexus/bin/nexus start
sudo -u nexus /app/nexus/bin/nexus stop
```

* -u nexus: esegue i comandi come utente Nexus.

## 10. Controllare i processi Java attivi

Verifichiamo che Nexus stia girando:

```bash
ps -ef | grep nexus | grep java
```

Dovresti vedere un processo simile:
```bash
nexus  2823  2729  0 06:00 pts/0 00:01:44 /app/nexus/bin/java ... com.sonatype.insight.brain.service.InsightBrainService server /etc/nexus-iq-server/config.yml
```

La presenza di questo processo indica che il server è avviato correttamente.

## 11. Troubleshooting

* Se Nexus non si avvia, controlla /app/sonatype-work/nexus3/log/nexus.log.
* Se la porta 8081 non è aperta, verifica SELinux e nexus.properties.
* Controlla lo stato del servizio:

```bash
sudo systemctl status nexus.service
sudo journalctl -xeu nexus.service | tail -50
```

Riavvia il servizio se necessario:

```bash
sudo systemctl restart nexus.service

```

Assicurati che l’utente nexus abbia accesso completo a /app/nexus e /app/sonatype-work.

## 12. Note finali

* L’installazione è senza Docker, quindi tutti i file sono direttamente sul filesystem.
* Assicurati di avere Java 11 installato.
* La configurazione predefinita espone Nexus su tutte le interfacce (0.0.0.0), ma puoi limitarle modificando application-host.

