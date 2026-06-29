# Pre-Publish Checklists (human-writer-de)

> Ready-to-paste self-review checklists, one per content-type, for German prose. Run `analyze.py --lang de` first, then walk through the relevant checklist before delivery. Each checklist is short, actionable, and covers the tells most likely to trigger AI detectors for that specific format. Adapted from the master `human-writer` skill; the German quick-triage at the end is the satellite-specific addition.

> **Reminder (German-critical):** the spaced en-dash "–" (U+2013) is NATIVE German and is NEVER a tell. The em-dash "–" you must hunt is the FOREIGN "—" (U+2014). German quotes are „ " (Gänsefüßchen) or »« — straight ASCII "X" is the tell.

---

## Marketing Long-Form

Before publishing blogs, READMEs, landing pages, or long-form newsletters in German, confirm:

- [ ] Analyzer score ≤ 24 with `--lang de --type marketing`
- [ ] First 100 words contain zero Tier-1 suspect vocabulary
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] First paragraph does not open with "In der heutigen Zeit", "Im digitalen Zeitalter", "Es ist wichtig zu beachten", or "Egal, ob Sie X oder Y"
- [ ] Closing paragraph does not start with "Zusammenfassend", "Letztendlich", "Letztlich", or "Fazit:"
<!-- human-writer:ignore-end -->
- [ ] Foreign em-dash "—" count = 0 (run `analyze.py` to check); native "–" asides are fine and unlimited
- [ ] All quotes are German „ " (or Swiss »«); zero straight ASCII "X"
- [ ] At least one specific number, named entity, or first-person opinion per ~300 words
- [ ] Bulleted lists vary opening verbs (≤ 80% share the same first word)
- [ ] No H2 has exactly 3 H3 children (if 2+ H2s exist, vary H3 counts)
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "Das Wichtigste in Kürze" / "Was Sie lernen werden" / "Die X besten" standalone block
<!-- human-writer:ignore-end -->
- [ ] No emoji as section header

If any item fails: fix before delivery. If analyzer score is 25–49 after fixes, apply the top 3 recommendations and re-score. If 50+, reconsider your angle.

---

## Short-Form Communications

Before sending emails, LinkedIn posts, or customer replies in German, confirm:

- [ ] Analyzer score ≤ 24 with `--lang de --type short-comms`
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "Ich hoffe, diese Nachricht erreicht Sie wohlauf" opening
- [ ] No "Ich freue mich auf Ihre Rückmeldung" / "Mit freundlichen Grüßen" boilerplate as the whole sign-off thought
- [ ] No "Ich melde mich bei Ihnen, um", "Zögern Sie nicht, mir zu schreiben", "Bei Fragen stehe ich Ihnen gerne zur Verfügung"
<!-- human-writer:ignore-end -->
- [ ] Foreign em-dash "—": 0; native "–" fine
- [ ] At least one contraction, sentence fragment, or first-person verb in replies over 30 words
- [ ] Sign-off is first name, short phrase, or nothing, not the same corporate boilerplate every email
- [ ] First line is not "Ich wende mich an Sie" / "Ich schreibe Ihnen, um" / "Ich wollte mich melden"

If any item fails: revise before sending. (Note: in casual chat/SMS register, straight ASCII quotes are acceptable; the analyzer's straight-quote weight is lower under `--type short-comms`.)

---

## Technical Documentation

Before publishing internal READMEs, RFCs, or technical journals in German, confirm:

- [ ] Analyzer score ≤ 24 with `--lang de --type technical`
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] First sentence states the WHY (problem this doc solves), not "In diesem Dokument werden wir … erkunden"
- [ ] No "Legen wir los!" / "Fangen wir an!" / "Ohne weitere Umschweife" filler
- [ ] No "Fazit" section (unless >500 words and earns a summary)
- [ ] Every instance of "robust", "skalierbar", "nutzen", "elegant", "leistungsstark" passes the "replace with 'gut'" test
<!-- human-writer:ignore-end -->
- [ ] Code blocks are not surrounded by paragraphs that re-explain them line-by-line
- [ ] One sentence per concept: no restating or hedging the same idea twice
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] Zero hedging openers ("Es ist wichtig zu beachten", "Es sei darauf hingewiesen", "Es gilt zu beachten", "Hinweis:")
<!-- human-writer:ignore-end -->

If any item fails: revise. Technical docs should be direct and economical.

---

## Editorial-SEO Articles

Before publishing ranking-optimized articles in German, confirm:

