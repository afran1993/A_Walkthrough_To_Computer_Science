# Git – Complete Cheat-Sheet

## Setup
| Description | Command |
|-------------|---------|
| Mostra versione di Git | `git --version` |
| Configura username | `git config --global user.name "Nome"` |
| Configura email | `git config --global user.email "email@example.com"` |
| Mostra configurazioni | `git config --list` |

## Repository
| Description | Command |
|-------------|---------|
| Inizializza un nuovo repository Git | `git init` |
| Clona un repository remoto | `git clone <url>` |
| Collega un repository remoto | `git remote add origin <url>` |
| Mostra repository remoti | `git remote -v` |

## Branching & Merging
| Description | Command |
|-------------|---------|
| Mostra branch attivo | `git branch` |
| Mostra tutti i branch remoti | `git branch -r` |
| Mostra tutti i branch locali + remoti | `git branch -a` |
| Crea un nuovo branch | `git branch <nome_branch>` |
| Passa a un branch | `git checkout <branch>` |
| Crea e passa a un nuovo branch | `git checkout -b <branch>` |
| Unisce branch nel branch corrente | `git merge <branch>` |
| Merge con commit anche se fast-forward possibile | `git merge --no-ff <branch>` |
| Squash merge (tutti i commit in uno) | `git merge --squash <branch>` |
| Elimina branch locale | `git branch -d <branch>` |
| Elimina branch remoto | `git push origin --delete <branch>` |
| Rebase interattivo | `git rebase -i <branch>` |

## Staging & Commit
| Description | Command |
|-------------|---------|
| Controlla lo stato dei file | `git status` |
| Aggiunge file all’area di staging | `git add <file>` |
| Aggiunge tutti i file all’area di staging | `git add .` |
| Crea un commit con messaggio | `git commit -m "messaggio"` |
| Mostra log dei commit | `git log` |
| Mostra log con grafico dei branch | `git log --oneline --graph --all` |
| Mostra differenze tra file modificati | `git diff` |
| Mostra differenze tra staging e ultimo commit | `git diff --staged` |
| Rimuove file dall’area di staging | `git reset <file>` |
| Rimuove file dal repository e disco | `git rm <file>` |
| Sposta file/renaming | `git mv <old> <new>` |

## Remote & Collaboration
| Description | Command |
|-------------|---------|
| Scarica e integra modifiche remote | `git pull` |
| Invia modifiche locali al repository remoto | `git push` |
| Imposta upstream per un branch | `git push --set-upstream origin <branch>` |
| Forza push (usa con attenzione) | `git push --force` |
| Fetch dal remote | `git fetch <remote>` |
| Fetch da tutti i remoti | `git fetch --all` |
| Pull con rebase locale | `git pull --rebase` |

## Undo & History
| Description | Command |
|-------------|---------|
| Annulla modifiche locali non salvate | `git checkout -- <file>` |
| Ripristina commit locale (hard reset) | `git reset --hard <commit>` |
| Ripristina commit locale ma mantiene staging | `git reset --soft <commit>` |
| Ripristina commit locale ma mantiene modifiche nel working dir | `git reset --mixed <commit>` |
| Crea un commit inverso senza cancellare la storia | `git revert <commit>` |
| Rimuove file non tracciati | `git clean -f` |
| Rimuove file e directory non tracciati | `git clean -fd` |
| Mostra dettagli di un commit | `git show <commit>` |
| Mostra la storia dei movimenti di HEAD | `git reflog` |
| Mostra differenze tra commit | `git diff <commit1> <commit2>` |

## Stash & Work in Progress
| Description | Command |
|-------------|---------|
| Salva modifiche temporaneamente | `git stash` |
| Salva modifiche con messaggio | `git stash save "messaggio"` |
| Applica modifiche dallo stash e rimuove stash | `git stash pop` |
| Mostra stash salvati | `git stash list` |
| Crea branch da stash | `git stash branch <branch>` |
| Rimuove uno stash specifico | `git stash drop <stash@{n}>` |

## Tags & Releases
| Description | Command |
|-------------|---------|
| Crea tag locale | `git tag <nome_tag>` |
| Crea tag annotato | `git tag -a <nome_tag> -m "messaggio"` |
| Lista tag | `git tag -l` |
| Invia tag remoto | `git push origin <nome_tag>` |
| Invia tutti i tag | `git push origin --tags` |

## Submodules
| Description | Command |
|-------------|---------|
| Aggiungi un submodule | `git submodule add <repo> <path>` |
| Inizializza e aggiorna submodules | `git submodule update --init --recursive` |
| Aggiorna submodules all’ultimo commit remoto | `git submodule update --remote` |

## Inspection & Debug
| Description | Command |
|-------------|---------|
| Mostra chi ha modificato ogni riga di un file | `git blame <file>` |
| Mostra log con statistiche dei file modificati | `git log --stat` |
| Mostra log con patch/diff per ogni commit | `git log -p` |
