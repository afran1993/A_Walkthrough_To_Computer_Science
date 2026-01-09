# Linux CLI – Cheat-Sheet

## File & Directory Management
| Description | Command |
|-------------|---------|
| Lista file e directory | `ls` |
| Lista file con dettagli | `ls -l` |
| Lista file, inclusi nascosti | `ls -a` |
| Cambia directory | `cd <path>` |
| Mostra percorso corrente | `pwd` |
| Crea directory | `mkdir <dirname>` |
| Crea directory ricorsivamente | `mkdir -p <path>` |
| Copia file o directory | `cp <source> <destination>` |
| Copia ricorsivamente | `cp -r <source> <destination>` |
| Sposta o rinomina file | `mv <source> <destination>` |
| Rimuovi file | `rm <file>` |
| Rimuovi directory ricorsivamente | `rm -r <directory>` |
| Mostra contenuto file | `cat <file>` |
| Mostra file con scorrimento | `less <file>` |
| Mostra prime righe di un file | `head <file>` |
| Mostra ultime righe di un file | `tail <file>` |
| Segui in tempo reale modifiche file | `tail -f <file>` |
| Trova file per nome | `find <path> -name "<pattern>"` |
| Cerca testo dentro file | `grep "<pattern>" <file>` |

## File Permissions
| Description | Command |
|-------------|---------|
| Cambia permessi di file | `chmod 755 <file>` |
| Cambia proprietario di file | `chown user:group <file>` |
| Mostra permessi dettagliati | `ls -l` |

## System Information
| Description | Command |
|-------------|---------|
| Mostra versione del sistema | `uname -a` |
| Mostra info dettagliate del sistema | `lsb_release -a` |
| Mostra spazio disco | `df -h` |
| Mostra uso spazio directory/file | `du -sh <path>` |
| Mostra memoria | `free -h` |
| Mostra informazioni CPU | `lscpu` |
| Mostra processi correnti | `top` |
| Mostra processi con filtro | `ps aux | grep <process>` |

## Package Management (Debian/Ubuntu)
| Description | Command |
|-------------|---------|
| Aggiorna lista pacchetti | `sudo apt update` |
| Aggiorna pacchetti installati | `sudo apt upgrade` |
| Installa pacchetto | `sudo apt install <package>` |
| Rimuove pacchetto | `sudo apt remove <package>` |
| Pulisce pacchetti inutilizzati | `sudo apt autoremove` |

## Networking
| Description | Command |
|-------------|---------|
| Mostra configurazione IP | `ip a` |
| Mostra routing | `ip route` |
| Mostra connessioni attive | `netstat -tulnp` |
| Mostra connessioni TCP | `ss -tulw` |
| Ping a host | `ping <host>` |
| Test porta TCP | `nc -zv <host> <port>` |
| Mostra DNS risoluzione | `nslookup <host>` |
| Mostra traceroute | `traceroute <host>` |
| Mostra tabelle ARP | `arp -a` |

## Processes & Jobs
| Description | Command |
|-------------|---------|
| Mostra processi in esecuzione | `ps aux` |
| Uccide processo per PID | `kill <PID>` |
| Uccide processo per nome | `pkill <name>` |
| Cambia priorità di un processo | `renice <priority> -p <PID>` |
| Lancia comando in background | `<command> &` |
| Mostra job in background | `jobs` |
| Porta job in foreground | `fg %<job_id>` |

## Disk & File System
| Description | Command |
|-------------|---------|
| Monta filesystem | `mount <device> <mount_point>` |
| Smonta filesystem | `umount <mount_point>` |
| Mostra partizioni | `lsblk` |
| Mostra filesystem | `df -hT` |
| Controlla filesystem | `fsck <device>` |

## Compression & Archiving
| Description | Command |
|-------------|---------|
| Comprime in tar.gz | `tar -czvf <archive>.tar.gz <files>` |
| Estrae tar.gz | `tar -xzvf <archive>.tar.gz` |
| Comprime in zip | `zip <archive>.zip <files>` |
| Estrae zip | `unzip <archive>.zip` |

## Users & Groups
| Description | Command |
|-------------|---------|
| Mostra utenti | `cat /etc/passwd` |
| Mostra gruppi | `cat /etc/group` |
| Aggiungi utente | `sudo adduser <username>` |
| Rimuovi utente | `sudo deluser <username>` |
| Aggiungi utente a gruppo | `sudo usermod -aG <group> <user>` |

## Misc / Useful
| Description | Command |
|-------------|---------|
| Visualizza output paginato | `less <file>` |
| Cerca testo in output | `<command> | grep <pattern>` |
| Conta linee/parole/caratteri | `wc -lwc <file>` |
| Mostra variabili ambiente | `printenv` |
| Imposta variabile ambiente | `export VAR=value` |
| Visualizza ultimi comandi eseguiti | `history` |
| Esegui comando come root | `sudo <command>` |
