# Stylistic Tells — German

> Doctrine for the `human-writer-de` skill. Covers DE-specific stylistic patterns (suspect vocabulary, AI constructions, typography incl. the foreign em-dash vs the native en-dash, straight quotes, ß/ss). Doctrine is written in English; examples are in German because that's the surface the analyzer matches against.

German ChatGPT output, like French and Spanish, runs *louder* than its English counterpart, for two reasons:

1. The model leans on a neutral-formal register learned from journalism (*FAZ*, *Süddeutsche*, *Die Zeit*), academic prose, and corporate copy. That register is naturally inflated — "in der heutigen Zeit", "es ist wichtig zu beachten", "im digitalen Zeitalter" — and it reads as marketing boilerplate the moment it lands on anything concrete.
2. English formulas leak through as calques: `leverage` → "nutzen", `seamless` → "nahtlos", `robust` → "robust", `cutting-edge` → "hochmodern", `holistic` → "ganzheitlich", `dive into` → "tauchen Sie ein". A native ear catches these immediately; the model never does.

This file codifies which words, phrases, openers, closers, and typographic choices are tells in German, with severity and rewrite.

**German has one typographic feature the analyzer treats with extreme care — and it is the single biggest false-positive trap in the whole skill.** It is documented as item #1 below, before anything else, because getting it wrong invalidates the detector.

---

## #1 FALSE-POSITIVE TRAP — the spaced en-dash "–" is NATIVE German (never flag it)

German has TWO dashes that look similar but are completely different signals:

| Char | Unicode | Name | Status in German | Detector behavior |
|---|---|---|---|---|
| `–` | U+2013 | **Halbgeviertstrich** (spaced en-dash) | **NATIVE, CORRECT** — the standard German parenthetical/aside dash, and the range dash ("Seiten 12–14") | **NEVER counted, NEVER flagged** |
| `—` | U+2014 | Geviertstrich (em-dash) | **FOREIGN** — Anglo-American typography; not used in native German prose | counted by `detect_em_dashes`; the actual tell |

Native German uses the spaced en-dash `–` exactly where English uses a spaced em-dash:

- Native German: "Die Migration – wir hatten sie aufgeschoben – brachte weniger Probleme als gedacht."
- AI / Anglo habit: "Die Migration — wir hatten sie aufgeschoben — brachte weniger Probleme als gedacht."

The first is **correct, idiomatic, human German** and MUST score zero em-dashes. The second uses the foreign `—` and is the tell. `detect_em_dashes` counts ONLY `—` (U+2014); it is deliberately blind to `–` (U+2013), so native German prose containing en-dash asides never triggers the em-dash detector.

**Cleanup doctrine.** When cleaning AI German that uses `—`, the correct fix is to *replace it with the native `–`* (or a comma / colon / parentheses), NOT to delete the dash structure. The en-dash aside is good German; only the character is wrong.

---

## Suspect vocabulary (DE)

The list is calibrated against typical ChatGPT/Gemini German output across marketing, editorial, and corporate copy.

**Hard rule.** Never use 2+ tier-1 items in the same paragraph. Cap any single tier-1 item at 1 per 500 words. Treat tier-1 items the way you would treat "delve" or "tapestry" in English: even one is suspicious; two is a giveaway.

**Soft rule.** Tier-2 items are legitimate in their domain. Track frequency: 3+ tier-2 items in a 300-word paragraph = the paragraph reads as AI-generated. Cap any single tier-2 item at 2 per 500 words.

**Pure-frequency rule.** Tier-3 items are individually invisible but flag the prose when 5+ co-occur in a single paragraph. The analyzer does NOT list tier-3 (false-positive risk on common German words is too high for a per-word matcher); the human reviewer does.

### Tier 1 — High signal (always avoid)

These are the German equivalents of English `delve`, `tapestry`, `realm`. Each line: the phrase, why it's a tell, a human alternative.

#### Meta-comments and hedging openers

- **"es ist wichtig zu beachten"** / **"es ist wichtig zu erwähnen"** / **"es ist wichtig festzuhalten"** — Why: meta-textual hedge, a direct calque of "it's important to note". The single most frequent DE AI topic-sentence opener of 2025-2026. Alternative: drop the prefix, state the claim. "Es ist wichtig zu beachten, dass der Umsatz stieg" → "Der Umsatz stieg um 12 %."
- **"es sei darauf hingewiesen"** / **"es sei angemerkt"** / **"es sei betont"** — Why: pompous *Beamtendeutsch* passive ("let it be noted that"). Alternative: state the fact inline.
- **"es ist erwähnenswert"** — Calque of "it's worth mentioning". Alternative: just mention it.
- **"an dieser Stelle sei erwähnt"** / **"es gilt zu beachten"** / **"es gilt festzuhalten"** — Same family, schoolbook register. Alternative: drop.
- **"wie wir alle wissen"** / **"wie allgemein bekannt"** / **"bekanntlich"** — Why: appeal to phantom consensus no native writer uses unless ironically. Alternative: drop.
- **"in diesem Sinne"** / **"in diesem Zusammenhang"** / **"in diesem Kontext"** / **"vor diesem Hintergrund"** — Why: meta-textual signposts AI uses to glue paragraphs. Alternative: drop; the next sentence carries the logic.
- **"darüber hinaus"** / **"des Weiteren"** (as paragraph openers) — Schoolbook connectors. Alternative: "und", "auch", restructure.
- **"andererseits"** / **"auf der anderen Seite"** (overused as connectors) — Alternative: "aber", restructure.

