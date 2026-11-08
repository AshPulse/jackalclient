# Sicurezza del sito Zero Client

## Già nel file HTML
- CSP (meta) con hash SHA-256 per ogni script/stile inline: niente `unsafe-inline`, `default-src 'none'`, connessioni solo verso `api.github.com`, nessun form, nessun oggetto/iframe.
- `referrer` ristretto (l'anti-clickjacking è negli header di `_headers`, non nella pagina: così la preview funziona), link esterni con `noopener noreferrer`.
- `REPO` accettato solo nel formato `utente/repo`; dati di GitHub e cache locale validati come numeri.

## Da fare tu (il browser non permette di farlo da dentro la pagina)
1. **Header HTTP**: pubblica il file `_headers` (Netlify / Cloudflare Pages) oppure copia i valori nel tuo server. `frame-ancestors`, HSTS e Permissions-Policy funzionano solo come header.
2. **Libreria 3D con SRI**: fissa la versione esatta e calcola l'hash, poi aggiungi `integrity` al tag `<script>`:
   ```
   curl -sL https://cdn.jsdelivr.net/npm/skinview3d@VERSIONE/bundles/skinview3d.bundle.js | openssl dgst -sha384 -binary | openssl base64 -A
   ```
   Metti `integrity="sha384-RISULTATO"` e sostituisci `@3` con la versione esatta (`npm view skinview3d version`).
3. **Dopo ogni modifica** a script o stili inline, gli hash della CSP vanno rigenerati (altrimenti il browser blocca il codice modificato).
4. HTTPS obbligatorio sul dominio; per attacchi DDoS usa il livello gratuito di Cloudflare davanti al sito.
