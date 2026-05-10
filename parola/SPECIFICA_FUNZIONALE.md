# Cibo per la Mente — Parola (Specifica Funzionale)

**Contenitore:** Cibo per la Mente (sito raccoglitore di mini-giochi/quiz, espandibile in futuro)
**Gioco:** Parola — Wordle in italiano (primo gioco del contenitore)
**Owner:** Davide Miori
**Data creazione:** 2026-05-10
**Stato:** In definizione

> **Nota architetturale:** "Cibo per la Mente" è il sito contenitore. Il primo gioco rilasciato è "Parola". L'architettura prevede di poter aggiungere altri giochi sotto la stessa cappello (es. quiz, anagrammi, ecc.) in futuro. URL ipotizzato: `cibopermente.github.io/parola/`.

---

## 1. Obiettivo

Creare un giochino web da condividere via WhatsApp: link cliccabile → si apre una pagina dove l'utente prova a indovinare una parola italiana di 5 lettere in 6 tentativi, con feedback colorato sulle lettere (stile Wordle).

Caso d'uso: divertimento personale, da mandare ad amici/familiari. Niente login, niente account, niente backend.

---

## 2. Regole di gioco

- Quattro livelli di difficoltà a scelta a inizio sessione:
  - **Facile**: parola di **4 lettere**.
  - **Normale**: parola di **5 lettere**.
  - **Difficile**: parola di **6 lettere**.
  - **Estremo**: parola di **7 lettere**.
- Il giocatore ha sempre **6 tentativi** indipendentemente dalla difficoltà.
- Stato e statistiche sono salvati separatamente per ogni livello (puoi giocare tutti e quattro nello stesso giorno).
- A ogni tentativo deve scrivere una parola di esattamente 5 lettere.
- Dopo l'invio, ogni lettera viene colorata:
  - 🟩 **Verde**: lettera giusta nella posizione giusta.
  - 🟨 **Giallo**: lettera presente nella parola ma in posizione sbagliata.
  - ⬜ **Grigio**: lettera non presente nella parola.
- Vittoria: tutte le 5 lettere verdi entro il 6° tentativo.
- Sconfitta: 6 tentativi esauriti senza indovinare → si rivela la parola.

### Regola colori — caso lettere ripetute
Se la parola da indovinare è `PALLA` e l'utente scrive `LELLO`:
- La prima `L` è in posizione giusta → verde.
- La seconda `L` è in posizione giusta → verde.
- Le altre lettere seguono la regola standard.

Se la parola è `CASA` e l'utente scrive `SASSO`:
- Solo una `S` viene colorata (quella in posizione giusta), le altre `S` extra restano grigie. Cioè: ogni lettera della parola target "consuma" al massimo un colore.

---

## 3. Scelte di design (decise con utente il 2026-05-10)

| Decisione | Scelta |
|-----------|--------|
| Tentativi | 6 |
| Selezione parola | Una al giorno uguale per tutti (stile Wordle ufficiale) |
| Hosting | GitHub Pages |
| Stack | HTML + CSS + JavaScript puro, single-file o pochi file statici |
| Backend | Nessuno |
| Account utente | No |

---

## 4. Esperienza utente

### Flusso
1. Utente clicca link WhatsApp → si apre la pagina nel browser mobile/desktop.
2. Vede griglia 6×5 vuota + tastiera virtuale sotto.
3. Digita una parola (tastiera fisica su desktop, tastiera virtuale su mobile).
4. Preme INVIO → si colorano le caselle, si colorano anche i tasti della tastiera virtuale.
5. Continua finché non vince o esaurisce i tentativi.
6. A fine partita: messaggio (vinto/perso), parola rivelata se persa, pulsante "Condividi risultato" che copia negli appunti un riepilogo emoji da incollare su WhatsApp.

### Esempio condivisione
```
Parola #42 — 4/6
⬜⬜🟨⬜⬜
🟨⬜🟩⬜⬜
🟩🟩🟩⬜🟨
🟩🟩🟩🟩🟩
```

### Vincoli UX
- **Mobile-first**: il link parte da WhatsApp, la maggioranza apre da telefono.
- Tastiera virtuale obbligatoria su mobile (la tastiera di sistema non basta perché serve il feedback colorato sui tasti).
- Una sola partita al giorno per parola (il giocatore può ricaricare ma vede lo stato salvato).

---