#### Adverbs and phrases of certainty

- **"zweifellos"** / **"zweifelsohne"** / **"ohne Zweifel"** / **"ohne jeden Zweifel"** — Why: heavy "without a doubt" assertions AI sprinkles on any claim. Alternative: drop, or back the claim with a number.
- **"fraglos"** / **"unbestreitbar"** / **"unbestritten"** — Same family. Alternative: drop.
- **"selbstverständlich"** / **"natürlich"** / **"offensichtlich"** (as openers) — Why: AI's "obviously" reflex. Alternative: drop; if it's obvious, don't say it.
- **"in der Tat"** / **"tatsächlich"** (as intensifiers) — Calque of "indeed". Alternative: drop.

#### Spatial-temporal abstractions

- **"in der heutigen Zeit"** / **"in der heutigen schnelllebigen Welt"** / **"in der heutigen digitalen Welt"** — Why: meta-frame opener with no date, event, or actor. The classic AI German cold-open. Alternative: name the year, the event. "In der heutigen schnelllebigen Welt der Daten…" → "Seit der DSGVO 2018…"
- **"im digitalen Zeitalter"** / **"im Zeitalter der KI"** / **"im Zeitalter der Digitalisierung"** / **"im Informationszeitalter"** — Same family. Alternative: a date, a fact.
- **"in einer Welt, in der"** / **"in der modernen Welt"** — Lyrical opener. Alternative: name the actual situation.
- **"heutzutage"** (as scene-setter) — Vague time-stamp. Alternative: a concrete date.
- **"im Kern von"** / **"im Herzen von"** / **"im Zentrum von"** (when not literal) — Calque of "at the heart of". Alternative: "in", "bei", or drop.

#### Comparators and frames

- **"im Lichte von"** / **"vor dem Hintergrund von"** — Calque of "in the light of". Alternative: "angesichts", "nach", or drop.
- **"im Hinblick auf"** / **"in Anbetracht"** — Pompous "with regard to". Alternative: "für", "bei".
- **"wenn es darum geht"** (as filler before an infinitive) — Why: AI's go-to padding ("when it comes to choosing"). Alternative: "beim Auswählen", "wenn du wählst".

#### Metaphorical inflators

- **"nahtlos"** / **"reibungslos"** / **"mühelos"** — Calque of "seamless / frictionless / effortless". The canonical DE rendering of "seamless" and a heavy tell. Alternative: "direkt", "ohne Umwege", "ohne Bruch", or drop.
- **"ganzheitlich"** — Calque of "holistic". Outside genuine medicine/pedagogy, empty. Alternative: "vollständig", "umfassend" (sparingly), or name the scope.
- **"Synergie"** / **"Synergien"** / **"synergetisch"** — Buzzword. Alternative: name the relationship.
- **"bahnbrechend"** / **"wegweisend"** / **"richtungsweisend"** — Why: AI's "groundbreaking / trailblazing" reflex. Alternative: "neu", and show why.
- **"hochmodern"** / **"modernste"** / **"zukunftsweisend"** — Calque of "cutting-edge / state-of-the-art". Alternative: "neu", "aktuell" (with a date).
- **"revolutionär"** / **"revolutionieren"** (outside literal politics) — Calque of "revolutionary". Alternative: "neu", "verändert die Regeln" (sparingly).
- **"transformativ"** / **"disruptiv"** — Calques of "transformative / disruptive", barely idiomatic. Alternative: "entscheidend", "wichtig".
- **"robust"** (non-technical) — Calque of "robust". Alternative: "solide", "zuverlässig", "hält stand".
- **"skalierbar"** (in marketing) — state the capacity number instead.
- **"leistungsstark"** / **"mächtig"** (marketing) — Alternative: a benchmark or number.
- **"erstklassig"** / **"branchenführend"** / **"weltklasse"** — Calque of "best-in-class / world-class / industry-leading". Alternative: cite a benchmark or a number.
- **"unverzichtbar"** — Marketing cliché now associated with AI prose. Alternative: "nötig" (sparingly), or name the reason.

#### Structural metaphors

- **"Eckpfeiler"** / **"Grundpfeiler"** / **"Grundstein"** / **"Rückgrat"** / **"Herzstück"** / **"Dreh- und Angelpunkt"** — Why: AI scatters heroic nouns as if every concept needs one. Alternative: name the function ("X hängt von Y ab").

#### Verbs and verbal phrases

