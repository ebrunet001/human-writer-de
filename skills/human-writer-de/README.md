# Human Writer — German (de) — a Claude skill

A Claude Code skill that produces **German** prose reading as human-authored, sanitizes German AI drafts to remove detector tells, and scores any German draft for AI-detection risk before publication.

This is the **German member** of the `human-writer` per-language family — one autonomous skill per language (`en`, `fr`, `es`, `pt`, `de`, `ar`, `hi`), all built on the same architecture. They install side by side and do not conflict: each activates on its own language triggers.

## Why this skill exists

`mr-bridge.com` ships localized marketing and editorial copy in German. Modern Sonnet / Opus / GPT-class German output is fluent enough to ship, but it carries fingerprints that commercial detectors (Copyleaks, GPTZero, Originality.ai) latch onto, and German carries its own tells the EN/FR analyzer never checked:

- **Statistical:** low burstiness, narrow type-token ratio (language-agnostic).
- **Stylistic:** the same forty or fifty inflated words repeated across drafts ("es ist wichtig zu beachten", "im digitalen Zeitalter", "nahtlos", "ganzheitlich", "robust"), formulaic frames ("Es geht nicht nur um X, sondern um Y").
- **Typographic (German-specific):** straight ASCII quotes `"X"` where native German uses `„X"` (Gänsefüßchen), the **foreign em-dash „—" (U+2014)** imported from English where German uses commas or the native en-dash, EN→de calques ("nahtlos", "nutzen", "hochmodern", "ganzheitlich", "verwertbar").

### The German en-dash trap

German has TWO similar-looking dashes with opposite meanings:

- **„–" (U+2013, Halbgeviertstrich, spaced en-dash)** — **NATIVE, CORRECT** German typography for asides and ranges. It is good writing. It is **NEVER** a tell.
- **„—" (U+2014, em-dash)** — **FOREIGN** to German; the actual AI/Anglo import tell.

The analyzer's em-dash detector counts ONLY „—" and is deliberately blind to „–". A regression test and two native fixtures (which deliberately contain „–") guarantee the en-dash is never false-flagged.

A draft drops, it goes through a detector, the score comes back at 70%+, and it's rejected. The fix is a disciplined sweep of the specific German tells detectors weight. This skill encodes that doctrine plus a deterministic analyzer so any Claude Code session can produce, clean, or audit German prose to a target score before delivery.

## What it can do

**Three modes:**
- **Write (schreiben):** produce new German content already engineered to score LOW_RISK.
- **Clean (bereinigen):** rewrite an existing German AI draft to strip tells, preserving meaning.
- **Audit (prüfen):** score a German draft, surface the top offending tells, recommend fixes without rewriting it.

**One language, fully specialized:**
- **German (de):** ~170-entry suspect vocabulary (Tier 1 + Tier 2), German AI-construction regex bank, German tricolon detector (`und` conjunction), and a dedicated **straight-quote** typography detector (`"X"` vs `„X"`) — calibrated NOT to false-fire on clean native prose. The em-dash detector counts only the foreign „—" and never the native „–".

**Four content-types** (each with its own adapter): marketing long-form, short-comms, technical, editorial-SEO.

**Optional external integration:** live scoring via Copyleaks, GPTZero, or Originality.ai (`--external <provider>`). Lazy `httpx` import, so the analyzer works fully offline.

## What's inside

```
human-writer-de/
├── SKILL.md                          # Orchestration hub: routing + master checklist + anti-patterns (de)
├── README.md                         # This file
├── INSTALL.md                        # Installation instructions
├── requirements.txt                  # pyyaml (required) + httpx (optional)
├── references/
│   ├── tells-stylistic-de.md         # ⭐ CORE: DE suspect vocab + AI constructions + straight-quote/en-dash typography + calques
│   ├── tells-statistical.md          # burstiness / TTR (language-agnostic)
│   ├── tells-structural.md           # bullets / headers / tricolons / emoji + German noun-capitalization note
│   ├── humanization-techniques.md    # the ten humanization moves (German worked examples)
│   ├── adapter-marketing.md          # marketing long-form adapter
│   ├── adapter-short-comms.md        # short-form comms adapter
│   ├── adapter-technical.md          # technical-docs adapter (prose-only contract)
│   ├── adapter-editorial-seo.md      # editorial-SEO adapter
│   ├── external-detectors.md         # Copyleaks / GPTZero / Originality.ai integration notes
│   └── checklists.md                 # pre-publish checklists + German quick-triage
└── scripts/
    ├── rules.yaml                    # ⭐ de: vocab + constructions + thresholds (incl. straight_quote_max)
    └── analyze.py                    # ⭐ de-adapted: tricolon (und) + detect_straight_quotes; em-dash counts only „—"
```