- [ ] Analyzer score ≤ 24 with `--lang de --type editorial-seo`
- [ ] Target keyword appears in: title, first 100 words, and ≥1 H2
- [ ] First 100 words contain a specific data point or opinion (not just the keyword)
- [ ] H2 hierarchy is varied, not pyramid (not 3 H3s per H2 uniformly)
- [ ] At least 1 internal link with natural anchor text (not "hier klicken")
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "In der heutigen Welt der X," / "Im digitalen Zeitalter," opener
- [ ] No "Fazit" that just restates the intro / tl;dr
<!-- human-writer:ignore-end -->
- [ ] At least one opinion that someone could reasonably disagree with
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "Der ultimative Leitfaden", "Alles, was Sie wissen müssen", "Der komplette Guide" UNLESS your keyword data demands it
<!-- human-writer:ignore-end -->
- [ ] Sentence-length standard deviation ≥ 8 (mix short and long)
- [ ] Title: non-noun words lowercase (German nouns are correctly capitalized — that is NOT a tell)

If any item fails: fix before publishing.

---

## Universal Baseline

Apply to **all** content types before delivery:

- [ ] Final `analyze.py --lang de` run; score ≤ 24
- [ ] No Tier-1 suspect vocabulary in the first 100 words (check `rules.yaml` for the list)
- [ ] Zero foreign em-dashes "—" (native "–" is fine and unflagged)
- [ ] All quotes in German „ " / »« form (except deliberate chat/SMS register)
- [ ] At least one specific element: number, name, date, fact, quote, not generic claims
- [ ] At least one stylistic asymmetry: a short sentence after long ones, a varied list length, unexpected structure
- [ ] Doesn't end with a summary or call-to-action that merely restates the opening

---

## German quick-triage (satellite-specific)

A 60-second eyeball pass for any German draft, in priority order — the fastest path from "looks AI" to "reads human". Mirrors the order in `tells-stylistic-de.md` § Quick triage:

1. **Dashes.** Count the foreign em-dash "—" (U+2014) in prose. >0 → replace with native "–" (U+2013) or comma/colon/parentheses. **NEVER touch the native "–" — it is correct German.**
2. **Quotes.** Scan for straight ASCII "X". Convert each to German „X" (or Swiss »X«). Be consistent.
3. **Opener.** First sentence starts with "in der heutigen Zeit / im digitalen Zeitalter / es ist wichtig zu beachten / tauchen Sie ein"? Rewrite.
4. **Closer.** Last paragraph starts with "zusammenfassend / letztendlich / letztlich / fazit"? Rewrite.
5. **Tier-1 vocab.** "nahtlos (seamless), nutzen (leverage), hochmodern (cutting-edge), ganzheitlich (holistic), robust, bahnbrechend, unverzichtbar" — cap 1 per paragraph.
6. **Constructions.** "es geht nicht nur um …, sondern um …", "egal, ob Sie … oder …", "Sie fragen sich vielleicht", "ohne jeden Zweifel".
7. **Tricolons.** Count "X, Y und Z"; cap 1 per 200 words.
8. **Calques.** "nahtlos" (seamless), "nutzen" (leverage), "hochmodern" (cutting-edge), "ganzheitlich" (holistic), "verwertbar" (actionable), "in 2024" (in 2024), "macht Sinn" (makes sense).
9. **POV.** At least one first-person mark if it's opinion.
10. **Typography.** German quotes, ß vs ss intact, noun-capitalization is CORRECT (don't flag it — only mid-title non-nouns), decimal comma + thousands point in de-DE.

A passing piece: 0 tier-1, ≤2 tier-2, 0 constructions, ≤1 tricolon/200w, 0 foreign "—" (native "–" unlimited), German quotes throughout, one first-person mark if opinion, non-noun title words lowercase. That lands LOW_RISK (≤ 24) and survives Copyleaks/GPTZero at sub-25 % in most domains.

---

## How to Use These Checklists

1. Write or clean your draft.
2. Run `python3 scripts/analyze.py --input <draft> --lang de --type <type> --format human` from the skill root.
3. If score > 24, apply the analyzer's top recommendations until ≤ 24.
4. Walk through the relevant checklist above (marketing, short-form, technical, or editorial-SEO) plus the German quick-triage.
5. Check off each item; revise any failures.
6. Re-run the analyzer if you made major changes.
7. Deliver only when both gates pass (analyzer ≤ 24 AND checklist complete).

---

## See Also

- `tells-stylistic-de.md`: vocabulary, constructions, calques, punctuation tells (German), and the en-dash-is-native trap
- `adapter-marketing.md`: doctrine specific to long-form marketing
- `adapter-short-comms.md`: doctrine specific to short communications
- `humanization-techniques.md`: positive techniques for adding human voice (German examples)
- `scripts/analyze.py`: deterministic analyzer (run before checklist)
- `external-detectors.md`: optional integration with Copyleaks, GPTZero, etc.