- **"nutzen"** (when it means "leverage", e.g. "das Potenzial nutzen") — Calque of "leverage". Alternative: "verwenden", "einsetzen", "sich zunutze machen" (sparingly).
- **"voll ausschöpfen"** / **"das Potenzial freisetzen"** / **"das volle Potenzial"** / **"freischalten"** — Calque of "unlock the full potential". Alternative: "nutzen" (plainly), "verwenden".
- **"befähigen"** / **"ermöglichen"** (metaphorical) — Why: AI's "empower / enable" reflex. Alternative: "lässt dich …", name the mechanism.
- **"vorantreiben"** / **"beschleunigen"** (overused: "das Wachstum vorantreiben") — Alternative: "wachsen lassen", name the action.
- **"katalysieren"** — Calque of "catalyze". Alternative: "löst aus", "beschleunigt".
- **"verkörpern"** — Why: AI's "embodies" reflex. Alternative: "ist", "steht für".
- **"schmieden"** / **"etablieren"** — Heavy verbs where "bauen", "aufbauen", "einführen" would do.
- **"tauchen Sie ein"** / **"tauchen wir ein"** / **"lassen Sie uns eintauchen"** — Why: AI's "dive into / immerse" opener. Banned as an introduction. Alternative: state the subject directly.
- **"lassen Sie uns erkunden"** / **"erkunden wir"** — Calque of "let's explore". Alternative: "schauen wir uns … an", "hier".

### Tier 2 — Medium signal (contextual)

Legitimate in domain (e.g. "optimieren" in an SRE post is fine). Flag when 3+ co-occur in 300 words.

#### Verbs

- **"erkunden"** / **"entdecken"** (metaphorical) — Alt: "ansehen", "finden".
- **"enthüllen"** / **"offenbaren"** — Marketing reveal verbs. Alt: "zeigen", "vorstellen".
- **"transformieren"** (overused) — Alt: "ändern", "verbessern".
- **"optimieren"** / **"rationalisieren"** / **"vereinfachen"** — Domain-fine, AI filler otherwise. Alt: name the concrete change.
- **"fördern"** / **"begünstigen"** — Hedge-y "helps to" verbs. Alt: "erleichtert", verb of the actual mechanism.
- **"gewährleisten"** / **"sicherstellen"** (overused) — Alt: "sorgt für", or state the condition.
- **"maximieren"** / **"minimieren"** — Corporate filler. Alt: "erhöhen", "senken", "reduzieren".
- **"begleiten"** (B2B "we accompany you") — Alt: "hilft beim" + noun.
- **"adressieren"** (calque of "to address an issue") — Alt: "angehen", "lösen", "behandeln".
- **"verwalten"** (overused catch-all) — Alt: name the action.
- **"implementieren"** — Calque-tinged in non-technical copy. Alt: "umsetzen", "einbauen".
- **"überwachen"** / **"integrieren"** — Domain-fine; AI filler otherwise.

#### Adjectives of importance / intensity

- **"entscheidend"** / **"wesentlich"** / **"grundlegend"** / **"unerlässlich"** / **"maßgeblich"** / **"zentral"** — All inflators. AI sprays them. Alt: drop, or quantify.
- **"bedeutend"** / **"bemerkenswert"** / **"relevant"** / **"signifikant"** — Empty modifiers. Alt: the number.
- **"außergewöhnlich"** / **"beeindruckend"** / **"spektakulär"** — AI's intensifier toolkit. Alt: name the specific quality.
- **"einzigartig"** (as "ein einzigartiges Erlebnis") — Alt: name what's actually different.
- **"innovativ"** — Empty self-praise. Alt: "neu", and show why.
- **"elegant"** / **"ausgefeilt"** / **"intuitiv"** / **"benutzerfreundlich"** — Product-copy cliché. Alt: name the design choice.
- **"umfassend"** / **"vielfältig"** — Inflators. Alt: a number, "viele".

#### Abstract nouns

- **"Ökosystem"** (metaphorical, outside biology) — Calque of "ecosystem". Alt: "Markt", "Netzwerk", "Umfeld".
- **"Universum"** (metaphorical: "das Wein-Universum") — Alt: "die Welt des Weins", "die Branche", or drop.
- **"Paradigma"** / **"Paradigmenwechsel"** — Academic filler outside Kuhn. Alt: "Modell", "Ansatz".
- **"Erlebnis"** / **"Erfahrung"** (vague: "ein unvergessliches Erlebnis") — Alt: name what the user actually does.
- **"Lösung"** / **"Lösungen"** (vague, "unsere Lösung") — Alt: the actual thing: "das Dashboard", "der Cronjob", "der CSV-Export".
- **"Reise"** (metaphorical: "deine Reise mit uns") — UX/tourism cliché. Alt: name the steps.
- **"Mehrwert"** / **"Spielraum"** — Corporate filler.
- **"eine Vielzahl von"** / **"eine breite Palette von"** / **"eine Fülle von"** / **"ein breites Spektrum"** — Alt: a number, "viele".

#### Connectors (overuse)

- **"zudem"** / **"ferner"** / **"ebenso"** — Schoolbook. Alt: "und", "auch".
- **"folglich"** / **"demzufolge"** / **"infolgedessen"** / **"somit"** (heavy) — Alt: "also", "deshalb".
- **"nichtsdestotrotz"** / **"dennoch"** / **"jedoch"** (every paragraph) — Alt: "aber", restructure.

### Tier 3 — Low signal (frequency-only)

Individually fine. Flag a paragraph if 5+ co-occur. (The analyzer does not list these; the reviewer does.)

