# Humanization Techniques

> Doctrine for the `human-writer-de` skill. The ten moves that turn AI-shaped prose into human-shaped prose. Apply in WRITE mode while drafting; apply in CLEAN mode as a targeted checklist. The doctrine is shared across the `human-writer` per-language family; the worked examples here are in German because that's the surface the analyzer matches against.

The techniques are ordered by impact-per-edit. If you can only apply one, apply #1. If you can apply two, add #5. If you have time for everything, the full ten will cut analyzer score by 30–50 points on a typical AI draft.

Each technique below answers the same four questions:

1. **Definition.** What is it?
2. **Why it works.** Which statistical / stylistic signal does it disrupt?
3. **Worked examples.** Before and after, in EN plus DE where applicable.
4. **How to apply.** Proactive in WRITE mode, targeted in CLEAN mode.

> German typography note: the worked examples below use the **native spaced en-dash "–" (U+2013)** for asides — this is correct German and is never flagged. They use **German quotes „ "**. Never substitute the foreign em-dash "—" (U+2014); that is the very tell the skill hunts.

---

## 1. Vary sentence length deliberately

**Definition.** Alternate short (≤ 6 words) and long (≥ 25 words) sentences. Aim for a standard deviation of sentence lengths ≥ 8.

**Why it works.** AI prose averages 18–22 words per sentence with a low standard deviation (typically 3–5). The analyzer measures this via `sentence_length_stdev` and flags anything under ~8. Variance is the cheapest, highest-signal humanization edit available.

### Rhythm patterns to build in

| Pattern | Shape | Use case |
|---|---|---|
| **Short-short-long** | 5w + 4w + 28w | Set up a claim, then expand |
| **Long-short** | 30w + 4w | Build then punch (very human) |
| **Fragment-long** | 2w + 25w | Topic + dump |
| **Short-medium-fragment** | 6w + 15w + 3w | Cadence variation |
| **Run-on with semicolon** | 35w with `;` | One thought, two beats |

Use semicolons to extend without resetting rhythm. Use periods to reset hard. Use commas to slow without breaking. Explicit fragments ("Das war's.", "Und fertig.", "Punkt.") are the strongest rhythm breakers.

### Worked example (EN)

Bad (uniform; lengths 13, 13, 11, 12; stdev ~1):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> The pricing tool exports a CSV file with margin tiers per SKU. It updates daily based on competitor data scraped from major marketplaces. Users can filter by category, region, or supplier. The dashboard shows historical price evolution over the past 90 days.
<!-- human-writer:ignore-end -->

Good (varied; lengths 2, 8, 25, 18, 5, 8; stdev ~9):

> CSV out. One row per SKU, one column per marker. The pricing tool updates the file every night, pulling competitor data from Amazon, eBay, and three regional marketplaces I won't name in public. Filter it in Excel — category, region, supplier — and you get the same view our procurement team uses. Ninety days of history. That's usually enough to spot a competitor stockout.

### Worked example (DE)

Bad (gleichförmig; Längen 14, 13, 15, 12; Abweichung ~1,2):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Unser Analyse-Tool exportiert für jeden Artikel des Katalogs eine CSV-Datei. Es aktualisiert sich jede Nacht anhand der Daten der Konkurrenz. Die Nutzer können nach Kategorie, Region oder Lieferant filtern. Das Dashboard zeigt die Preisentwicklung über 90 Tage.
<!-- human-writer:ignore-end -->

Good (Längen 3, 12, 28, 5, 18; Abweichung ~10):

> CSV-Export. Eine Zeile pro Artikel, eine Spalte pro Preismarker. Das Tool läuft jede Nacht und holt die Preise von Amazon, eBay und drei regionalen Marktplätzen, die ich hier lieber nicht nenne. Du filterst in Excel, fertig. Neunzig Tage Historie – das deckt fast alle Lagerausfälle der Konkurrenz ab, die wir 2025 gesehen haben.

### Worked example (DE) (second pattern)

