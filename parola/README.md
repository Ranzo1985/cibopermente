# Cibo per la Mente — Parola

Mini-gioco web stile Wordle in italiano. Una parola di 5 lettere al giorno, 6 tentativi.

## Provarlo in locale

Apri `index.html` direttamente con doppio click nel browser. Funziona — non serve un server.

Se vuoi un server locale (utile su mobile in LAN):

```powershell
cd "c:/Users/DAVIDE/Desktop/Claude_Workspace/projects/cibo-per-la-mente/parola"
python -m http.server 8000
# poi apri http://localhost:8000/
```

## Deploy su GitHub Pages

1. Crea un nuovo repo pubblico su GitHub (es. `cibopermente`).
2. Da locale:
   ```powershell
   cd "c:/Users/DAVIDE/Desktop/Claude_Workspace/projects/cibo-per-la-mente"
   git init
   git add .
   git commit -m "First release: Parola"
   git branch -M main
   git remote add origin https://github.com/Ranzo1985/cibopermente.git
   git push -u origin main
   ```
3. Su GitHub: **Settings → Pages → Source: Deploy from branch → main / root → Save**.
4. Dopo ~1 minuto il sito è live a **https://ranzo1985.github.io/cibopermente/parola/**.
5. Manda quel link via WhatsApp.

## Struttura file

| File | Descrizione |
|------|-------------|
| `index.html` | Gioco completo: HTML + CSS + JS, 4 livelli, brand, modale privacy, OG meta |
| `parole.js` | Liste parole 4/5/6/7 lettere (soluzioni + accettate) |
| `og-image.svg` | Sorgente anteprima WhatsApp (da convertire in PNG, vedi sopra) |
| `SPECIFICA_FUNZIONALE.md` | Documento di progetto |

## Anteprima link su WhatsApp

`og-image.png` (1200×630) è già nel repo, generato da `og-image.svg`. WhatsApp/Telegram/Facebook lo useranno per mostrare il cartoncino con titolo, descrizione e immagine.

**Test anteprima** dopo il deploy: incolla l'URL su [https://www.opengraph.xyz/](https://www.opengraph.xyz/) per vedere come apparirà.

**Per rigenerare il PNG** se modifichi l'SVG (richiede Edge installato):
```powershell
& "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --headless --disable-gpu --screenshot=og-image.png --window-size=1200,630 --hide-scrollbars (Resolve-Path og-image.svg).Path
```

## Statistiche di utilizzo (Umami Cloud)

Il sito è predisposto per Umami Cloud — analytics gratis, no cookie, GDPR-ok.

**Setup (5 minuti):**
1. Vai su [https://cloud.umami.is](https://cloud.umami.is) e crea un account.
2. Dashboard → **Add website** → nome: `Cibo per la Mente`, dominio: il tuo URL Pages (es. `tuoUsername.github.io`).
3. Clicca sul sito appena creato → **Tracking code**: copia il `data-website-id` (un UUID).
4. Apri `index.html`, cerca `INSERISCI_QUI_IL_TUO_WEBSITE_ID`, sostituiscilo col tuo UUID.
5. Togli i `<!--` e `-->` attorno alla riga `<script defer src="https://cloud.umami.is/script.js"...`.
6. Commit e push.

**Cosa vedrai sul dashboard:**
- Visite totali e visitatori unici (anche per giorno/settimana/mese).
- Paese, browser, dispositivo (desktop/mobile).
- Eventi custom: `game-start`, `game-won`, `game-lost` (per difficoltà).

Il tracking è anonimo e senza cookie — non serve banner GDPR.

## Aggiornare la lista parole

Le parole vengono dalla repo open source [napolux/paroleitaliane](https://github.com/napolux/paroleitaliane). Per rigenerare `parole.js` partendo dai file originali, vedi sezione 5 della specifica.