Ansatz, Konzept, Strategie, Herausforderung, Chance, Perspektive, Vision, Ambition, Leistung, Effizienz, Rentabilität, Wachstum, Begleitung, Nutzen, Vorteil, Stärke, Ressource, Potenzial, Dynamik, Hebel, Achse, Säule, Dimension, Facette, Aspekt, Element, Komponente, Bereich, Rahmen, Governance, Ausrichtung, Kohärenz, Transparenz, Skalierbarkeit, Modularität, Interoperabilität, Nachhaltigkeit, Wirkung, Nachvollziehbarkeit, Beobachtbarkeit, Automatisierung, Industrialisierung.

**Why low signal**: each appears naturally in honest German B2B prose. **Why they still matter**: a paragraph with 5+ of them is corporate AI Esperanto.

### Replacements (consolidated)

| Tell | Human alternative |
|---|---|
| es ist wichtig zu beachten | drop and state the claim |
| es sei darauf hingewiesen | drop; state the fact inline |
| in der heutigen Zeit / schnelllebigen Welt | heute / seit [Datum] / hier |
| im digitalen Zeitalter | heute / seit [konkretes Datum] |
| tauchen Sie ein / tauchen wir ein | schauen wir uns … an / hier |
| lassen Sie uns erkunden | sehen wir uns … an |
| das Potenzial freisetzen | nutzen / verwenden |
| nahtlos / reibungslos | direkt / ohne Bruch / ohne Umwege |
| ganzheitlich | vollständig / umfassend (sparingly) |
| robust (nicht technisch) | solide / zuverlässig / hält stand |
| transformativ | wichtig / entscheidend |
| hochmodern / modernste | neu / aktuell (with a date) |
| bahnbrechend / wegweisend | neu, und zeig warum |
| nutzen (leverage) | verwenden / einsetzen / sich zunutze machen |
| befähigen / ermöglichen | lässt dich … / name the mechanism |
| Eckpfeiler / Rückgrat / Herzstück | name the function literally |
| Ökosystem (metaphorical) | Markt / Netzwerk / Umfeld |
| Universum des X | die Welt des X / die Branche |
| Synergie | name the relationship |
| eine Vielzahl von / ein breites Spektrum | eine Handvoll / mehrere / [Zahl] |
| zudem / ferner | und / auch |
| folglich / somit | also / deshalb |

---

## AI constructions (DE)

Patterns the analyzer matches via regex. Severity drives scoring.

### High severity

#### "Es geht nicht nur um X, sondern um Y"

Calque of "It's not just X, it's Y". Never use. Replace with a concrete claim.

- Vermeiden: "Es geht nicht nur um ein Pricing-Tool, sondern um einen strategischen Vorteil."
- Bevorzugen: "Das Tool zeigt die Marge je Artikel — dieselben Zahlen, die der Einkäufer sieht."

#### "Tauchen Sie ein in …" / "Tauchen wir ein in …" / "Lassen Sie uns gemeinsam …"

Never use as an introduction. State the subject directly.

- Vermeiden: "Tauchen Sie ein in die faszinierende Welt des dynamischen Pricings."
- Bevorzugen: "Dynamisches Pricing — fangen wir bei den Margenschwellen an."

#### "In der heutigen Zeit …" / "Im digitalen Zeitalter …" / "Im Zeitalter der KI …"

Never open with a temporal abstraction. Use a date, an event, or jump straight in.

- Vermeiden: "In der heutigen schnelllebigen Welt, in der Daten das neue Öl sind …"
- Bevorzugen: "Seit der DSGVO kostet der Export deiner Kundendatenbank mehr."

#### "Stellen Sie sich eine Welt vor …" / "Was wäre, wenn ich Ihnen sagen würde …?"

Conference-bro openers. Banned.

- Vermeiden: "Stellen Sie sich eine Welt vor, in der Ihre Kunden vor dem Kauf zahlen."
- Bevorzugen: "Vorkasse gibt es längst — Wine.com macht das seit 2003."

#### "Es ist wichtig zu beachten …" / "Es sei darauf hingewiesen …" / "Ohne jeden Zweifel …"

Cf. suspect vocab. Pattern-matched separately because they act as topic-sentence openers.

- Vermeiden: "Es ist wichtig zu beachten, dass die Migration die Leistung verbesserte."
- Bevorzugen: "Die Migration senkte die Latenz um 40 %."

### Medium severity

#### "Egal, ob Sie X oder Y …" / "Ganz gleich, ob Sie X oder Y …"

Avoid unless the branching is real. By default, pick one reader and write for them.

- Standardmäßig verboten: "Egal, ob Sie ein Start-up oder ein Großunternehmen sind …"
- Akzeptabel wenn echt: "Ob Sie in EUR oder in USD fakturieren, der Export ist identisch."

#### "Schnallen Sie sich an …"

AI signal-flares to the reader. Cut them; the sentence after is the actual content.

#### "Lassen Sie uns das aufschlüsseln." / "Lassen Sie es mich erklären." / "Kommen wir zur Sache."

Meta-sentences that perform thinking instead of doing it. Skip to the decomposition.

#### "Kurz gesagt," (as a closer)

Announces a summary. Either summarize, or don't — don't announce.