Bad:

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Der Scraper holt die Daten von der Ausgangsseite. Er bereinigt sie nach den konfigurierten Regeln. Er speichert sie in einer PostgreSQL-Datenbank. Er stellt sie über eine REST-API bereit.
<!-- human-writer:ignore-end -->

Good:

> Der Scraper schluckt die Seite, bereinigt sie mit unseren selbstgebauten Regeln (die nie ein Redesign des Ziels überleben) und schiebt sie nach Postgres. Drei Spalten. Fertig. Die REST-API holt den Rest – und wir lassen die Ausgabe in JSON, weil das Frontend genau das will, ohne Umwege.

### How to apply

**WRITE mode (proactive).** As you draft, force yourself to drop a one- or two-word sentence after every long one. If you've written three sentences of similar length, the next one *must* be a fragment or a 30+ word run-on.

**CLEAN mode (targeted edit).** Run `analyze.py` and look at `sentence_length_stdev`. If under 8, identify the three longest paragraphs and rewrite them by:
1. Splitting one mid-length sentence into a fragment + a sentence.
2. Joining two short sentences into one long, comma-spliced or semicolon-linked sentence.
3. Adding one explicit fragment ("Fertig.", "Das war's.", "Punkt.").

---

## 2. Inject one opinion or specific anecdote per ~300 words

**Definition.** Every 300 words or so, insert one of: a first-person take, a named entity, a specific number, or a small concrete story.

**Why it works.** AI prose is information-dense but opinion-empty and entity-light. LLMs default to generic claims because generic claims minimize the chance of being wrong. Specific entities ("Riesling 2018", "47 Req/s", "Marie beim Einkauf") are statistically rare in AI output; they're high-signal markers of authored prose because they couldn't be generated without first-hand context.

### What counts as an "opinion"

A take that someone could disagree with. Not "X ist gut für Y"; that's a fact-claim. But "ich würde X bei allem unter 100 SKUs weglassen, weil sich der ROI nicht lohnt" is an opinion: opinionated, specific, defensible.

| Not an opinion | Opinion |
|---|---|
| "Leistung ist wichtig" | "Unter 1k QPS ist Leistung das Einzige, was zählt; alles andere ist Zeitverschwendung." |
| "Wähl das richtige Werkzeug" | "Nimm kein Playwright für statische Seiten. Cheerio ist schneller und wartungsärmer." |
| "Pricing ist wichtig" | "Die meisten Pricing-Strategien scheitern, weil das Team, das sie festlegt, nie mit dem spricht, das kalkuliert." |

### What counts as "specific"

A named entity (person, place, product, version), a date, a number with units, or a concrete event. Not "ein großer Marktplatz"; sag Amazon. Not "kürzlich"; sag "März 2024". Not "die Nutzer"; sag "das Einkaufsteam eines unserer Weinkunden".

### How to inject without breaking flow

Three placements work:

1. **Mid-paragraph aside.** "Der Actor (wir haben ihn mit 47 Req/s auf einer 2-GB-Instanz gemessen) verwaltet die Pakete …"
2. **End-of-paragraph kicker.** "… und so. Ehrlich gesagt: Da hören die meisten auf – und da fangen wir an."
3. **New sentence between two claims.** "Pricing-Tools brauchen Rate-Limits. Die Ziel-API drosselt dich übrigens bei 60/min. Also baut man darum herum."

### Bad vs good (DE)

Bad (vage Binsenweisheit):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Scraping im großen Stil erfordert sorgfältig ausgewählte Werkzeuge. Die Wahl des richtigen Frameworks kann einen erheblichen Unterschied bei der Leistung ausmachen.
<!-- human-writer:ignore-end -->

Good (spezifische Haltung):

> Scraping im großen Stil läuft auf eine Entscheidung hinaus: Akzeptierst du den Bot-Check der Zielseite oder kämpfst du dagegen an? Ein getarnter Headless-Browser über Residential-Proxys kämpft (~0,30 $/1k Seiten). Ein nackter HTTP-Client akzeptiert (~0,05 $/1k, aber nach zwei Monaten bist du gesperrt). Man kämpft für die hochwertigen Ziele; man akzeptiert für die unkritischen.

