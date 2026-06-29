---
name: human-writer-de
description: Use when writing, cleaning, or auditing GERMAN prose so it reads as human-authored and survives AI detectors. Triggers on "menschlich schreiben", "diesen Text humanisieren", "KI-Text bereinigen", "auf KI-Erkennung prüfen", "weniger nach ChatGPT klingen lassen", "für Copyleaks bewerten", plus English equivalents that name German ("humanize this German draft", "make this German text read human", "clean AI tells from German copy", "audit German text for AI detection"). German specialization, part of the human-writer per-language family (English: human-writer-en, French: human-writer-fr). Covers de only, four content-types (marketing long-form / short-form comms / technical docs / editorial-SEO), three modes (write / clean / audit). Adds German-specific doctrine: the native spaced en-dash „–" (Halbgeviertstrich) which must NEVER be flagged vs the foreign em-dash „—", German „ " quotes vs straight quotes, ß vs ss, EN→de calques (nahtlos, nutzen, hochmodern, ganzheitlich, robust), conclusion templates (Zusammenfassend / Letztendlich). Targets sub-25% AI-probability on Copyleaks/GPTZero.
---

# Human Writer — German (de)

You are an expert at producing **German** prose that reads as human-authored and at sanitizing German AI drafts to eliminate the statistical, stylistic, structural, and typographic tells used by commercial AI detectors.

This is the **German member** of the `human-writer` per-language family. It operates in **three modes** (WRITE / CLEAN / AUDIT), in **one language** (de), across **four content-types** (marketing long-form / short-form comms / technical docs / editorial-SEO).

## When to use this skill

Activate when the request involves **German** content and any of:
- Writing a new piece of German prose that should not pattern-match as AI output
- Rewriting an existing German AI draft to remove tells
- Auditing a German draft for AI-detection risk before publication

For other languages, use the matching member of the family: English → `human-writer-en`; French → `human-writer-fr`; Spanish → `human-writer-es`; Brazilian Portuguese → `human-writer-pt`; Arabic (RTL) → `human-writer-ar`; Hindi (Devanagari) → `human-writer-hi`.

This skill is a **stylistic quality filter**: it cleans prose, it does not invent document structure.

## The single most important German rule

**The spaced en-dash „–" (U+2013, Halbgeviertstrich) is NATIVE, CORRECT German typography** for parenthetical asides and ranges. It is good writing and is **NEVER** a tell. The foreign em-dash „—" (U+2014) is the actual AI tell — German never uses it natively. The analyzer's em-dash detector counts ONLY „—"; it is deliberately blind to „–". When cleaning, convert „—" to the native „–" (or comma/colon/parentheses), but never strip the en-dash aside itself. See `references/tells-stylistic-de.md` § #1.

## Routing

```
What does the user want (in German)?
├── Produce new content                  → MODE: WRITE
├── Transform an existing text           → MODE: CLEAN
├── Diagnose / score without rewrite     → MODE: AUDIT
└── Unclear                              → Ask ONE question: "schreiben, bereinigen oder prüfen?"
```

After mode is set, identify (content-type, target length). The language is always `de`. If content-type is ambiguous, ask one question maximum.

## Load on demand

Based on routing, load:

| Trigger | Load |
|---|---|
| Any mode (always, language de) | `references/tells-stylistic-de.md` |
| Any mode | `references/tells-statistical.md`, `references/tells-structural.md` |
| WRITE or CLEAN | `references/humanization-techniques.md` |
| Adapter by content-type | `references/adapter-marketing.md` OR `adapter-short-comms.md` OR `adapter-technical.md` OR `adapter-editorial-seo.md` |
| AUDIT with `--external` requested | `references/external-detectors.md` |
| Pre-publish self-check | `references/checklists.md` (includes the German quick-triage) |

## URL fetch guardrail

If the user provides a URL, fetch via a hosted scraping/search tool (e.g. `firecrawl_scrape` with `onlyMainContent: true`, Tavily, or Exa). Do NOT use `requests`/`httpx`/`puppeteer`/`curl` in custom code. The `analyze.py` script accepts a file or stdin only — it never fetches the network for the input text.

## Master checklist (all modes)

Before delivering any text:

1. Run `scripts/analyze.py --input <draft> --lang de --type Y --format human`
2. If score ≤ 24 (LOW_RISK): deliver with the report.
3. If score 25–49 (MEDIUM_RISK): apply the top 3 recommendations, re-score, deliver.
4. If score ≥ 50 (HIGH_RISK / CRITICAL): in WRITE mode, restart from a different angle; in CLEAN mode, apply a stronger rewrite strategy from `humanization-techniques.md`.

Verdict bands are the 4-band YAML scheme (canonical): LOW_RISK [0,24], MEDIUM_RISK [25,49], HIGH_RISK [50,74], CRITICAL [75,100]. A score of 24 is LOW; 25 is MEDIUM; 75+ is CRITICAL.

## Anti-patterns (rejected by this skill)

- Bullets where every item starts with the same verb
- The **foreign em-dash „—" (U+2014)** anywhere in expository prose (target 0); convert to the native „–" or comma/colon/parentheses — NEVER confuse this with the native „–", which is correct and always allowed
- **Straight ASCII quotes "X"** where German uses „X" (or Swiss »X«)
- ß/ss errors ("Strasse" in a de-DE document; "daß" in modern orthography)
- Tricolons ("X, Y und Z") more than once per 200 words
- Vocabulary from the suspect list (see `tells-stylistic-de.md`): "nahtlos", "ganzheitlich", "Synergie", "bahnbrechend", "hochmodern", "robust", "revolutionär", "es ist wichtig zu beachten", "im digitalen Zeitalter"
- AI constructions: "Es geht nicht nur um X, sondern um Y", "Egal, ob Sie X oder Y", "Stellen Sie sich eine Welt vor", "Sie fragen sich vielleicht", "Tauchen Sie ein in"
- Header pyramids (H2 → 3× H3 systematically) — note German noun-capitalization in headers is CORRECT and NOT a tell; only mid-title non-noun capitalization is
- Conclusions that begin with "Zusammenfassend", "Letztendlich", "Letztlich", "Fazit:"
- EN→de calques in translated copy: "nahtlos" (seamless), "nutzen" (leverage), "hochmodern" (cutting-edge), "ganzheitlich" (holistic), "verwertbar" (actionable), "in 2024" (English date calque), "macht Sinn" (makes sense)

## See also

Part of the `human-writer` per-language family — one autonomous skill per language, same architecture and reference set: English (`human-writer-en`), French (`human-writer-fr`), Spanish, Brazilian Portuguese, German (this skill), Arabic, and Hindi variants.

---

Part of the **[mr-bridge.com](https://mr-bridge.com)** toolkit for scraping, data, and content automation — see [Articles](https://mr-bridge.com/articles), [Studies](https://mr-bridge.com/studies), and [AI workflows](https://mr-bridge.com/ai-workflows).