## Installation

See `INSTALL.md`. TL;DR for macOS/Linux:

```bash
mkdir -p ~/.claude/skills
unzip human-writer-de.zip -d ~/.claude/skills/
pip install --user -r ~/.claude/skills/human-writer-de/requirements.txt
```

## How to invoke

Once installed, the skill auto-activates on German prose requests. Example prompts:

- "Schreib einen 600-Wörter-Artikel über Wein-Futures-Pricing, der menschlich klingt, nicht nach KI"
- "Bereinige diesen KI-Entwurf, damit der Copyleaks-Score unter 25 fällt"
- "Prüfe diese deutsche E-Mail auf KI-Erkennung, bevor ich sie sende"
- "Humanisiere diesen Text, er klingt zu sehr nach ChatGPT"
- "Make this German landing-page copy read human, not AI"

If auto-activation misses, force it: "Use the `human-writer-de` skill to..."

## The analyzer

`scripts/analyze.py` is a deterministic scorer that runs offline. It loads `scripts/rules.yaml` (German vocab lists, regex patterns, thresholds) and emits a 0-100 score plus a list of flagged tells.

```bash
# Audit mode (default, JSON output for tooling)
python3 scripts/analyze.py --input draft.md --lang de --type marketing

# Human-readable report
python3 scripts/analyze.py --input draft.md --lang de --type editorial-seo --format human

# Pipe via stdin
cat draft.md | python3 scripts/analyze.py --lang de --type technical --format human
```

Required flags: `--lang de`, `--type` (`marketing` / `short-comms` / `technical` / `editorial-seo`). `--input` is optional; stdin otherwise.

**Score bands** (4-band YAML, canonical):
- `0-24` **LOW_RISK:** ship it.
- `25-49` **MEDIUM_RISK:** apply the top 3 recommendations, re-score.
- `50-74` **HIGH_RISK:** in WRITE mode restart; in CLEAN mode apply a stronger rewrite.
- `75-100` **CRITICAL:** major rewrite.

**Detectors implemented:** em-dash density (foreign „—" only, never the native „–"), sentence-length stdev (burstiness), TTR (lexical diversity), German suspect-vocabulary, German AI-construction regex bank, German tricolon (`und`), bullet parallelism, header pyramid, and the German-only **straight-quote** detector (low-weight, register-calibrated).

**Prose-only scoring.** The analyzer strips fenced code blocks, markdown data tables, and opt-in ignore regions before scoring. Wrap any intentionally AI-flavored citation so it doesn't count against you:

```
<!-- human-writer:ignore-start (Zitat eines schlechten Beispiels) -->
In der heutigen Zeit ist es wichtig zu beachten, dass robuste Lösungen nahtlos funktionieren.
<!-- human-writer:ignore-end -->
```

## What's NOT inside

- **English / French content:** use `human-writer-en` / `human-writer-fr`. Other locales: `human-writer-es` / `-pt` / `-ar` / `-hi`.
- **Document structure** (which sections, which schemas, which headings): use a dedicated structure/content tool. This skill is the stylistic filter applied on top of whatever produces the structure.
- **Technical SEO audit of a web project:** use a dedicated SEO tool. This skill handles content style only.

This skill is a stylistic filter invoked on top of structure-producing tools, never as a replacement.

## Part of the mr-bridge.com toolkit

This skill is part of the [mr-bridge.com](https://mr-bridge.com) toolkit for scraping, data, and content automation. Related resources:

- [mr-bridge.com](https://mr-bridge.com) — home
- [Scrapers](https://mr-bridge.com/scrapers) — the Apify Actor portfolio
- [MCP servers](https://mr-bridge.com/mcp-servers) — Model Context Protocol servers
- [AI workflows](https://mr-bridge.com/ai-workflows) — agents and automation
- [Studies](https://mr-bridge.com/studies) — data studies and one-pagers
- [Articles](https://mr-bridge.com/articles) — write-ups and guides
- [Solutions](https://mr-bridge.com/solutions) — end-to-end solutions

## License

Personal use. Customize freely. No warranty. The external-detector endpoints in `analyze.py` are language-agnostic POSTs and carry `# (verify)` markers; they were not re-verified for German specifically.