### How to apply

**WRITE mode.** Before writing, list 3 specific facts/anecdotes/opinions you can drop into the piece. Place one every ~300 words.

**CLEAN mode.** Search the draft for paragraphs that contain zero proper nouns, zero numbers, and zero first-person markers. Each such paragraph needs one injection. If the draft has *none* across the whole text, that's an emergency — the piece reads as generic and no amount of sentence-length variance will save it.

---

## 3. Use asymmetric bullets

**Definition.** In any bulleted list, deliberately vary the length, structure, opening word, and grammatical shape of each item.

**Why it works.** AI bullets are parallel by default: same verb (`Bauen / Bauen / Bauen`), same length (8–12 words each), same shape (verb + object). The analyzer flags this when > 80% of bullets share the same first or last word. Asymmetry breaks the fingerprint.

### Three asymmetry axes

| Axis | Bad (symmetric) | Good (asymmetric) |
|---|---|---|
| **Length** | All 6–8 words | Mix of 2-word fragments and 25-word callouts |
| **Opener** | All start with a verb | Mix verbs, nouns, questions, fragments |
| **Shape** | All `verb + object` | Mix commands, observations, questions, mini-paragraphs |

### Worked example (DE)

Bad (symmetrisch in Länge und Verb):

```
- Produktseiten scrapen
- Preisdaten extrahieren
- Ergebnisse im Dataset speichern
- Nach CSV exportieren
```

Good (asymmetrisch):

```
- Produktseiten scrapen (ein HTTP-Parser für Statisches, ein Headless-Browser für das Widerspenstige – siehe `selectors.ts`)
- Preise: Listenpreis, Angebot und das "Du sparst" getrennt erfassen, weil Amazon nach Lust und Laune rundet
- Datensatz → CSV: ein einziger CLI-Befehl, fertig
- Und danach? Da hören die meisten auf. Wir hängen es an Metabase für die Trendansicht.
```

What changed: bullet 1 has a parenthetical with a code path; bullet 2 opens with a noun and uses a colon callout; bullet 3 is one short clause with an arrow; bullet 4 is a rhetorical question + two sentences.

### Sub-bullets for asymmetry

Adding a single sub-bullet under one item (and only one) breaks the visual rhythm hard:

```
- Produktseiten scrapen
  - Headless-Browser nur für die JS-lastigen Ziele – kostet ~200 ms/Seite
- Preisextraktion
- CSV-Export
```

The lone sub-bullet kills the AI shape. It signals "a human chose to elaborate exactly here."

### When symmetric bullets ARE appropriate

Parallel lists where the reader compares like-with-like deserve symmetric formatting: step-by-step procedures, API reference tables, comparison matrices, pricing tiers. There, parallelism is *informational*. The rule: **prose-embedded lists should be asymmetric; reference/comparison structures can stay symmetric**.

### How to apply

**WRITE mode.** When you reach for a bullet list, draft it normally, then deliberately rewrite at least 2 of the 4 bullets to use a different shape.

**CLEAN mode.** Check `bullet_parallelism_ratio` from `analyze.py`. If ≥ 0.80, rewrite half the bullets to NOT start with the dominant verb.

---

## 4. Break tricolons: vary list sizes

**Definition.** Resist the "rule of three" reflex. Use lists of 2, 4, or 5 items. Cap the explicit three-item `und`-joined tricolon at 1 per 200 words.

**Why it works.** AI prose is saturated with tricolons. Typical specimens:

<!-- human-writer:ignore-start (citation: tricolon tells quoted, not used) -->
"schnell, zuverlässig und skalierbar", "bauen, testen und ausliefern", "klein, mittel und groß".
<!-- human-writer:ignore-end -->

The cadence is comforting and tidy, which is exactly why LLMs default to it. The analyzer flags density above 1 per 200 words.

### Specific alternatives

