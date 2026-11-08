# Zero Client: guida per pubblicare su GitHub

Il sito è già configurato:
- il pulsante Download scarica `downloads/zero-client-1.0.0.jar`
- indirizzo del sito: https://ashpulse.github.io/jackalclient/
- link GitHub nel menu: https://github.com/AshPulse/jackalclient

## 1. Metti i file nel tuo repository locale
Dal terminale (cambia il percorso dello zip se serve):

    cd ~/Downloads
    unzip -o zero-client-final.zip -d zc_new
    cp -r zc_new/zero-client/. ~/Downloads/zero-client-site/zero-client/

Se `unzip` non c'è: estrai lo zip con il file manager e copia a mano tutto il contenuto
della cartella `zero-client` dentro `~/Downloads/zero-client-site/zero-client/`
(sovrascrivi i file esistenti, non toccare la cartella nascosta `.git`).

## 2. Commit e push

    cd ~/Downloads/zero-client-site/zero-client
    git add .
    git commit -m "Zero Client: sito finale e download"
    git push

Se risponde "rejected" o "fetch first":

    git pull --rebase origin main
    git push

## 3. Attiva il sito (una volta sola)
Su github.com apri il repository, poi Settings, Pages, "Deploy from a branch",
branch `main` e cartella `/ (root)`, Save. Dopo circa un minuto il sito è online.
Se non vedi le novità: Ctrl+F5.

## Aggiornare in futuro
Cambi i file, poi:

    git add . && git commit -m "Aggiornamento" && git push

Per una nuova versione del client: copia il nuovo .jar in `downloads/`, aggiorna il nome nel
tag `<meta name="jc-download" ...>` in `index.html` (solo quella riga) e fai commit e push.
Non modificare gli script dentro `index.html`: la CSP li blocca se cambiano.
