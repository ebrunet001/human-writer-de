# Installation — Human Writer (German) skill

This skill is designed to be installed at the **user level** in Claude Code, so it's available across all your projects without copying it into each repo.

It is the German member of the `human-writer` per-language family (one autonomous skill per language: `-en`, `-fr`, `-es`, `-pt`, `-de`, `-ar`, `-hi`). Install it alongside the other family members — they do not conflict; each activates on its own language triggers.

## Prerequisites

- Claude Code installed and working
- Python 3.10+ available on your system (required for the analyzer script)
- `pip` available for installing one mandatory dependency
- A terminal with `unzip` (macOS, Linux) or PowerShell with `Expand-Archive` (Windows)

## Installation

### macOS / Linux

```bash
# 1. Create the user-level skills directory if it doesn't exist
mkdir -p ~/.claude/skills

# 2. Unzip the skill into that directory
unzip human-writer-de.zip -d ~/.claude/skills/

# 3. Install Python dependencies
pip install --user -r ~/.claude/skills/human-writer-de/requirements.txt
# pyyaml is required; httpx is optional (only for --external mode)

# 4. Verify the structure
ls ~/.claude/skills/human-writer-de/
# Expected: SKILL.md  README.md  INSTALL.md  requirements.txt  references/  scripts/

# 5. Make the analyzer script executable (optional, for convenience)
chmod +x ~/.claude/skills/human-writer-de/scripts/analyze.py
```

### Windows (PowerShell)

```powershell
# 1. Create the user-level skills directory if it doesn't exist
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"

# 2. Unzip the skill into that directory
Expand-Archive -Path .\human-writer-de.zip -DestinationPath "$env:USERPROFILE\.claude\skills\"

# 3. Install Python dependencies
pip install --user -r "$env:USERPROFILE\.claude\skills\human-writer-de\requirements.txt"

# 4. Verify the structure
Get-ChildItem "$env:USERPROFILE\.claude\skills\human-writer-de\"
# Expected: SKILL.md  README.md  INSTALL.md  requirements.txt  references/  scripts/
```

## Verification

Open Claude Code and ask:

> "What skills do you have access to?"

You should see `human-writer-de` in the list. If not, check that the SKILL.md file is at the path:

- macOS / Linux: `~/.claude/skills/human-writer-de/SKILL.md`
- Windows: `%USERPROFILE%\.claude\skills\human-writer-de\SKILL.md`

## First test

### Test the skill's auto-activation

In any directory (Claude Code session), ask:

> "Prüfe diesen Absatz auf KI-Erkennungsrisiko, ich befürchte, er klingt zu sehr nach ChatGPT: [paste a paragraph]"

The skill should activate, route to AUDIT mode, run `scripts/analyze.py --lang de`, and produce a structured report.

### Test the analyzer directly

The analyzer is a standalone CLI. It accepts input from `--input <file>` or stdin, requires `--lang de` and `--type`, and emits either JSON (default) or a human-readable report (`--format human`).

```bash
cd ~/.claude/skills/human-writer-de

# Audit your own draft (a file or stdin)
python3 scripts/analyze.py \
  --input path/to/your-draft.md \
  --lang de \
  --type marketing \
  --format human
```

Expected output: a header showing the score, top tells, recommendations. An AI-flavored draft (AI clichés, tricolons, the foreign em-dash „—", or straight ASCII quotes where German uses „ ") should land at HIGH_RISK or CRITICAL (≥ 50); clean native German prose should land at LOW_RISK (≤ 24), since native German carries no foreign em-dash „—" (it uses the native „–"), no straight ASCII quotes, and no suspect vocab.

### Quick smoke test (no draft needed)

You can point the analyzer at one of the shipped doctrine files to confirm it runs offline:

```bash
cd ~/.claude/skills/human-writer-de
python3 scripts/analyze.py \
  --input references/tells-stylistic-de.md \
  --lang de \
  --type marketing \
  --format human
```

Expected: a report prints with no error and no network call. (Catalog files cite many tells by design, so the score itself is not meaningful here — this only confirms the analyzer works.)

## Optional: external detector setup

The `--external` flag dispatches to one of Copyleaks, GPTZero, or Originality.ai. To use it:

1. Install `httpx` (already in `requirements.txt`).
2. Obtain an API key from the provider and export it:

```bash
export COPYLEAKS_EMAIL="..."       # Copyleaks needs email + key (two-step auth)
export COPYLEAKS_API_KEY="..."     # for --external copyleaks
export GPTZERO_API_KEY="..."       # for --external gptzero
export ORIGINALITY_AI_API_KEY="..." # for --external originality
```

3. Invoke:

```bash
python3 scripts/analyze.py \
  --input draft.md \
  --lang de \
  --type marketing \
  --external copyleaks \
  --format human
```

**Provider verification status:**
- **Copyleaks: live-verified (2026-06-02).** Two-step auth (login with email+key → 48h access token → Bearer on the detector call), response parsed at `summary.ai`. Requires BOTH `COPYLEAKS_EMAIL` and `COPYLEAKS_API_KEY`. Minimum input is ~255 characters (shorter text returns a 400, surfaced as an error, not a crash). `sandbox` mode returns the same shape for free. Free-tier credits are minimal; budget paid credits before looping over `--external copyleaks`. The `--external` plumbing is language-agnostic POSTs and was not re-verified for German specifically.
- **GPTZero / Originality.ai: paused (paid-only API).** Their endpoints and response-parsing JSON paths in `call_gptzero` / `call_originality` remain best-effort reconstructions from public docs, each with an inline `# (verify)` comment.

If `httpx` is not installed, `--external` dispatch raises `ImportError` cleanly without breaking the analyzer's local scoring.

## Updating the skill

To install a newer version, just replace the directory:

```bash
# macOS / Linux
rm -rf ~/.claude/skills/human-writer-de
unzip human-writer-de.zip -d ~/.claude/skills/
pip install --user -r ~/.claude/skills/human-writer-de/requirements.txt
```

Your customizations to `scripts/rules.yaml` (vocab lists, thresholds) will be lost. If you want to preserve them, fork the skill locally before updating.

## Uninstalling

```bash
# macOS / Linux
rm -rf ~/.claude/skills/human-writer-de

# Windows (PowerShell)
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude\skills\human-writer-de"
```

To also uninstall the Python deps (only if you don't use them elsewhere):

```bash
pip uninstall pyyaml httpx
```

## Troubleshooting

**Skill doesn't activate when expected.**
The skill's auto-activation depends on the description in `SKILL.md`'s frontmatter. Triggers include: "menschlich schreiben", "diesen Text humanisieren", "KI-Text bereinigen", "auf KI-Erkennung prüfen", "humanize this German draft". If your phrasing doesn't match, force activation: "Use the `human-writer-de` skill to..."

**Analyzer fails with `ModuleNotFoundError: No module named 'yaml'`.**
`pyyaml` is mandatory. Install with `pip install --user pyyaml`.

**Analyzer fails with `ModuleNotFoundError: No module named 'httpx'` when using `--external`.**
`httpx` is optional and only needed for external detector dispatch. Install with `pip install --user httpx`. Without `--external`, the analyzer runs fully offline.

**Analyzer flags legitimate human German prose as AI (false positive).**
This shouldn't happen at score ≤ 24, but tolerances are content-type-specific. If you find a clear FP:
1. Run `--format human` to see which detector flagged it.
2. Either loosen the threshold in `scripts/rules.yaml` for that detector, or add the flagged construct to a project-specific allowlist.
3. Keep the prose as a regression sample so you can re-check it after future edits.

**The straight-quote detector flags clean prose.**
The straight-quote detector (`detect_straight_quotes`) only fires when the count of straight ASCII double-quotes (`"`) in prose exceeds `straight_quote_max` (default 1). If your prose deliberately uses straight quotes (chat/SMS register, or code-heavy notes), raise `straight_quote_max` in `rules.yaml` or switch `--type short-comms`. Note: the native German spaced en-dash `–` (U+2013) is never counted by the em-dash detector — only the foreign `—` (U+2014) is.

**Python 3.10+ syntax errors.**
`analyze.py` uses `from __future__ import annotations` and PEP 604 union syntax (`int | None`). On Python 3.8 or 3.9 this will fail. Upgrade Python.

## Skill content overview

For the full skill layout, see [`README.md`#whats-inside](README.md#whats-inside).

## Support

This skill is a personal toolkit. For doctrine improvements (new German tells you keep seeing in the wild that aren't in `references/tells-stylistic-de.md`), edit that file directly. It is the authoritative surface; the analyzer is calibrated to follow.