| Reflex | Alternative | Example |
|---|---|---|
| Tricolon (3 items, "und") | **List of 2** | "Schnell und billig." |
| Tricolon (3 items, "und") | **List of 4** | "Schnell, billig, gecacht und geprüft." |
| Tricolon | **List of 5 with asyndeton** | "Schnell, billig, gecacht, geprüft, in einem Befehl ausrollbar." |
| Tricolon | **List of 3 with asyndeton (no "und")** | "Schnell, zuverlässig, skalierbar." |
| Tricolon | **Pair + parenthetical** | "Schnell und zuverlässig (auch billig, aber das ist Beigabe)." |

Asyndeton — dropping the final "und" — is the cheapest variation. It keeps the three-beat rhythm but loses the AI-signature `, und` connector.

### Worked example (DE)

Bad (drei Tricolons in zwei Sätzen):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Das Tool ist schnell, zuverlässig und präzise. Es verwaltet Pakete, Varianten und Sprachen. Es läuft täglich, wöchentlich und auf Abruf.
<!-- human-writer:ignore-end -->

Good (ein Tricolon-Budget ausgegeben; Listengrößen variiert):

> Das Tool verwaltet Pakete und Varianten in 12 Sprachen. Tägliche Läufe, oder auf Abruf für eine einmalige Prüfung. Schnell, zuverlässig, präzise: in dieser Reihenfolge optimieren wir.

### How to apply

**WRITE mode.** Keep a mental "tricolon budget" of 1 per 200 words. If you've already used one, force the next list into 2 or 4 items, or use asyndeton.

**CLEAN mode.** Search the draft for `, und ` followed by a noun ending the sentence. Count instances. If > 1 per 200 words, rewrite the lowest-impact occurrences first.

---

## 5. Cut all hedging openers

**Definition.** Delete the AI-templated qualifier phrases that front-load sentences. State the claim directly.

**Why it works.** Hedging openers are the most distinctive AI signature in long-form prose. They're space-filler with zero information value. Removing them is the highest words-saved-per-edit move available.

### Full forbidden list (DE)

<!-- human-writer:ignore-start (citation table: tells quoted, not used) -->
| Forbidden opener | Why it's a tell |
|---|---|
| "Es ist wichtig zu beachten, dass" | The #1 DE topic-sentence tell |
| "Es sei darauf hingewiesen, dass" | Beamtendeutsch passive |
| "Es ist erwähnenswert, dass" | Calque + AI template |
| "Es gilt zu beachten, dass" | Templated |
| "An dieser Stelle sei erwähnt, dass" | Schoolbook |
| "Wie wir alle wissen" | Phantom consensus |
| "Ohne jeden Zweifel," | Empty certainty assertion |
| "Zusammenfassend," | Conclusion template |
| "Letztendlich," | Calque of "ultimately" |
| "Letztlich," | Calque of "ultimately" |
<!-- human-writer:ignore-end -->

### Worked example (DE)

Bad:

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Es ist wichtig zu beachten, dass der Actor mindestens 1 GB Speicher benötigt. Es sei darauf hingewiesen, dass die Leistung mit Paketen sinkt. Es gilt zu beachten, dass sich das Schema ändern kann.
<!-- human-writer:ignore-end -->

Good (Variante 1, direkt):

> Der Actor braucht mindestens 1 GB. Die Leistung sinkt mit Paketen (in der v2 behoben). Das Schema kann sich ändern.

Good (Variante 2, Qualifizierer inline):

> Der Job braucht mindestens 1 GB, unverhandelbar im Gratistarif. Bei Paket-SKUs läuft er 40 % langsamer; ein bekanntes Problem für die v2. Das Schema ändert sich in den nächsten zwei Quartalen.

Same content. Half the words. Sounds like someone actually wrote it.

### How to apply

**WRITE mode.** Never start a sentence with a hedging opener. If you catch yourself typing "Es ist wichtig zu beachten", delete and rewrite.

**CLEAN mode.** Grep the draft for every entry in the forbidden list. Delete each occurrence and rewrite the remaining sentence. This is the single highest-ROI CLEAN-mode operation.

