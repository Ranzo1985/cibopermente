# Lancio — Checklist passo-passo

Documento operativo per portare il sito online. Tutte le azioni che dovevo fare io sono **già fatte**; qui restano solo i tuoi passi.

---

## ✅ Fatto da Claude

- [x] Codice del sito (HTML/CSS/JS, palette calda, brand "Cibo per la Mente").
- [x] Liste parole 4-7 lettere generate da fonte open source (`parole.js`).
- [x] Favicon SVG inline (tile terracotta con M).
- [x] Meta Open Graph + Twitter Card configurati.
- [x] Immagine anteprima `og-image.png` (1200×630) generata da Edge headless.
- [x] Modale Privacy completa, GDPR-friendly, con email contatto.
- [x] Snippet Umami Cloud attivo col tuo `data-website-id`.
- [x] `git init` + primo commit `85b4551` sulla cartella `cibo-per-la-mente/`.
- [x] `.gitignore` standard.
- [x] README di progetto + README del gioco + spec funzionale.

Hai 8 file pronti, tutti committati su branch `main`. Verifica:
```powershell
cd "c:\Users\DAVIDE\Desktop\Claude_Workspace\projects\cibo-per-la-mente"
git log --oneline
git status
```

---

## 🔧 Cosa devi fare tu

### 1. Crea il repo su GitHub (2 min)