#### "Sie fragen sich vielleicht …" / "Vielleicht fragen Sie sich …"

AI's "you might be wondering" reflex. Bad. Just answer the implied question.

#### "Das Urteil ist eindeutig:" / "Die Schlussfolgerung liegt auf der Hand:"

AI's "the verdict is clear" reflex. Drop.

#### "Abschließend lässt sich sagen" / "Zusammenfassend lässt sich sagen" / "Im Großen und Ganzen"

Conclusion frames. See below.

### Low / conclusion severity

#### Conclusion openers: "Zusammenfassend," / "Letztendlich," / "Letztlich," / "Fazit:"

Conclusion templates. **"Letztendlich" and "letztlich" are the strongest DE closing tells** (direct calques of "ultimately"), and **"Zusammenfassend"** is the canonical AI summary opener. Cap all at 0 as paragraph openers.

### Hedging openers (drop entirely)

- "Es ist wichtig zu beachten, dass"
- "Es sei darauf hingewiesen, dass"
- "Es sei angemerkt, dass"
- "Es gilt zu beachten, dass"
- "An dieser Stelle sei erwähnt, dass"

If the qualifier matters, state it inline. Example: "Das Pricing hängt vom Volumen ab (wichtig: ohne MwSt.)."

---

## Em-dash discipline — and the en-dash that is NOT a tell

This is the German-critical section. Re-read item #1 above first.

**Ban the em-dash "—" (U+2014) entirely. Target 0; hard cap ≤ 1 per 1000 words.** The wide "—" is foreign to German typography; native German never uses it. Its appearance in German prose is a pure AI/Anglo-import tell, because the model carries the English emphasis-dash habit across.

**Do NOT touch the spaced en-dash "–" (U+2013).** It is the correct native German parenthetical and range dash. It is good writing. `detect_em_dashes` ignores it entirely.

### Why German is stricter than English on "—"

- Native German expository prose uses the **spaced en-dash `–`** for asides ("Die Idee – endlich – funktioniert."), commas for short asides, colons for setups, and parentheses for true asides.
- The em-dash `—` simply does not belong to German orthography (Duden uses the Halbgeviertstrich `–`). Seeing `—` in German prose is like seeing American date formatting in a German letter: a foreign fingerprint.

### Replacement table

| AI overuse (foreign "—") | Human DE |
|---|---|
| "Schnell — effektiv — einfach." | "Schnell, effektiv, einfach." or "Schnell. Effektiv. Einfach." |
| "Das Tool — für Weingüter gebaut — läuft täglich." | "Das Tool – für Weingüter gebaut – läuft täglich." (native en-dash) or "Das Tool, für Weingüter gebaut, läuft täglich." |
| "Es funktioniert — meistens." | "Es funktioniert (meistens)." |
| "Es ist einfach — und es funktioniert." | "Es ist einfach. Und es funktioniert." |
| "Drei Optionen — A, B und C — alle gültig." | "Drei Optionen: A, B und C. Alle gültig." |
| "Flexibles Pricing — Pay-per-Event." | "Flexibles Pricing: Pay-per-Event." |

The general rule: convert `—` to the native `–`, to a comma/colon/period, or to parentheses. The aside structure is fine — only the foreign character is wrong.

---

## Calques anglosächsisch zu vermeiden (KI-Anglizismen)

These English-origin patterns leak into German ChatGPT output. They're stronger tells in German than in English because they read as unnatural to a native ear.

### Lexical calques

- **"nahtlos"** (seamless) → "direkt", "ohne Bruch", "ohne Umwege". "Nahtlos" is the canonical DE rendering of "seamless" and a heavy tell.
- **"nutzen"** (leverage, when overused as the default "use") → "verwenden", "einsetzen", "sich zunutze machen"
- **"hochmodern"** / **"modernste"** (cutting-edge) → "neu", "aktuell" (with a date)
- **"robust"** (non-technical, robust) → "solide", "zuverlässig"
- **"ganzheitlich"** (holistic) → "vollständig", "umfassend"
- **"transformativ"** (transformative) → "entscheidend", "wichtig"
- **"skalierbar"** (scalable, in marketing) → state the capacity number
- **"verwertbar"** / **"umsetzbar"** (actionable) → "nützlich", "anwendbar". The "actionable insight" sense is a calque; "umsetzbar" is closer but still over-used in AI copy.
- **"adressieren"** (to address an issue) → "angehen", "behandeln", "lösen"
- **"realisieren"** (to realize / understand, calque) → German "realisieren" means *to implement/build*; for "to realize = understand" use "erkennen", "merken"
- **"in 2024"** (English "in 2024" calque) → German is **"2024"** or "im Jahr 2024", never "in 2024"
- **"einmal mehr"** (once more, calque) → "wieder", "erneut"
- **"macht Sinn"** (makes sense, calque) → "ergibt Sinn", "ist sinnvoll" (AI overuses "macht Sinn")
- **"definitiv"** (definitely, calque-toned filler) → drop, or "sicher", "auf jeden Fall"

### Calques de traducción (EN→DE)

When **translating** EN source into DE (not writing fresh), a second class of calque appears: domain terms rendered word-for-word into DE strings that either don't exist or shift meaning. They pass every "fluency" check yet read as machine output to a native professional.