---

## 6. Use idiosyncratic markers

**Definition.** Deliberately build 1–2 recurring tics per piece (a favored conjunction, a pet phrase, a quirky structural pattern) that the analyzer cannot fingerprint but that human readers attribute to authorial personality.

**Why it works.** Human writers have tics. AI prose is *too clean*. It avoids signature moves because LLMs are trained to produce average-of-corpus output. A deliberate tic registers as personality.

One tic per ~500 words is invisible to the analyzer (which thresholds on density) but registers to readers. Two tics per 200 words is noise.

### DE-specific tics

| Tic | Cadence | Use case |
|---|---|---|
| "Sieh mal," as sentence pivot | 1 per ~1000 words | Pivot to a strong claim |
| "Naja," as paragraph opener | 1 per ~800 words | Casual register |
| "Also," as connector | 1 per ~400 words | Spoken register |
| "Ehrlich gesagt," as opener | 1 per ~600 words | First-person take |
| "Und fertig." / "Punkt." as fragment | 1 per ~600 words | Hard stop after a claim |
| Sentences ending with ", schon klar." | 1 per ~1000 words | Very casual register; informal only |

### Worked example (DE)

Ohne Tic (sauber, KI-Geschmack):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Das Pricing-Tool läuft alle 15 Minuten. Es überwacht standardmäßig 50 Artikel. Die Ergebnisse landen in einer Postgres-Sicht, die das Team über Metabase abfragt.
<!-- human-writer:ignore-end -->

Mit "Sieh mal," und "also":

> Das Tool läuft alle 15 Minuten. Sieh mal, wir haben es mit 5 versucht und die Rate-Limits haben uns umgebracht – 15 ist die Untergrenze. Es überwacht standardmäßig 50 Artikel. Die Ergebnisse landen also in einer Postgres-Sicht, die das Team sowieso über Metabase abfragt.

The two tics are invisible to bot detection but read as a person with a voice.

### How to apply

**WRITE mode.** Before drafting, pick one or two tics. Use them at the cadence listed. Resist adding more.

**CLEAN mode.** If the piece is otherwise good but reads as bot-clean, inject *one* tic at *one* natural insertion point. Re-run the analyzer.

---

## 7. Inject digressions and parentheticals

**Definition.** Humans wander. AI stays on-track relentlessly. Insert one short digression per ~500 words. Use parentheses or the native en-dash "–" for genuine asides.

**Why it works.** LLMs are trained to follow the prompt without drift. The result is unnaturally focused prose: every paragraph stays inside its topical lane. Human writers tangent constantly. The drift is the signal of authentic thought. The key constraint: the digression must *return* to the main thread.

### Worked example (DE)

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
Ohne Abschweifung:

> Wein-Pricing ist volatil. Die Erzeuger passen sich an, indem sie die Marktsignale beobachten.
<!-- human-writer:ignore-end -->

Mit Abschweifung:

> Wein-Pricing ist volatil (der Burgunder 2020 fiel in drei Wochen um 11 %). Die Erzeuger passen sich an – zumindest die, die jede Woche auf die Marktsignale schauen.

### Worked example (DE) (longer)

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
Ohne Abschweifung:

> Scraping braucht gute Infrastruktur. Proxys, Fingerprint und Rate-Limits – alles muss abgestimmt sein, sonst sperrt dich das Ziel in unter einer Stunde.
<!-- human-writer:ignore-end -->

Mit Abschweifung:

> Scraping braucht gute Infrastruktur. Proxys, Fingerprint, Rate-Limits – einer falsch und das Ziel sperrt dich in unter einer Stunde. (Wir haben das bei einem Job im März 2024 schmerzhaft gelernt: tadellose Residential-Proxys, tadelloser Fingerprint, aber wir vergaßen, den Sleep zu randomisieren. Nach 47 Minuten gesperrt.) Naja, Gürtel und Hosenträger auf jeder Ebene.

### How to apply

**WRITE mode.** Plan one digression per major section (every ~500 words). Mark insertion points in your outline.