## 5. Selezione parola del giorno

- Lista di parole valide italiane di 5 lettere (lista "soluzioni") — curata, parole comuni e riconoscibili.
- Lista di parole accettate come tentativi (più ampia, include la lista soluzioni + altre parole italiane di 5 lettere) — per non rifiutare tentativi legittimi.
- La parola del giorno = `lista_soluzioni[ giorni_dal_2026-05-10 % len(lista_soluzioni) ]`.
- Tutti i giocatori nel mondo vedono la stessa parola lo stesso giorno (in base al fuso orario locale del browser — semplificazione accettata).

### Stato lista (aggiornato 2026-05-10)
- **Fonte:** repo open source `napolux/paroleitaliane` su GitHub.
- Liste per ciascuna lunghezza:

| Lunghezza | Soluzioni (parole comuni) | Accettate (dizionario) |
|-----------|----------------------------|--------------------------|
| 4 (facile)    | 86  | 2.820 |
| 5 (normale)   | 173 | 8.176 |
| 6 (difficile) | 216 | 18.617 |
| 7 (estremo)   | 199 | 40.258 |

- **File generato:** `parole.js` (~853 KB), espone `LISTE = { 4: {...}, 5: {...}, 6: {...}, 7: {...} }`.

---

## 6. Persistenza locale

Salviamo in `localStorage` del browser:
- Data ultima partita giocata.
- Numero parola affrontata oggi (per non far rigiocare se ricarica).
- Tentativi inseriti finora oggi.
- Statistiche cumulative: partite giocate, vittorie, distribuzione tentativi, streak attuale, streak max.

---

## 6-bis. Analytics

- **Strumento:** Umami Cloud (free tier 10k eventi/mese, no cookie, no banner GDPR).
- **Cosa misuriamo:**
  - Pageview standard (visite + visitatori unici, paesi, dispositivi).
  - Eventi custom: `game-start`, `game-won`, `game-lost` con campo `difficulty` (4/5/6/7) e `tries` (per le vittorie).
- **Snippet:** già predisposto in `index.html`, da attivare sostituendo `INSERISCI_QUI_IL_TUO_WEBSITE_ID` (vedi README).
- **Dashboard:** `https://cloud.umami.is/` (account utente).

## 6-ter. Stile grafico — palette calda "estate mediterranea"

Direzione: caldo, accogliente, invita a tornare. Niente dark mode — ispirazione gelateria/casa di vacanza.

**Palette (CSS variables in `:root`):**
| Token | Hex | Uso |
|-------|-----|-----|
| `--bg` | `#FBF3E4` | sfondo crema |
| `--card` | `#FFFAF0` | celle, modali, bottoni difficoltà |
| `--text` | `#3D2B1F` | testo principale (marrone caldo) |
| `--text-muted` | `#8B7867` | sottotitoli, descrizioni |
| `--accent-1` | `#D26A4F` | terracotta (azione primaria) |
| `--accent-2` | `#E5A845` | ambra/zafferano (badge difficile) |
| `--accent-3` | `#8FAD7E` | salvia (badge facile) |
| `--accent-4` | `#B8553D` | rosso ruggine (badge estremo, titoli logo) |
| `--accent-blue` | `#6B9CB8` | azzurro mare (badge normale) |
| `--correct` / `--present` / `--absent` | `#6B9259` / `#E5A845` / `#B8AB9A` | colori Wordle in tono caldo |

**Tipografia:**
- Logo + numeri statistiche + lettere caselle: **Fraunces** (serif espressivo, Google Fonts).
- UI/testo: **Inter** (sans-serif moderno).

**Logo brand (schermata home):**
```
   ─── ✦ ───
   Cibo per la
      MENTE
   il gioco di oggi · PAROLA
```
- "Cibo per la" italic Playfair-style top
- "MENTE" serif bold gigante color rust (`--accent-4`) con sottile text-shadow
- Tagline distanziata, "PAROLA" evidenziato in terracotta

**Mini-logo (header in-game):** stessa composizione in piccolo nel centro, tra il pulsante back e statistiche.

**Sfondo decorativo home:**
- 8 lettere C-I-B-O-M-E-N-T in font serif italic, opacità 10%, colori accentati alternati (terracotta/ambra/salvia/azzurro), animazione `float` 18s.
- 3 gradienti radiali: ambra (alto), terracotta (basso), salvia (centro) per profondità mediterranea.