| EN source | Calque (avoid) | Idiomatic DE | Note |
|---|---|---|---|
| seamless integration | "nahtlose Integration" | **"direkte Integration" / "integriert sich ohne Bruch"** | "nahtlos" is the AI fingerprint for "seamless" |
| actionable insights | "verwertbare Erkenntnisse" | **"anwendbare Erkenntnisse" / "Zahlen, mit denen du arbeiten kannst"** | "actionable" calque |
| cutting-edge | "hochmodern" / "modernste" | **"neu" / "aktuell"** (with a date) | AI-overused |
| holistic approach | "ganzheitlicher Ansatz" | **"vollständiger Ansatz"** or name the scope | "ganzheitlich" is the AI fingerprint for "holistic" |
| to support a feature | "ein Feature supporten" | **"unterstützen"** | don't transliterate "support" |
| robust solution | "robuste Lösung" | **"solide / zuverlässige" + the concrete thing** | drop "Lösung" if possible |
| world-class | "weltklasse" | **"erstklassig"** (with restraint) / cite a benchmark | AI-overused |
| game changer | "Game-Changer" | **"Wendepunkt"** | translate; don't leave English in body copy |

**Doctrine.** These are context-dependent (a few, like "erstklassig", are tolerable), so they need a native judgment call, not a blind find-replace. In translated marketing/editorial copy aimed at a German professional audience, prefer the idiomatic column. The tell is strongest when the calque is the **central concept** of the piece.

### Syntactic calques

- **"X-getrieben"** / **"angetrieben von X"** (X-driven / powered by X) → "auf Basis von X", "mit X" — fine sparingly, AI overuses
- **"X-bereit"** (X-ready) → "kompatibel mit X", "vorbereitet für X"
- **"das X-erste Unternehmen"** (X-first, calque) → "ein Unternehmen, das zuerst auf X setzt"
- **Gerund/-ing-as-title** ("Optimieren deines Workflows") — English progressive-title calque. German prefers an infinitive-noun or a question: "So optimierst du deinen Workflow", "Workflow-Optimierung".
- **Passive overuse** — AI overuses the passive ("es wird empfohlen, dass") where German prefers "man empfiehlt" / active voice.

### Punctuation / typography calques

- **Straight quotes `"X"`** instead of German `„X"` (Gänsefüßchen) — **the wired DE detector** (`detect_straight_quotes`). See below.
- **Em-dash `—`** instead of the native en-dash `–` — the wired em-dash tell. See above.
- **Title Case in DE titles** — German capitalizes ALL nouns natively (see note below), so "Title Case Every Word" is harder to spot than in EN/ES; the tell is capitalizing *non-noun* words (articles, prepositions, verbs) that German leaves lowercase mid-title.
- **Decimal point instead of comma** ("3.5 Millionen" where German uses "3,5 Millionen") and **thousands separator** ("1,000" where German uses "1.000" or "1 000") — locale tells.
- **"in 2024"** — see lexical calques; German omits the "in".

---

## German typography tells (the straight-quote detector + more)

### Straight quotes `"` vs German `„ "` — the signature DE typography tell

German uses **Gänsefüßchen**: a low-9 opening quote `„` (U+201E) and a high-6 closing quote `"` (U+201C). The pattern is `„Text"`. (Swiss Standard German and some editorial styles use the French guillemets `»Text«` — also native.) **AI plain-text German output frequently emits straight ASCII quotes `"X"`** (English habit). A run of straight `"` in otherwise-native prose is a strong tell.

- AI output: `"ein profitables Projekt"`  →  straight ASCII quotes
- Native DE: `„ein profitables Projekt"`  (or Swiss `»ein profitables Projekt«`)

The detector (`detect_straight_quotes`) counts straight ASCII double-quote characters `"` (U+0022). `strip_non_prose` removes fenced code and tables first, so most inch-marks and code-string quotes are gone before counting; what remains is prose-level usage. The threshold `straight_quote_max: 1` tolerates a single stray quote (e.g. a surviving inch-mark like `12"`) without false-firing, and clean native prose carries 0. It is low-weight (chat/quick-notes register legitimately uses straight quotes — for that register use `--type short-comms`).

**Cleanup rule.** When cleaning AI German for publication, sweep every `"X"` and convert to `„X"` (or `»X«` for Swiss/guillemet style). Match opening and closing forms consistently.

### ß vs ss — the Eszett

The **ß** (Eszett / scharfes S) follows a long vowel or diphthong ("Straße", "Maß", "groß", "weiß"); **ss** follows a short vowel ("Fluss", "muss", "dass"). Two failure modes both read as non-native:

1. **Systematic ß→ss replacement** ("Strasse", "Mass", "gross") — this is *correct in Swiss Standard German* (which abolished the ß), so it is only a tell if the rest of the text is otherwise de-DE/de-AT. In a clearly German-German document, uniform "ss" for "ß" suggests either a Swiss source or a mangled encoding.
2. **Wrong ß/ss after vowel length** ("Fluß" for "Fluss", "daß" for "dass" — pre-1996 *alte Rechtschreibung*) — dates the text or signals an old corpus.

