---
name: acquisti-online
description: Cerca prodotti da comprare online e offerte (Amazon, comparatori come Trovaprezzi e idealo, grandi negozi, AliExpress) partendo da budget e requisiti, verifica prezzi, venditori e recensioni positive e negative, e consiglia cosa comprare e dove. Usala quando l'utente vuole comprare qualcosa, ad esempio "trova/consigliami un X sotto Y euro", "qual è il migliore X a questo prezzo", "è un buon prezzo?", "ci sono offerte su X", confronti tra modelli o negozi, sconti da verificare. EN — Finds products and deals online (Amazon, price comparators, major shops, AliExpress) from budget and requirements, verifies prices, sellers and positive/negative reviews, and recommends what to buy and where. Use it whenever the user wants to buy something, e.g. "find me X under €Y", "best X for the money", "is this a good price?", "any deals on X", comparing models or shops, checking a discount. Research only, never places orders.
license: MIT
---

# Acquisti online

> **EN summary:** research-only shopping assistant. It clarifies requirements (asking one question about intended use only when it changes the pick), checks what specs really mean and whether they are plausible (e.g. RMS vs peak watts, USB power limits), finds 4–6 candidates via web search, verifies live prices, sellers and the real total cost (needed accessories, shipping) in a real browser when a browsing tool is available (Claude in Chrome, Claude desktop's built-in browser, Playwright or similar; without one it falls back to web search and marks prices as indicative), skips sponsored results, checks price history, analyses the star distribution plus 6 positive and 6 negative reviews of the finalists (watching for reviews pooled across variants), then compares the exact same product's total price (incl. shipping) across other shops and price comparators with Amazon as the reference, suggesting another shop only if it saves at least €3 or 5% and is reliable; for EU users also checks the same ASIN on other EU Amazon sites (total incl. shipping, same variant); optionally checks AliExpress/Temu with the full landed cost (EU customs duty and fees), a higher threshold (20% or €10) and no knock-offs, never bypassing anti-bot checks; and only then answers with a verdict, a comparison table and a recommendation. It never adds to cart, logs in or pays. Instructions below are in Italian; the agent always answers in the user's language and adapts shops and comparators to the user's country.

Questa skill trova **cosa comprare e dove**, rispettando i requisiti dell'utente, e lo presenta **in chat** come tabella comparativa più un consiglio chiaro. L'obiettivo è che l'utente possa scegliere e cliccare il link giusto senza dover rifare la ricerca.

Rispondi nella lingua dell'utente.

**Solo ricerca.** Non aggiungere mai al carrello, non completare ordini, non creare account, non inserire indirizzi, dati di pagamento o credenziali. L'acquisto lo fa sempre l'utente.

## 1. Capire la richiesta

Estrai dalla richiesta:
- **Prodotto** e uso previsto (es. casse per PC da scrivania, per musica o per gaming).
- **Budget**: massimo, e se è rigido o indicativo.
- **Requisiti minimi** (potenza, capacità, dimensioni, connessioni, compatibilità…).
- **Priorità**: prezzo più basso, qualità/prezzo, marca affidabile, consegna rapida.
- **Paese di consegna**: deducilo dalla lingua e dal contesto (in mancanza, Italia), con prezzi nella valuta locale IVA inclusa. Usa i negozi e i comparatori di quel paese: per l'Italia Amazon.it, Trovaprezzi.it, idealo.it; altrove l'Amazon locale e i comparatori locali (es. idealo.de/.fr/.es, PriceRunner, Google Shopping). I passaggi sotto citano i siti italiani come esempio: le scritte delle pagine (es. "Venduto da", "Sponsorizzato", "Nome stile") e i siti (Trovaprezzi, CamelCamelCamel Italia) hanno equivalenti nella lingua e nel paese dell'utente. Le regole su dazi e tasse della sezione 7 valgono per chi compra nell'UE.

**Non bombardare di domande.** Se manca qualcosa di non essenziale, assumi un valore ragionevole e dichiaralo in una riga "Ipotesi" in cima. Chiedi prima di procedere solo se senza quel dato la ricerca non ha senso (es. "un regalo" senza dire cosa).

**Eccezione: l'uso previsto.** Se l'uso non è chiaro e cambierebbe il consiglio (es. casse per musica o per giochi e video, cuffie per sport o per viaggi, PC per ufficio o per gaming), fai **una sola domanda breve** prima della ricerca, con 2–3 opzioni concrete. Così eviti di dover dare due consigli alternativi alla fine. Se l'uso non sposta la scelta, non chiederlo.

## 2. Interpretare bene le specifiche

I numeri di marketing ingannano: verifica cosa significano davvero prima di dire che un prodotto rispetta un requisito.

- **Potenza audio**: distingui **RMS** (continua, quella che conta) da **picco/PMPO** (spesso il doppio o più). Se l'utente chiede "almeno 10 W", segnala quali modelli arrivano a 10 W RMS e quali solo di picco.
- **Batterie (mAh, Wh)**, **luminosità (lumen ANSI vs "lumen")**, **velocità (MB/s di lettura vs scrittura)**, **risoluzione nativa vs supportata**: stessa logica, specifica sempre quale valore stai citando.
- **Versioni e varianti**: molti prodotti hanno V2, Plus, Pro con specifiche e prezzi diversi. Verifica la versione esatta prima di confrontare. Per tastiere e prodotti con varianti per paese, controlla il **layout o la versione regionale** dell'annuncio specifico (titolo, variante selezionata, scheda tecnica): nelle ricerche compaiono spesso, a prezzi più bassi, versioni tedesche (QWERTZ), americane (US International) o di altri paesi dello stesso modello.

Se le fonti sono in conflitto su una specifica, dillo invece di sceglierne una a caso. In caso di dubbio vale la scheda del produttore.

### Le specifiche sono credibili?

Anche la scheda del produttore può esagerare. Prima di accettare un numero, confrontalo con **il prezzo, le dimensioni e l'alimentazione** del prodotto:
- **Alimentazione**: un dispositivo alimentato solo da una porta USB del PC ha poca energia a disposizione (circa 2,5 W da USB 2.0, 4,5 W da USB 3.0, di più solo con un alimentatore USB esterno o USB-C con ricarica rapida). Delle casse USB che dichiarano 10–14 W RMS, quindi, non li erogano davvero dalla porta del PC.
- **Prezzo e dimensioni**: diffida di valori fuori scala per la fascia (es. una power bank da 30.000 mAh a 10 €, un proiettore da 10.000 "lumen" a 80 €, una microSD da 1 TB a 15 €).
- **Riscontri**: se le recensioni o le foto dell'etichetta riportano un valore diverso (es. "dietro c'è scritto 3 W"), vale quello.

Quando una specifica è poco credibile, **non usarla come motivo per consigliare il prodotto**: segnalala come "dichiarata, poco credibile" e confronta i prodotti su ciò che si può verificare (recensioni, marca, test indipendenti).

## 3. Trovare i candidati

Usa **WebSearch** con più ricerche in parallelo nello stesso turno:
- guide e classifiche recenti ("migliori X sotto Y euro <anno>");
- recensioni dei modelli più citati;
- specifiche dal sito del produttore quando servono.

Seleziona **4–6 candidati** che rispettano (o si avvicinano a) budget e requisiti. Includi al massimo un'opzione leggermente fuori budget se è nettamente migliore, segnalandola come tale.

Le guide "migliori X" sono spesso pagine di affiliazione: usale per trovare i nomi dei modelli, non come prova di qualità né per i prezzi (spesso vecchi).

## 4. Verificare i prezzi attuali

La ricerca web **non mostra i prezzi aggiornati** di Amazon e simili, e i comparatori bloccano WebFetch (Trovaprezzi risponde 403). Per prezzi affidabili:

**Se hai uno strumento di navigazione** (nel resto della skill: "il browser"). Vale qualsiasi strumento che apra pagine web reali e ne legga il contenuto: Claude in Chrome (`mcp__claude-in-chrome__*`), il browser integrato dell'app desktop di Claude, Playwright o strumenti simili di altri agenti. Gli esempi tecnici sotto (nomi di strumenti come `get_page_text`, `javascript_tool`) si riferiscono a Claude in Chrome: con altri strumenti usa le funzioni equivalenti.
- Con Claude in Chrome: carica gli strumenti necessari in **una sola** chiamata ToolSearch e chiama prima `tabs_context_mcp`. Con qualsiasi browser: lavora in **una nuova scheda** (non toccare quelle dell'utente) e chiudila alla fine.
- **Limitati a leggere**: niente click su "Aggiungi al carrello", "Acquista ora", login, coupon da attivare o simili.
- Fonti, in ordine:
  1. **Amazon.it**: cerca il modello esatto, apri la scheda prodotto e leggi prezzo, venditore ("Venduto da" / "Spedito da"), valutazione e numero di recensioni. Distingui il venditore Amazon dai venditori terzi del Marketplace. Le pagine Amazon sono molto lunghe: `get_page_text` va bene per una pagina di ricerca, ma per confrontare più schede è più efficiente estrarre solo i campi utili con uno script (`javascript_tool`: `fetch('/dp/<ASIN>')` + `DOMParser`, leggendo titolo, prezzo, venditore e istogramma). Evita gli strumenti che inviano l'intera pagina a un modello (es. `find`): sulle pagine Amazon falliscono per lunghezza.
  2. Il confronto completo con gli altri negozi si fa dopo, solo sul prodotto scelto (sezione 7).
- **Risultati sponsorizzati**: nelle ricerche di Amazon e dei comparatori escludi i risultati marcati "Sponsorizzato" dalla selezione per voto e popolarità; se ne citi uno, segnalalo come sponsorizzato.
- **Condizione dell'offerta**: l'offerta mostrata in automatico nella scheda (buybox) può essere **usata** ("Amazon Seconda mano", "Usato - Come nuovo") o di un venditore terzo anche quando esiste l'offerta nuova venduta da Amazon. Controlla sempre la condizione; se è usata, cerca l'offerta **nuova** nell'elenco "Tutte le opzioni di acquisto" (con Claude in Chrome si legge da `/gp/product/ajax/aodAjaxMain/?asin=<ASIN>&pc=dp`: ogni offerta riporta prezzo, "Nuovo"/"Usato" e venditore). Confronta i prodotti sul prezzo del **nuovo**; cita l'usato solo a parte, come tale. Nella risposta avvisa l'utente di scegliere l'offerta nuova se la pagina propone l'usato.
- **Annunci doppi**: lo stesso prodotto può avere più annunci (venditori, colori o confezioni diversi) con prezzi e voti diversi. Riporta l'annuncio che consigli e, se un altro dello stesso prodotto costa meno o ha un voto molto diverso, dillo.
- **Costo reale**: oltre al prezzo, considera spedizione, accessori **necessari** per usare il prodotto come previsto (es. alimentatore non incluso, cavo, adattatore, scheda di memoria) e garanzia/reso (Amazon e grandi catene contro venditori terzi o esteri). Se un accessorio necessario manca, aggiungi il suo costo indicativo e usa il totale per confrontare i prodotti e per verificare il budget.
- **Storico prezzi** per i prodotti Amazon sopra i ~30 €, o quando c'è uno "sconto": controlla su **CamelCamelCamel** (it.camelcamelcamel.com), **Keepa** o il grafico "ultimi 6 mesi" della scheda Trovaprezzi se il prezzo attuale è davvero basso rispetto agli ultimi mesi. Uno sconto calcolato su un prezzo di listino mai praticato non è un'offerta: dillo.

**Se non hai un browser** la skill funziona lo stesso, con meno precisione: usa solo la ricerca web (WebSearch/WebFetch). Riporta i prezzi trovati nelle fonti con data e fonte, marcali come "indicativo" e invita l'utente a verificarli prima di comprare. Non inventare mai un prezzo. Venditore, risultati sponsorizzati, istogramma delle stelle e comparatori spesso non saranno visibili: analizza quello che trovi (es. recensioni pubblicate su siti di test o forum) e **dichiara in cima alla risposta** che hai lavorato senza browser e quali controlli mancano.

Scala l'impegno sul valore dell'acquisto: per oggetti sotto ~20 € bastano pochi controlli; sopra ~200 € verifica più negozi, lo storico prezzi e più recensioni.

## 5. Affidabilità del venditore

Segnala come rischio:
- prezzi molto sotto la media degli altri negozi;
- venditori terzi con poche valutazioni o valutazioni recenti negative;
- spedizione dall'estero con tempi di settimane per un negozio che sembra italiano;
- negozi senza partita IVA, indirizzo o contatti verificabili;
- marchi sconosciuti con specifiche irrealistiche (es. "100 W" in una cassa da 15 €).

Preferisci, a parità di prezzo, il venditore con reso facile (Amazon, grandi catene).

## 6. Recensioni

**Non dare nessun consiglio né classifica finale prima di aver completato questo passaggio.** I finalisti dei passi 3–5 sono provvisori: scegli i 2–3 più promettenti, analizza le loro recensioni come descritto sotto, e solo dopo decidi l'ordine. Un consiglio basato su specifiche dichiarate e recensioni in evidenza può essere ribaltato dalle negative.

Le recensioni in evidenza sulla scheda prodotto sono quasi sempre positive: leggere solo quelle dà un quadro distorto. I difetti ricorrenti (guasti dopo pochi mesi, fruscio, volume insufficiente, pezzi diversi dalle foto) emergono solo dalle negative. Per questo, **solo per i 2–3 finalisti** (non per tutti i candidati, per non allungare troppo la ricerca):

1. **Distribuzione delle stelle**: riporta la % di 5★, 4★, 3★, 2★ e 1★ dall'istogramma di Amazon (o del negozio), oltre al voto medio e al numero totale di recensioni. Il dato chiave è la **% di 1–2★**: sopra il ~10–12% è un campanello d'allarme, da segnalare. Le percentuali vanno prese dall'istogramma, non calcolate sulle recensioni lette (un campione di 6+6 non è rappresentativo).
2. **6 recensioni positive** (4–5★) e **6 negative** (1–3★), preferendo le più recenti (ultimi ~12 mesi) e quelle con testo sostanzioso. Su Amazon le negative si filtrano dalla pagina recensioni (`/product-reviews/<ASIN>?filterByStar=critical&sortBy=recent`, le positive con `filterByStar=positive`); se la pagina non è leggibile, usa quelle disponibili e dichiara quante ne hai lette.
3. **Temi ricorrenti**: raggruppa ciò che hai letto in pro e contro, indicando quante recensioni citano ogni tema (es. "fruscio: 3 negative su 6", "suono pulito: 5 positive su 6"). Un difetto citato da più recensioni indipendenti pesa molto più di un caso isolato; un singolo prodotto difettoso arrivato rotto pesa poco.
4. **Impatto sulla scelta**: se le negative rivelano un difetto serio o ricorrente (guasti, problemi che toccano il requisito dell'utente), abbassa il prodotto in classifica e dillo esplicitamente. Dai più peso alle recensioni di chi ha un uso simile a quello dell'utente.
5. **Recensioni che non riguardano il prodotto**: su Amazon le recensioni sono spesso raggruppate tra varianti (colori, taglie, versioni vecchie e nuove: lo indica la riga "Nome stile"/"Colore" sopra ogni recensione) o arrivano da un annuncio riciclato per un prodotto diverso. Se le recensioni parlano di un modello, una versione o un oggetto diverso da quello in vendita, o se la maggior parte risale a prima di un cambio di versione, dillo e dai più peso a quelle recenti sulla variante giusta. Segnala anche raffiche di recensioni a 5★ molto brevi e simili tra loro nello stesso periodo.

Se le recensioni smentiscono una specifica o una guida, correggi la classifica e dillo.

Non riportare giudizi di recensioni che non hai letto: le anteprime dei motori di ricerca non bastano. Se hai letto meno recensioni del previsto, dillo.

## 7. Confronto prezzi tra negozi

Dopo aver scelto il prodotto consigliato, verifica se **lo stesso identico prodotto** costa meno altrove, usando Amazon come riferimento. Fallo sul prodotto consigliato; sul secondo solo se la differenza di prezzo tra i due è piccola (~10% o meno), perché il confronto potrebbe cambiare l'ordine.

1. **Riferimento Amazon**: prezzo, spedizione, **totale**, venditore (Amazon o terzo) e tempi di consegna.
2. **Stesso prodotto esatto**: cercalo su **Trovaprezzi.it** e **idealo.it** (o i comparatori del paese dell'utente) e, se servono, sui grandi negozi (MediaWorld, Unieuro, eBay.it, sito del produttore). Controlla codice modello, versione, colore e condizione: escludi offerte di versioni diverse (es. V2 contro V3), confezioni diverse, usato o ricondizionato, salvo indicarle a parte come tali.
   - Trovaprezzi blocca WebFetch ma si apre con il browser: la ricerca è `https://www.trovaprezzi.it/categoria.aspx?id=-1&libera=<modello>` e porta direttamente alla scheda del prodotto, dove ci sono le offerte, l'opzione "Includi spese spedizione" e il grafico prezzi degli ultimi 6 mesi. I nomi dei negozi spesso sono loghi: leggili dagli attributi `alt` delle immagini o dal link "Vedi offerta"; se non riesci, scrivi "negozio non identificato" invece di indovinare.
   - Senza browser, usa WebSearch sul modello esatto e riporta i prezzi come "indicativi", con data e fonte.
3. **Confronta il totale** (prezzo + spedizione), non il solo prezzo: un negozio con 0,20 € in più e 7,90 € di spedizione costa molto di più. Annota anche i tempi di consegna.
4. **Affidabilità del negozio** più economico: valutazioni sul comparatore, Trustpilot o Feedaty, sede in Italia o UE, diritto di recesso, reso. Applica i criteri della sezione 5.
5. **Soglia di convenienza**: suggerisci un negozio diverso da Amazon solo se il totale è più basso di **almeno 3 € o del 5%** (vale il maggiore dei due) **e** il negozio è affidabile. Sotto la soglia, Amazon resta il consiglio: reso facile e consegna rapida valgono la differenza. Dillo esplicitamente.

### Amazon di altri paesi UE

Per un utente nell'UE, controlla anche lo **stesso ASIN** sugli Amazon europei che spediscono nel suo paese (per l'Italia: Amazon.de, .fr, .es, .nl). Usano lo stesso account, non hanno dazi e la garanzia legale resta di 2 anni; a volte costano meno, soprattutto l'elettronica su Amazon.de.
- Apri direttamente `https://www.amazon.<paese>/dp/<ASIN>`: stesso ASIN = stesso prodotto, ma verifica comunque che la variante sia identica.
- Confronta il **totale con la spedizione nel paese dell'utente** (spesso a pagamento e fuori da Prime: se la pagina non mostra il costo per il suo paese, dillo invece di stimarlo) e i tempi di consegna.
- Escludi o segnala le differenze di versione: layout della tastiera, lingua dei menu o delle istruzioni, tipo di spina.
- Applica la stessa soglia dei negozi europei (almeno 3 € o il 5%) e ricorda che il reso è possibile ma meno comodo.

### Marketplace extra-UE (AliExpress, Temu)

Confronto **facoltativo**, con regole diverse dai negozi europei: tempi, costi nascosti, resi e garanzia sono peggiori, e spesso il prodotto "uguale" non è lo stesso prodotto.

**Quando farlo**: se l'utente lo chiede, oppure se il prodotto è di una marca venduta anche lì (es. Fifine, Tribit, Baseus, Ugreen, Anker, Xiaomi) o è un oggetto generico dove la marca conta poco (cavi, supporti, accessori, organizer). Non dare per scontato che una marca ci sia o non ci sia: **verifica con una ricerca**. Spesso i prodotti di marche occidentali (es. Logitech) sono venduti da rivenditori terzi invece che da uno store ufficiale: in quel caso controlla nome e % di feedback positivi del venditore, segnala il rischio di falsi o merce non originale e l'assenza di garanzia italiana, e considera l'offerta solo se il venditore è affidabile. Se il modello cercato non c'è (compaiono solo ricambi o prodotti simili), dillo in una riga invece di proporre imitazioni.

**Stesso prodotto, non imitazioni**: per i prodotti di marca cerca lo **store ufficiale** del marchio ("Official Store"/"Store ufficiale") e lo stesso codice modello. Prodotti senza marca o "simili" non sono lo stesso prodotto: se li citi, indicali a parte come alternative di qualità sconosciuta. Prezzi molto più bassi per un prodotto di marca indicano un probabile falso: segnalalo e non consigliarlo.

**Come cercare**:
- **AliExpress** si legge con il browser: `https://it.aliexpress.com/w/wholesale-<parole-separate-da-trattini>.html`. I risultati comprendono annunci e prodotti non pertinenti: tieni solo quelli che corrispondono al modello. Preferisci le offerte con **"Spedizione da: Italia / Unione europea"** (filtro nella pagina) e annota se l'annuncio indica **"dazi coperti"**.
- **Temu** di solito mostra una **verifica di sicurezza** (puzzle) appena si apre una ricerca. **Non tentare mai di risolverla o aggirarla.** Prova prima con WebSearch (prezzi "indicativi"); se non basta, proponi all'utente due strade: completa lui la verifica nella scheda e poi tu leggi la pagina, oppure gli lasci le parole di ricerca e il **prezzo massimo** oltre il quale Temu non conviene rispetto ad Amazon (calcolato con la soglia sotto). Temu indica la merce già in Europa come "magazzino locale".

**Costo finale**: prezzo + spedizione + **dazi e tasse sulle spedizioni extra-UE**, salvo merce spedita da magazzini UE o annunci con dazi già inclusi. Dal 1° luglio 2026 c'è un dazio UE di **3 € per articolo** (non per pacco) sulle spedizioni sotto i 150 €, su cui si paga anche l'IVA; nel 2026 erano previsti anche contributi aggiuntivi di circa 2 € a pacco con date incerte. **Verifica con una ricerca web le regole in vigore al momento** prima di calcolare il totale, e indica la fonte.

**Soglia di convenienza più alta**: consiglia AliExpress o Temu solo se il costo finale è più basso di Amazon di **almeno il 20% o 10 €** (vale il maggiore dei due). Sotto la soglia, i tempi di consegna (spesso 1–3 settimane dalla Cina), i resi più complicati (spese e contributi spesso non rimborsati) e la garanzia non valgono il risparmio. Per l'elettronica segnala anche spina, certificazione CE e assenza di garanzia italiana.

Nella risposta, metti questi marketplace in una **riga separata** della tabella "Dove comprarlo", con tempi di consegna e avvertenze su resi e garanzia.

## 8. Come rispondere

Formato, nell'ordine:

1. **Verdetto in grassetto** in una o due righe: cosa si trova con quel budget e se i requisiti sono realistici (es. "Con 30 € i 10 W si trovano, ma quasi sempre di picco: 10 W RMS veri sono rari").
2. **Tabella comparativa**: modello, specifica chiave richiesta (indicando RMS/picco ecc. e se è poco credibile), prezzo + negozio (con link), costo reale se diverso dal prezzo (accessori necessari, spedizione), voto/recensioni, note brevi. I modelli fuori budget o che non rispettano un requisito vanno segnalati o barrati.
   Per i finalisti aggiungi un blocco **Recensioni** con: distribuzione stelle (% 5★…1★, evidenziando la % di 1–2★) e i temi ricorrenti di positive e negative con il conteggio (es. "fruscio: 3/6 negative").
3. **Consiglio finale**, deciso solo dopo l'analisi delle recensioni (sezione 6): la scelta migliore per l'uso e la priorità dell'utente, con il motivo in una frase che citi dati verificati (recensioni, costo reale), non solo specifiche dichiarate. Aggiungi un'alternativa solo se serve davvero (es. "se vuoi spendere il meno possibile…").
4. **Dove comprarlo**: piccola tabella del confronto della sezione 7 (negozio, prezzo, spedizione, **totale**, consegna, affidabilità), con Amazon come prima riga di riferimento, e una conclusione in una frase: "Amazon è il più conveniente" oppure "Su <negozio> risparmi X € spedizione inclusa, venditore affidabile".
5. **Avvertenze** solo se servono: prezzi da verificare, versione da controllare, venditore da evitare, offerta usata proposta in automatico al posto della nuova.
6. **Fonti**: elenco dei link usati, come link markdown.

Mantieni la risposta più compatta possibile: frasi brevi, niente paragrafi lunghi, tabelle invece di elenchi quando confronti. I blocchi Recensioni e Dove comprarlo vanno tenuti essenziali (una riga per prodotto o negozio). Se i risultati sono scarsi o incerti, dillo chiaramente invece di riempire la tabella.

## 9. Dopo la risposta

Offri in una riga i passi successivi utili, ad esempio: controllare i prezzi dal vivo con un browser (se non l'hai fatto), approfondire un modello, o cercare un'alternativa con requisiti diversi.