1. Vai su [github.com/new](https://github.com/new).
2. **Repository name:** `cibopermente`
3. **Public** (obbligatorio per GitHub Pages gratis).
4. **NON** spuntare "Add a README" / "Add .gitignore" / "Choose a license" — abbiamo già tutto in locale.
5. Click **Create repository**.

### 2. Push del codice (1 min)

Apri PowerShell e incolla (è già configurato col tuo username Ranzo1985):

```powershell
cd "c:\Users\DAVIDE\Desktop\Claude_Workspace\projects\cibo-per-la-mente"
git remote add origin https://github.com/Ranzo1985/cibopermente.git
git push -u origin main
```

Ti chiederà autenticazione: usa il **token personale** GitHub (non la password — GitHub non accetta più password da CLI). Se non ne hai uno:
- Vai su [github.com/settings/tokens/new](https://github.com/settings/tokens/new)
- Note: `cli push`
- Expiration: 90 giorni (o no expiration)
- Scope: spunta solo **`repo`**
- Generate → copia il token (lungo, inizia con `ghp_`)
- Quando PowerShell chiede password, incolla il token

### 3. Attiva GitHub Pages (1 min)

1. Sul repo appena creato → **Settings** (in alto a destra)
2. **Pages** (menu sinistro)
3. **Source: Deploy from a branch**
4. **Branch: `main`** / **Folder: `/ (root)`**
5. **Save**

Aspetta 1-2 minuti. In cima alla pagina apparirà un riquadro verde con l'URL del sito.

### 4. Test del sito (5 min)

URL del gioco: **https://ranzo1985.github.io/cibopermente/parola/**

Controlla:
- [ ] Si apre la home con logo "Cibo per la Mente" e i 4 pulsanti difficoltà.
- [ ] Click su "Normale" → si apre la griglia 5×6, prova una parola (es. `CASA` se 4 lettere o `MONDO` se 5).
- [ ] Click su "📊" → mostra statistiche.
- [ ] Click su "Privacy" in basso → si apre la modale con l'informativa.
- [ ] Apri il sito da telefono per verificare il mobile (chiave: tastiera virtuale e leggibilità).

### 5. Verifica anteprima WhatsApp (2 min)

Vai su [https://www.opengraph.xyz/](https://www.opengraph.xyz/) e incolla l'URL.
Devi vedere il cartoncino con logo "Cibo per la Mente" + scritta + tile colorati.

Test reale: mandati il link da solo su WhatsApp (chat con te stesso o "Saved Messages") per controllare l'anteprima dal vivo.

### 6. Verifica Umami (5 min)

1. Apri il sito 2-3 volte da dispositivi diversi (PC + telefono).
2. Gioca una partita.
3. Vai su [cloud.umami.is](https://cloud.umami.is) → dashboard → vedrai:
   - Visite e visitatori unici.
   - Eventi `game-start`, `game-won`, `game-lost`.

Se non vedi nulla dopo 5-10 minuti, controlla che lo `<script>` Umami in `index.html` non sia bloccato (es. da un AdBlock molto aggressivo del browser).

### 7. Mandalo in giro

Tre o quattro contatti per partire — gente che apprezza i giochini di parole. Chiedi feedback su:
- Difficoltà delle parole (troppo facili? troppo strane?)
- Velocità di caricamento sul loro telefono.
- Se l'anteprima WhatsApp è chiara.

---

## 📊 Strategia analytics e ads (per il futuro)

### Cosa monitorare ogni settimana
- **Visitatori unici** (metrica chiave)
- **Visite ripetute** (= "voglia di tornarci")
- **Distribuzione partite per difficoltà** (capire cosa preferisce la gente)
- **Win rate** (se troppo basso → parole troppo difficili)

### Soglie indicative per valutare ads
| Visitatori unici / mese | Cosa fare |
|-------------------------|-----------|
| < 100 | Nessuna pubblicità. Concentrati sul prodotto e sul passaparola. |
| 100-1.000 | Aggiungi nuovi giochi sotto lo stesso contenitore (anagrammi, quiz, ecc.). Niente ads. |
| 1.000-10.000 | Inizia a valutare ads "etiche" (es. EthicalAds, Carbon Ads) o donazioni (Buy Me a Coffee). |
| > 10.000 | Google AdSense / Ezoic / Mediavine diventano sensati. **Attenzione:** in quel momento la privacy policy va riscritta e serve banner cookie GDPR. |

### Quando aggiungerai ads, ricorda
- L'attuale informativa Privacy dice esplicitamente "no pubblicità". Va riscritta.
- Gli ad-network usano cookie di tracciamento → serve banner cookie a norma (es. tramite [Cookiebot](https://www.cookiebot.com/) o [Iubenda](https://www.iubenda.com/)).
- Conviene tenere le ads **fuori dalla zona di gioco** (sopra l'header o sotto la tastiera), non nel mezzo.
- Test A/B: se il bounce rate sale tanto dopo le ads, valuta di toglierle e cercare altre fonti (donazioni, sponsor diretto di un piccolo brand).

### Roadmap nuovi giochi (idee)
La struttura `cibo-per-la-mente/<gioco>/` è già pensata per ospitarne altri:
- **Anagrammi**: data una sequenza di lettere, trova la parola.
- **Indovinello del giorno**: testo breve, risposta a digitazione libera con tolleranza accenti.
- **Memory parole**: coppie di sinonimi/contrari da abbinare.
- **Frase del giorno**: un proverbio/citazione coperta, scopri lettere come a "La ruota della fortuna".

Ogni gioco aggiunto = altro motivo per tornare = visite ripetute = più dati.

---

## 🆘 Problemi comuni

| Sintomo | Causa probabile | Fix |
|---------|-----------------|-----|
| `git push` chiede password e rifiuta | Devi usare un Personal Access Token, non la password GitHub | Vedi step 2 sopra |
| Pagina 404 dopo aver attivato Pages | DNS / propagazione, oppure path sbagliato | Aspetta 2 min, ricarica. Verifica URL: deve essere `/cibopermente/parola/` con slash finale |
| Anteprima WhatsApp non appare | Cache di WhatsApp/Facebook | Su [opengraph.xyz](https://www.opengraph.xyz/) verifica che i meta siano letti correttamente. WhatsApp può cachare per ~24h: se hai aggiornato l'immagine, potresti dover forzare il debugger di Facebook |
| Umami non riceve eventi | Browser con AdBlock aggressivo blocca lo script | Prova in finestra privata o da altro dispositivo. Se il problema persiste su tutti, controlla che il `data-website-id` sia corretto |