**CLEAN mode.** Read each paragraph and ask: "Did the writer think of anything specific while writing this?" If every paragraph is locked to its topic, inject one parenthetical with a real specific fact.

---

## 8. Choose concrete over abstract

**Definition.** When given the choice between a generic noun and a specific one, always pick the specific. AI defaults to abstractions ("Lösungen", "Unternehmen", "Workflows"); humans default to concrete examples ("die Excel-Tabelle mit 14 Reitern", "das Einkaufsteam unseres Weinkunden", "die Montagmorgen-Vorbereitung").

**Why it works.** AI prose lives in the abstraction layer because abstractions are safer. Concrete nouns ("Postgres", "Amazon", "12 Minuten") commit to facts that must be true. Their presence is a strong signal of first-hand writing.

### Abstract → concrete substitution table (DE)

<!-- human-writer:ignore-start (citation table: abstract AI nouns quoted, not used) -->
| Abstract (AI default) | Concrete (human alternative) |
|---|---|
| "die Unternehmen" | "unser Weinkunde" / "ein 12-köpfiges Einkaufsteam" |
| "Lösungen" | das konkrete Ding: "das Dashboard", "der Cronjob", "der CSV-Export" |
| "Workflows" | der konkrete Schritt: "die Montags-Vorbereitung", "das Import-Skript" |
| "die Nutzer" | "der Einkäufer bei Edeka" / "der Analyst am Trading-Desk" |
| "die Daten" | "47 Spalten: SKU + Preis + Konkurrent + Zeitstempel" |
| "die Leistung" | "47 Req/s auf einer 2-GB-Instanz" |
| "die Skalierbarkeit" | "wir liefen gegen 2,3 Mio. URLs in 9 Stunden" |
| "wertvolle Erkenntnisse" | "die konkrete Zahl, die du vorher nicht hattest" |
| "die Beteiligten" | nenn sie: "der CFO", "das Einkaufsteam" |
| "das Ökosystem" | nenn es: "die npm-Registry", "die Postgres-Extensions" |
<!-- human-writer:ignore-end -->

### Worked example (DE)

Bad (abstrakt):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Unsere Lösung hilft Unternehmen, ihre Prozesse zu optimieren.
<!-- human-writer:ignore-end -->

Good (konkret):

> Wir haben die 14-Reiter-Excel-Tabelle, die unser Kunde für sein Pricing nutzte, durch ein einziges Dashboard ersetzt. Die Montagmorgen-Vorbereitung schrumpfte von 2 Stunden auf 12 Minuten.

### Worked example (DE) (longer)

Bad:

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Die Plattform ermöglicht es Organisationen, Echtzeitdaten zu nutzen, um bessere Entscheidungen zu treffen.
<!-- human-writer:ignore-end -->

Good:

> Die Plattform schiebt die Preisänderungen von 8 Marktplätzen nach Slack, Kanal für Kanal und Region für Region. Wenn der Rioja 2018 um mehr als 5 % fällt, weiß es der Trading-Desk in 90 Sekunden. Sie schlossen letztes Quartal drei Einkaufsorders mit Signalen ab, die das Tool vor den Mails des Maklers lieferte.

### How to apply

**WRITE mode.** Each time you reach for an abstract noun ("Lösung", "Nutzer", "Daten"), pause: "What's the concrete version?" Write that.

**CLEAN mode.** Grep the draft for the abstract nouns in the table above. Replace each with a concrete equivalent or rewrite the sentence around it.

---

## 9. Vary transitions, drop the formal connectors

**Definition.** AI-generated transitions are predictable and connector-heavy. Humans transition with simple conjunctions, restructure sentences, or skip transitions entirely.

**Why it works.** The connector-class AI tells are the formal-register conjunctions below. They appear when an LLM tries to make logical structure visible at the surface, which humans rarely do.

### Forbidden transitions (DE)

