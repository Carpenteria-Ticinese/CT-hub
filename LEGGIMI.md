# Hub — Carpenteria Ticinese Sagl

## Cosa fa
Pagina unica che apre tutte le app (card + ricerca «Cosa devi fare?»).
Nessun login, nessun database, nessun dato dell'utente. Tutti vedono tutte le card:
ogni app controlla da sola chi può entrare (col suo login).
Ordine delle card: lo decide solo Umberto.

## File
- `index.html` — tutto l'Hub (HTML, CSS e JS dentro). Le card sono nell'elenco `APPS`.
- `aggiorna.js` — avviso «🔄 Nuova versione: aggiorna» (uguale in tutte le app, vedi sotto).
- `versione.json` — numero della versione pubblicata, es. `{"versione":5}`.
- `manifest.json`, `icone/` — per installare l'Hub sul telefono/PC.
- `spec.html` — vecchia pagina «Vedi progetto», nessuna card la apre più.

## Tabelle possedute
Nessuna.

## Versione
Il numero è scritto in 4 punti, sempre uguali:
`versione N` in fondo alla pagina · `?v=N` su manifest, icone e `aggiorna.js` ·
`data-versione="N"` sulla riga di `aggiorna.js` · `versione.json`.

## Apertura delle app (funzione `openApp` in index.html)
Ogni app si apre nella sua scheda, con nome fisso `ct_app_<id>`. Mai un clic che non fa niente:
1. nessuna scheda con quel nome → scheda nuova con l'app;
2. scheda sullo stesso sito dell'Hub (github.io): se è già sull'app la si porta davanti senza
   ricaricarla; se è su un indirizzo vecchio la si manda all'indirizzo giusto;
3. scheda su un altro sito (es. Preventivi su workers.dev): l'Hub prova a portarla davanti;
4. se dopo 0,7 s l'Hub è ancora davanti → l'app si apre in una scheda nuova;
5. se il browser blocca le schede nuove → l'app si apre al posto dell'Hub.

## aggiorna.js — come si usa (anche nelle altre app)
Regola: mai versioni vecchie bloccate sul telefono o sul PC.
1. Copiare `aggiorna.js` così com'è accanto a `index.html` dell'app.
2. Creare `versione.json` accanto a `index.html`: `{"versione":5}`.
3. In fondo a `<body>`: `<script src="aggiorna.js?v=5" data-versione="5"></script>`.
4. A ogni pubblicazione: stesso numero, più alto, in `versione.json`, `data-versione` e `?v=`.

Cosa fa: legge `versione.json` all'apertura, quando la pagina torna visibile e ogni 10 minuti,
saltando ogni memoria (`cache: 'no-store'` + `?t=<ora>`, così anche la copia di GitHub Pages
tenuta fino a 10 minuti). Se il numero è più alto di `data-versione` mostra in alto un tasto
grande «🔄 Nuova versione: aggiorna» (e «Dopo»). Il tasto ricarica la pagina con `?v=<nuovo>`.
Se il file manca o la rete non va non fa niente: mai blocchi.
Le app non hanno service worker (controllato il 9.11.2026): non serve altro.

## Come si prova
Prove automatiche (Playwright, fuori dal repo): primo clic, secondo clic con app davanti
e con Hub davanti, Hub ricaricato, scheda col nome giusto su indirizzo vecchio (stesso sito
e altro sito), finestre bloccate, telefono 375 px, banner con `versione.json` più alto,
«Dopo», «Aggiorna», `versione.json` mancante, nessun errore JS.
A mano: aprire l'Hub, cliccare ogni card, cliccarla una seconda volta, provare a 375 px.
Dopo la pubblicazione: Ctrl+F5 (o il tasto «🔄 Nuova versione: aggiorna»).