**Bottoni difficoltà:**
- Card crema con bordo colorato e badge tondo a destra contenente il numero di lettere.
- Salvia (4) → azzurro (5) → ambra (6) → ruggine (7).

**Wordle colors:**
- Correct: `#6B9259` (verde oliva)
- Present: `#E5A845` (zafferano)
- Absent: `#B8AB9A` (sabbia/taupe)

**Tastiera:** tasti color sabbia caramello (`#EDD9B6`), ombra inferiore 2px (effetto rilievo morbido).

**Modali:** crema, bordo sottile, angoli 16px, blur 3px sullo sfondo, bottoni primari pillola rotonda terracotta con ombra rust.

Mobile-first, font size con `clamp()` per adattarsi a 4-7 colonne sui telefoni.

## 7. Architettura tecnica

### File previsti
```
projects/cibo-per-la-mente/
├── parola/
│   ├── SPECIFICA_FUNZIONALE.md   (questo file)
│   ├── index.html                 (markup + CSS + JS, single-file per semplicità)
│   ├── parole.js                  (lista soluzioni + lista accettate)
│   └── README.md                  (istruzioni deploy GitHub Pages)
└── (futuri altri giochi)
```

### Tecnologie
- HTML5, CSS3 (Flexbox/Grid), JavaScript ES6 vanilla.
- Nessuna dipendenza esterna, nessun framework, nessun build step.
- Compatibilità: Chrome/Safari/Firefox ultimi 2 anni, mobile + desktop.

### Deploy
- Repository GitHub pubblico.
- GitHub Pages attivato sul branch `main`, cartella root.
- URL finale: `https://<username>.github.io/<repo>/` — da condividere su WhatsApp.

---

## 8. Roadmap implementativa

1. **Spec funzionale** ✅ (questo documento)
2. **Lista parole italiane 5 lettere** ✅ (`parole.js`, 173 soluzioni + 8176 accettate)
3. **Prototipo `index.html`** ✅ (griglia, logica colori, tastiera fisica + virtuale)
4. **Supporto mobile + tastiera virtuale** ✅ (tastiera QWERTY 26 lettere + INVIO/CANC)
5. **Persistenza localStorage + statistiche** ✅ (stato giornaliero + giocate/vinte/streak)
6. **Schermata fine partita + condivisione** ✅ (modale finale, share emoji per WhatsApp)
7. **Polish grafico** ✅ base (animazioni pop/shake, responsive, dark theme Wordle-like) — eventuale rifinitura dopo test reale
8. ⏳ **Deploy GitHub Pages + test link da WhatsApp** — da fare con account utente

---

## 8-bis. Privacy & condivisione

**Privacy modal accessibile dalla home** (link "Privacy" in footer):
- Dichiarazione "no email, no cookie, no profilazione, no pubblicità".
- Cosa raccogliamo: statistiche aggregate via Umami Cloud (anonime, IP solo per derivare paese poi scartato).
- localStorage spiegato (resta sul dispositivo, mai inviato).
- Servizi terzi elencati: GitHub Pages, Umami, Google Fonts.
- Diritti GDPR + email contatto: davide.miori@mioriconsulting.com.
- Titolare del trattamento: Davide Miori.

**Open Graph / WhatsApp preview:**
- Meta tag completi (`og:title`, `og:description`, `og:image`, `og:locale=it_IT`).
- Twitter Card `summary_large_image`.
- Immagine 1200×630 (`og-image.svg` in repo, da convertire in `og-image.png` per supporto WhatsApp).

**Favicon:** SVG inline come data URI nel `<link rel="icon">` — tile terracotta con "M" serif italic.

## 9. Decisioni e aperti

**Decisi (2026-05-10):**
- Nome gioco: **Parola**, dentro il contenitore **Cibo per la Mente**.
- Niente accenti: solo 26 lettere A-Z, parole come `PERCHÉ` escluse.
- Lista parole: la curo io partendo da fonti open italiane.

**Aperti:**
- [ ] Account GitHub e nome repo definitivo (ipotizzato `cibopermente`).
- [ ] Modalità "infinita" (rigiocabile subito con parola random) come pulsante extra dopo la partita del giorno?
- [ ] Logo / branding minimo per il contenitore.

---

*Documento da tenere aggiornato in tempo reale a ogni modifica del progetto.*
