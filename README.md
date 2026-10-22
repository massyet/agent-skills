# Agent Skills

🇮🇹 [Italiano](#italiano) · 🇬🇧 [English](#english)

---

## Italiano

### `acquisti-online`

Un assistente per gli acquisti online che **fa solo ricerca**. Gli dici cosa ti serve, il budget e i requisiti minimi; lui trova i prodotti adatti, verifica prezzi e venditori e ti consiglia cosa comprare spiegando perché.

**Cosa fa**
- Se l'uso previsto cambia il consiglio, fa una sola domanda all'inizio.
- Interpreta le specifiche nel modo giusto (es. watt RMS contro watt di picco, versioni V2/Plus/Pro) e segnala quelle poco credibili.
- Trova 4–6 prodotti con la ricerca web, escludendo i risultati sponsorizzati.
- Verifica i prezzi attuali, il venditore, il costo reale (accessori necessari, spedizione) e lo storico prezzi (CamelCamelCamel / Keepa) per riconoscere i finti sconti.
- Segnala i venditori poco affidabili.
- Per i finalisti analizza la **distribuzione delle stelle** (% di voti da 1 a 5★) e legge **6 recensioni positive e 6 negative**, contando i difetti ricorrenti e segnalando le recensioni che riguardano altre varianti.
- Per il prodotto consigliato confronta il **prezzo totale** (spedizione inclusa) dello stesso identico prodotto su altri negozi e comparatori (Trovaprezzi, idealo…), con Amazon come riferimento. Suggerisce un altro negozio solo se si risparmiano almeno 3 € o il 5% e il negozio è affidabile.
- Controlla lo stesso prodotto anche sugli **Amazon di altri paesi UE** (Amazon.de, .fr, .es…), che usano lo stesso account e spediscono in Italia: confronta il totale con la spedizione e verifica che la versione sia identica (tastiera, lingua, spina).
- Se ha senso, confronta anche **AliExpress e Temu**: solo lo stesso prodotto (niente imitazioni), costo finale con dazi e tasse UE, consigliati solo con un risparmio di almeno il 20% o 10 €. Non aggira mai le verifiche anti-bot: se Temu le mostra, chiede all'utente.
- Decide la classifica solo dopo l'analisi delle recensioni, e risponde con un verdetto, una tabella comparativa, un consiglio, dove comprarlo al prezzo migliore e le fonti.

**Cosa non fa mai:** aggiungere al carrello, fare login, inserire dati di pagamento o completare ordini. L'acquisto resta sempre a te.

**Esempio**
> Trova delle casse per PC, budget 30 euro, almeno 10W

**Requisiti**
- Un agente compatibile con le [Agent Skills](https://skills.sh/) (Claude Code, Codex, Cursor, Gemini CLI, …).
- Consigliato, **non obbligatorio**: uno strumento di navigazione che permetta all'agente di aprire i siti, come **Claude in Chrome**, il browser integrato dell'app desktop di Claude o Playwright.
  - **Con il browser**: prezzi attuali, venditore, risultati sponsorizzati, distribuzione delle stelle, recensioni negative, Trovaprezzi e AliExpress.
  - **Senza browser**: la skill funziona lo stesso con la sola ricerca web. Trova e confronta i prodotti, ma i prezzi sono "indicativi" e l'analisi delle recensioni è parziale; lo dichiara nella risposta.
  - Temu mostra spesso una verifica anti-bot: la skill non la aggira mai e chiede all'utente.

**Installazione**
```bash
npx skills add massyet/agent-skills@acquisti-online
```

Le istruzioni della skill sono in italiano, ma l'agente risponde nella lingua dell'utente e usa negozi e comparatori del suo paese.

---

## English

### `acquisti-online` ("online shopping")

A **research-only** shopping assistant. Tell it what you need, your budget and minimum specs; it finds suitable products, verifies prices and sellers, and recommends what to buy, explaining why.

**What it does**
- Asks one upfront question about intended use, only when it changes the pick.
- Reads specs correctly (e.g. RMS vs peak watts, V2/Plus/Pro variants) and flags implausible ones.
- Finds 4–6 candidates via web search, skipping sponsored results.
- Verifies current prices, sellers, the real total cost (needed accessories, shipping) and price history (CamelCamelCamel / Keepa) to spot fake discounts.
- Flags unreliable sellers.
- For the finalists, analyses the **star distribution** (% of 1–5★ ratings) and reads **6 positive and 6 negative reviews**, counting recurring issues and flagging reviews about other variants.
- For the recommended product, compares the **total price** (incl. shipping) of the exact same item across other shops and price comparators (Trovaprezzi, idealo…), with Amazon as the reference. It suggests another shop only if it saves at least €3 or 5% and the shop is reliable.
- Checks the same product on **other EU Amazon sites** (Amazon.de, .fr, .es…), which share the same account and ship across the EU: compares the total including shipping and makes sure the version is identical (keyboard layout, language, plug).
- When it makes sense, also checks **AliExpress and Temu**: same product only (no knock-offs), full landed cost with EU duty and fees, recommended only if it saves at least 20% or €10. It never bypasses anti-bot checks: if Temu shows one, it asks the user.
- Ranks only after the review analysis, and answers with a verdict, a comparison table, a recommendation, where to buy it cheapest and sources.

**What it never does:** add to cart, log in, enter payment details or place orders. You always make the purchase yourself.

**Example**
> Find me PC speakers under €30, at least 10W

**Requirements**
- An agent that supports [Agent Skills](https://skills.sh/) (Claude Code, Codex, Cursor, Gemini CLI, …).
- Recommended, **not required**: a browsing tool that lets the agent open websites, such as **Claude in Chrome**, Claude desktop's built-in browser or Playwright.
  - **With a browser**: live prices, sellers, sponsored results, star distribution, negative reviews, Trovaprezzi and AliExpress.
  - **Without a browser**: the skill still works using web search only. It finds and compares products, but prices are "indicative" and the review analysis is partial; it says so in the answer.
  - Temu often shows an anti-bot check: the skill never bypasses it and asks the user.

**Install**
```bash
npx skills add massyet/agent-skills@acquisti-online
```

The skill's instructions are written in Italian, but the agent answers in the user's language and uses shops and price comparators from the user's country.

---

## License / Licenza

[MIT](LICENSE)
