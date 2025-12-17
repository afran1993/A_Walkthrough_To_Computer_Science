# Git – Comandi principali (CLI)

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

## Branching
| Description | Command |
|-------------|---------|
| Mostra branch attivo | `git branch` |
| Crea un nuovo branch | `git branch <nome_branch>` |
| Passa a un branch | `git checkout <branch>` |
| Crea e passa a un nuovo branch | `git checkout -b <branch>` |
| Unisce branch nel branch corrente | `git merge <branch>` |
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

## Remote
| Description | Command |
|-------------|---------|
| Scarica e integra modifiche remote | `git pull` |
| Invia modifiche locali al repository remoto | `git push` |
| Pubblica tag remoto | `git push origin <nome_tag>` |

## Undo & History
| Description | Command |
|-------------|---------|
| Annulla modifiche locali non salvate | `git checkout -- <file>` |
| Ripristina commit locale (hard reset) | `git reset --hard <commit>` |
| Mostra dettagli di un commit | `git show <commit>` |
| Stash: salva modifiche temporaneamente | `git stash` |
| Riprendi modifiche dallo stash | `git stash pop` |
| Mostra stash salvati | `git stash list` |
| Mostra differenze tra commit | `git diff <commit1> <commit2>` |

## Tagging
| Description | Command |
|-------------|---------|
| Crea un tag locale | `git tag <nome_tag>` |
| Invia tag remoto | `git push origin <nome_tag>` |