Modern de-DE / de-AT keeps the ß. A document that drops it entirely (outside Swiss context) is suspect. The analyzer does not score ß/ss (too register- and locale-dependent for a deterministic check); the human reviewer flags it.

### The en-dash vs the em-dash vs the hyphen

- **Bindestrich `-`** (hyphen) — compound words ("E-Mail", "Service-Schicht"), word-break.
- **Halbgeviertstrich `–`** (en-dash, U+2013) — **NATIVE** German aside/parenthetical dash AND range dash ("Seiten 12–14", "Mo–Fr"). Never flagged. See item #1.
- **Geviertstrich `—`** (em-dash, U+2014) — **FOREIGN**; the actual tell. Replace with `–` or comma/colon/parentheses.

AI conflates these — most often importing the foreign `—` where German wants `–`. Cleanup replaces `—` with `–`.

### Capitalized nouns — why the header/title tells differ from EN/ES

German capitalizes **all nouns**, everywhere, natively ("der Tisch", "die Skalierbarkeit", "ein Unternehmen"). This is *correct German*, not a tell. Two consequences for the structural detectors:

- The English/Spanish "Title Case Every Word" tell is **weaker and harder to read** in German, because so many words in any German title are legitimately capitalized already. The German-specific signal is capitalizing the *non-nouns* a German title leaves lowercase (articles, prepositions, conjunctions, verbs, adjectives in mid-title): "Wie Man Den Workflow Optimiert" is a tell; "Wie man den Workflow optimiert" is correct.
- The header-pyramid detector (`detect_header_pyramid`) is **structural** (counts H3 children per H2) and is therefore unaffected by capitalization — it works in German exactly as in EN/FR/ES. But a human reviewer should not read German noun-capitalization in headers as an AI tell. (This note is duplicated in `tells-structural.md` for the structural reviewer.)

---

## Pedantische / schulmeisterliche Wendungen

Schoolbook German. The training corpus is heavy with academic and journalistic writing, so the model defaults to phrases a *Deutschlehrer* would have written in red pen, or *Beamtendeutsch* a clerk would have stamped.

Banned by default:

- **"Zunächst gilt es, den Kontext zu beleuchten"** — meta-textual scene-setting.
- **"Es ist wichtig zu beachten, dass"** — cf. supra.
- **"Ohne jeden Zweifel"** / **"Es steht außer Frage, dass"** — appeal to phantom consensus.
- **"Wie wir alle wissen"** / **"Wie allgemein bekannt"** — same.
- **"An diesem Punkt angelangt"** — fake academic transition.
- **"Abschließend lässt sich festhalten"** — fake academic closer.
- **"Auf den ersten Blick"** — AI's "at first glance" reflex (acceptable rarely).
- **"Der vorliegende Artikel"** / **"Das vorliegende Dokument"** — bureaucratic self-reference. Drop.
- **"Wir möchten im Folgenden"** / **"In diesem Artikel werden wir"** — academic intent statement. Drop.

Rewrite by **stating** instead of **announcing**.

- Vermeiden: "An diesem Punkt angelangt, gilt es, die Margen zu analysieren."
- Bevorzugen: "Jetzt zu den Margen."

---

## Conclusion templates (DE)

Never start a closing paragraph with any of:

- "Zusammenfassend,"
- "Zusammenfassend lässt sich sagen,"
- "Abschließend lässt sich sagen,"
- "Letztendlich,"
- "Letztlich,"
- "Im Großen und Ganzen,"
- "Fazit:"
- "Um es auf den Punkt zu bringen,"
- "Wie wir gesehen haben,"
- "Wie bereits erwähnt,"
- "Zusammenfassend ist die Zukunft vielversprechend,"
- "Dies ist erst der Anfang."

End on a concrete action, a number, or a sharp opinion. Examples:

- "Die Wahl hängt vom Volumen ab. Unter 100 Artikel: A. Darüber: B."
- "Drei Schritte: testen, messen, entscheiden. Der Rest ergibt sich."
- "Nicht überzeugt? Lass einen Dry-Run über 100 Zeilen laufen. Dann siehst du es."

---

## Voice and POV tells (DE)

### Kein "ich" auf 800+ Wörter

AI defaults to detached third-person or the impersonal "man". A native author of an opinion piece **uses first person at least once per 500 words**. Cleanup rule: insert one first-person sentence per 500 words to break the impersonal register.

- Vermeiden (im ganzen Text): "Man kann beobachten, dass die Margen fallen. Es ist wahrscheinlich, dass …"
- Bevorzugen: "Ich sehe die Margen bei drei von zehn Kunden fallen. Wahrscheinlich …"

### Übergebrauch des pädagogischen "wir"

The "we" of a textbook ("wir werden sehen, dass …"). AI defaults to this when asked to explain. Replace with "du/Sie" (direct address) or first-person singular.

- Vermeiden: "Wir werden uns ansehen, wie man migriert."
- Bevorzugen: "So migrierst du." / "Migration in drei Schritten."

### "Sie" als Marketing-Anrede überstrapaziert

The other extreme. "Sie", repeated every sentence, becomes salesy.