<!-- human-writer:ignore-start (citation list: tells quoted, not used) -->
- Zudem (as paragraph opener)
- Ferner
- Des Weiteren
- Darüber hinaus
- Folglich
- Demzufolge
- Infolgedessen (as paragraph opener)
- Nichtsdestotrotz (when overused)
- Andererseits (as paragraph opener)
- In diesem Sinne (as paragraph opener)
<!-- human-writer:ignore-end -->

### DE alternatives

- **"Und"**: yes, start a sentence with "und". German allows it.
- **"Aber"**: sharp pivot.
- **"Obwohl"**: informal contrast.
- **"Die Sache ist die:"**: register varies but this is human.
- **"Naja,"** / **"Also,"**: German rhythm markers.
- **"Sonst,"**: branching alternative.
- **Restructure**: often the cleanest transition is no transition — rewrite the next sentence to flow without a connector.

### Worked example (DE)

Bad (connector-überladen):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> Der Actor holt die Preise von Amazon. Zudem überwacht er die Lagerbestände. Des Weiteren integriert er sich mit Slack. Darüber hinaus unterstützt er tägliche Exporte.
<!-- human-writer:ignore-end -->

Good (variiert):

> Der Actor holt die Preise von Amazon. Er überwacht auch den Lagerbestand. Die Slack-Integration kommt in der v2. Tägliche Exporte – oder stündliche, wenn du willst.

### How to apply

**WRITE mode.** Never use the forbidden list. At a transition point, pick from the human alternatives or restructure.

**CLEAN mode.** Grep for every forbidden transition. Delete and rewrite. One of the fastest, highest-impact CLEAN-mode operations.

---

## 10. Build in productive imperfection

**Definition.** Humans pause, repeat for emphasis, change midstream, use casual contractions and oral elisions. AI hyper-corrects. A light imperfection ratio (1–2 instances per 500 words) registers as human without seeming sloppy.

**Why it works.** LLMs are trained out of the small imperfections real writing carries. Self-corrections, repetitions for emphasis, and casual interjections are statistically scarce in AI output and common in human prose. A small dose flips the signal.

### Imperfection categories (DE)

