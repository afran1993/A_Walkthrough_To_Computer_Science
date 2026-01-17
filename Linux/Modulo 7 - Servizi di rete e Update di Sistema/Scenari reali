# Networking Linux Avanzato - Domande e Risposte

## 1. Client-Server

**Domanda:** Chi avvia la comunicazione nel modello client-server?
**Risposta:** Il client. Il client invia richieste al server e riceve risposte (ad esempio HTTP 200 = OK, 404 = Not Found).

**Domanda:** Esempio pratico di flusso dati browser-server?
**Risposta:** Il browser richiede una pagina web; il server risponde inviando il codice HTML della pagina.

---

## 2. Configurazione VM e Rete

**Domanda:** Differenza tra NAT e Bridged in VirtualBox?
**Risposta:**

* NAT: la VM condivide l'IP dell'host, non raggiungibile dalla LAN.
* Bridged: la VM riceve un IP nella stessa subnet della rete fisica, diventa nodo indipendente.

**Domanda:** Come verifichi IP e connettività?
**Risposta:** `ip addr show`, `ping`, `curl`.

**Domanda:** Passaggi se una VM Bridged non riesce a fare ping a Google?
**Risposta:** Verificare la configurazione della scheda di rete della VM, gateway, firewall e cavi/connessione fisica.

---

## 3. IP Statico e DNS

**Domanda:** Perché usare IP statico per server critico?
**Risposta:** Per essere sempre raggiungibile allo stesso indirizzo.

**Domanda:** Come impostare DNS locale?
**Risposta:** Modificare `/etc/hosts` per mappare nomi a indirizzi IP locali.

**Domanda:** Differenza `/etc/hosts` vs `/etc/resolv.conf`?
**Risposta:**

* `/etc/hosts`: mappatura statica nomi locali.
* `/etc/resolv.conf`: server DNS da interrogare.

---

## 4. Strumenti diagnostici

**Domanda:** Differenza tra ping e curl?
**Risposta:**

* ping: verifica Layer 3 (IP) e raggiungibilità host.
* curl: verifica Layer 7 (HTTP/HTTPS) e servizio web.

**Domanda:** Comando per verificare porte TCP in ascolto?
**Risposta:** `ss -tln`

**Domanda:** Comando per vedere stato NIC e link fisico?
**Risposta:** `ethtool <interfaccia>`

---

## 5. Trasferimento file

**Domanda:** Differenza SCP vs rsync?
**Risposta:**

* SCP: copia sicura via SSH, trasferisce tutto il file.
* Rsync: trasferisce solo le parti modificate, più efficiente.

**Domanda:** Porta di default FTP?
**Risposta:** 21 (canale di controllo).

---

## 6. Bonding / Teaming

**Domanda:** Cosa fa bonding active-backup?
**Risposta:** Se la NIC primaria cade, il traffico passa sulla NIC secondaria. Verifica con `cat /proc/net/bonding/bond0`.

**Domanda:** Modalità comuni bonding?
**Risposta:**

* 0: balance-rr
* 1: active-backup
* 5: adaptive transmit load balancing

---

## 7. VLAN

**Domanda:** Come creare VLAN su Linux?
**Risposta:**

```bash
ip link add link enp0s3 name enp0s3.10 type vlan id 10
ip addr add 192.168.10.2/24 dev enp0s3.10
ip link set enp0s3.10 up
```

* Lato switch: configurare trunk/access per le VLAN.

---

## 8. Routing avanzato

**Domanda:** Come configurare failover con due gateway?
**Risposta:** Usare metriche diverse:

```bash
ip route add default via 192.168.1.1 dev enp0s3 metric 100
ip route add default via 192.168.2.1 dev enp0s3 metric 200
```

Per failover avanzato si possono usare strumenti tipo keepalived.

---

## 9. Firewall (iptables)

**Domanda:** Come permettere SSH solo da 192.168.1.0/24?
**Risposta:**

```bash
iptables -A INPUT -p tcp -s 192.168.1.0/24 --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j DROP
```

**Domanda:** Come capire se firewall blocca servizi?
**Risposta:** Controllare `iptables -L -n` o `nft list ruleset` e policy di default.

---

## 10. Troubleshooting server web

**Domanda:** Sequenza di controllo server web non raggiungibile?
**Risposta:**

1. `ping` host per connettività IP
2. `ss -tln | grep :80` per porte in ascolto
3. `curl` per test HTTP
4. Controllo firewall (`iptables -L -n`, `systemctl status firewalld`)
5. Controllo servizio web (`systemctl status httpd`)
6. Controllo cablaggio (`ethtool enp0s3`)
7. Controllo log recenti (`journalctl -u httpd -n 50`)

---

## 11. Interpretazione `ip addr show`

**Domanda:** Significato loopback vs interfaccia fisica?
**Risposta:**

* `lo` → solo traffico interno (127.0.0.1, 10.255.255.254)
* `eth0` → interfaccia reale verso rete esterna (es. 172.28.12.17/20)
* IPv6 link-local (`fe80::`) → comunicazione solo sul link locale