- Vermeiden: "Sie sparen Zeit. Sie sparen Geld. Sie gewinnen Ruhe."
- Bevorzugen: "Drei Stunden pro Tag gespart. 2.000 € im Monat. Und der Kunde ruft seltener an."

### Vorschreibendes Futur

"Sie werden entdecken", "Sie werden verstehen", "Sie werden in der Lage sein" — AI's prescriptive future. Replace with present or imperative.

- Vermeiden: "Sie werden unsere drei Säulen entdecken."
- Bevorzugen: "Drei Säulen. Schauen wir sie uns an."

### Höflichkeitskonjunktiv im Übermaß

"Es könnte interessant sein", "es wäre ratsam" — AI's politeness-conditional. Replace with direct present/imperative.

- Vermeiden: "Es könnte interessant sein, es auszuprobieren."
- Bevorzugen: "Probier es aus."

### du / Sie consistency

A German piece mixing "du" and "Sie" within the same register is a tell of stitched-together output. Pick one address form per target audience and hold it.

---

## Tricolon rationing (DE)

Same rule as EN/FR/ES: **cap at 1 tricolon per 200 words.** German has strong rhetorical pull toward triadic structures, so a high count reads as borrowed rhythm rather than authored prose. The analyzer's `detect_tricolons` matches the "X, Y und Z" pattern (conjunction **"und"**).

Vary list sizes (2, 4, 5 items). Use asyndeton ("schnell, zuverlässig, sauber" without "und"). Don't close every list with ", und Z".

- Vermeiden: "Das Tool ist schnell, zuverlässig und präzise. Es verwaltet Pakete, Varianten und Sprachen. Es läuft täglich, wöchentlich und auf Abruf."
- Bevorzugen: "Das Tool verwaltet Pakete und Varianten in 12 Sprachen. Tägliche Läufe — oder auf Abruf für eine einmalige Prüfung."

### Sub-rule: tricolons in titles and bullets

AI title format: "Schnell, zuverlässig und effizient". AI bullet last item: "und skalierbar". Both are tricolon tells.

- Title rewrite: "Schnell. Und zuverlässig." / "Schnell und zuverlässig" (two items) / "Schnell" (one item).
- Bullet rewrite: drop the "und", or replace the last bullet with a different rhythm.

---

## Quick triage (for the human reviewer)

When auditing a 500-word DE piece, scan in this order:

1. **Dashes.** Count the foreign em-dash "—" (U+2014) in prose. If >1, replace with the native "–" (U+2013) or comma/colon/parentheses. **NEVER touch the native "–" — it is correct German.**
2. **Quotes.** Scan for straight ASCII `"X"`. Convert each to German `„X"` (or Swiss `»X«`). Be consistent.
3. **Opening sentence** — if it starts with a temporal abstraction ("in der heutigen Zeit", "im digitalen Zeitalter") or a meta-frame ("es ist wichtig zu beachten"), rewrite.
4. **Closing sentence** — if it starts with any conclusion template ("zusammenfassend", "letztendlich", "fazit"), rewrite.
5. **Tier-1 vocab** — scan; cap at 1 per paragraph (watch "nahtlos", "ganzheitlich", "robust", "hochmodern", "bahnbrechend").
6. **AI constructions** — pattern-match by eye ("es geht nicht nur um …, sondern um …", "tauchen Sie ein in …", "Sie fragen sich vielleicht …").
7. **Tricolons** — count "X, Y und Z"; cap at 1 per 200 words.
8. **Calques** — find and replace the lexical ones ("nahtlos", "nutzen", "hochmodern", "verwertbar", "in 2024", "macht Sinn"); for translated copy scan the EN→DE table.
9. **POV** — at least one first-person mark if it's opinion.
10. **Typography** — German quotes, ß vs ss, noun-capitalization is correct (don't flag it), decimal comma / thousands point in de-DE.

A passing DE piece, by this skill's bar:

- 0 foreign em-dashes "—" per 500 words (native "–" unlimited and unflagged).
- 0–1 straight ASCII quote per piece; otherwise German „ " / »«.
- 0 tier-1 vocab items, ≤ 2 tier-2.
- 0 AI constructions.
- ≤ 1 tricolon per 200 words.
- At least one first-person mark if opinion.
- Title with non-nouns in lowercase (nouns correctly capitalized).

That piece will score LOW_RISK (≤ 24) on the deterministic analyzer and survive Copyleaks / GPTZero at sub-25 % AI probability in most domains.

---

## See also

- `tells-statistical.md` — burstiness, TTR, comma density doctrine (language-agnostic)
- `tells-structural.md` — bullets, headers, tricolons, emoji — plus the German noun-capitalization note for the header/title tell
- `humanization-techniques.md` — how to write with intentional asymmetry, with German worked examples
- `scripts/analyze.py` — `detect_suspect_vocab`, `detect_ai_constructions`, `detect_tricolons`, `detect_straight_quotes`, `detect_em_dashes` (U+2014 only)
- `scripts/rules.yaml` — DE thresholds (incl. `straight_quote_max`), suspect vocabulary, content-type weights
- the `human-writer` per-language family — sibling skills `human-writer-en`, `human-writer-fr`, `human-writer-es`, `human-writer-pt`, `human-writer-ar`, `human-writer-hi`, same architecture and reference set