| Category | Example | Cadence |
|---|---|---|
| **Oral elisions / contractions** | "geht's", "hab's", "is'" (very informal only) | Extreme cases — informal marketing only, never technical |
| **Repetition for emphasis** | "Das ist gut. Sehr gut." | 1 per ~500 words |
| **Self-correction** | "Der Preis bewegte sich um 11 % – naja, eher 12 %, wenn man die Gebühren mitzählt." | 1 per ~700 words |
| **Casual interjections** | "Ehrlich gesagt,", "Sieh mal,", "Hör mal,", "Naja," | 1 per ~400 words (overlaps with #6) |
| **Mid-sentence pivot** | "Wir probierten den billigsten Proxy, aber – na ja, du kannst es dir denken." | 1 per ~800 words |
| **Trailing "und so"** | "… oder was auch immer in deinem Stack, und so." | 1 per ~1000 words (informal only) |

### Worked example (DE)

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
Ohne Imperfektion (KI-hyperkorrekt):

> Der Preis bewegte sich in drei Wochen um 11 %. Dies ist signifikant. Es empfiehlt sich, die zugrunde liegende Ursache zu untersuchen.
<!-- human-writer:ignore-end -->

Mit Imperfektion (Selbstkorrektur + Interjektion):

> Der Preis bewegte sich in drei Wochen um 11 % – naja, eher 12 %, wenn man die Gebühren mitzählt. Das ist viel. Ehrlich gesagt, das sollten wir uns ansehen.

### Calibration warning

Imperfection is dosed. Too much and you cross from "human writer" to "sloppy draft". The cadences above are upper bounds. In a technical doc, lean low. In casual marketing copy, the upper end works.

### How to apply

**WRITE mode.** Use natural elisions where the register allows. Add one self-correction or casual interjection per ~500 words.

**CLEAN mode.** Identify one paragraph that reads as too-polished and inject a single self-correction.

---

## Bonus: how to combine techniques

The techniques compound. Applying #1 alone drops `sentence_length_stdev` flags. Applying #1 + #5 + #9 drops the three most common AI signatures simultaneously. Below is a worked example showing the cumulative effect on a German paragraph.

### Starting paragraph (vanilla AI output, ~90 words)

<!-- human-writer:ignore-start (citation: deliberately AI) -->
> In der heutigen schnelllebigen Welt des E-Commerce ist die Überwachung der Konkurrenzpreise von entscheidender Bedeutung. Unser Pricing-Intelligence-Tool bietet eine nahtlose, robuste und intuitive Lösung. Es geht nicht nur um die Verfolgung von Preisen, sondern darum, Ihr Team mit verwertbaren Erkenntnissen zu befähigen. Egal, ob Sie ein Start-up oder ein Großunternehmen sind, unsere Plattform hilft Ihnen, die Komplexität des dynamischen Pricings zu navigieren. Zudem integriert sie sich nahtlos in Ihre Workflows. Letztendlich können Sie datenbasierte Entscheidungen treffen und der Konkurrenz voraus sein.
<!-- human-writer:ignore-end -->

**Analyzer baseline:** suspect vocab ~7 (`in der heutigen schnelllebigen Welt`, `entscheidend`, `nahtlos`/`nahtlose`, `robuste`, `intuitive`, `befähigen`, `verwertbar`), tricolon 1 ("nahtlose, robuste und intuitive"), constructions "es geht nicht nur um … sondern", "egal, ob Sie … oder", conclusion "letztendlich", connector "zudem". Estimated AI-probability: HIGH/CRITICAL.

### Final humanized version (same content, ~90 words)

> Die Preise ändern sich auf Amazon im Weihnachtsgeschäft jede Stunde. Sieh mal – unser Tool läuft alle 15 Minuten gegen deine 50 teuersten SKUs und markiert jede Bewegung über 3 %. Und fertig. Das Dashboard ist eine Postgres-Sicht (ohne schicke UI), weil das Team, das sie braucht, sowieso in Metabase lebt. Wir haben es 2024 für einen Weinimporteur gebaut, nachdem man ihm sechs Wochen lang den Preis gedrückt hatte, bevor er es merkte. Willst du dasselbe Setup? Den Code findest du im Repo.

What changed: temporal abstraction → concrete season + marketplace; tricolon and suspect vocab gone; "es geht nicht nur um …" and "egal, ob Sie …" gone; "letztendlich" gone; "zudem" gone; added a "Sieh mal –" tic (with native en-dash), a "Und fertig." fragment, a real 2024 anecdote, concrete numbers (15 Min., 50 SKUs, 3 %), and a closing pointer.

### Order of operations summary

| Order | Technique | Reason |
|---|---|---|
| 1 | #5 Cut hedging openers | Highest words-saved, fastest fix |
| 2 | #9 Vary transitions | Cuts the connector class in one pass |
| 3 | #1 Vary sentence length | Statistical signal most analyzers test |
| 4 | #4 Break tricolons | Density signal — easy to target |
| 5 | #3 Asymmetric bullets | Only if the piece has lists |
| 6 | #8 Concrete over abstract | Compounds with #2 |
| 7 | #2 Inject opinion/anecdote | Highest authorial-signal move |
| 8 | #7 Digressions | Adds drift |
| 9 | #6 Idiosyncratic markers | Final personality layer |
| 10 | #10 Imperfection | Final humanity layer |

The first five fix the statistical signature. The last five inject the authorial signal.

---

## See also

- `tells-stylistic-de.md` — German vocabulary and construction lists that techniques #5, #8, #9 reference directly.
- `tells-statistical.md` — the metrics (sentence-length stdev, bullet parallelism, tricolon density, em-dash density) these techniques target.
- `tells-structural.md` — bullet, header, conclusion anti-patterns that techniques #3 and #4 disrupt (plus the German noun-capitalization note).
- `adapter-marketing.md` / `adapter-short-comms.md` / `adapter-technical.md` / `adapter-editorial-seo.md` — content-type-specific calibration.
