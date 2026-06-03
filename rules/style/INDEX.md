# Style Index

The learned voice. Read these every session before polishing.

## Files

- [STYLE_PROFILE.md](STYLE_PROFILE.md) — distilled rules across 4 dimensions (A word/phrasing · B sentence/structure · C tone/formality · D scenario), each with confidence + evidence + source sessions.
- [LEXICON.md](LEXICON.md) — high-density `avoid → prefer` word table, same confidence model.
- [scenarios/](scenarios/) — per-scenario overrides; only the **deltas** from the baseline profile. See [scenarios/INDEX.md](scenarios/INDEX.md).

## How to read it

1. Load STYLE_PROFILE + LEXICON as the **baseline**.
2. If polishing for a known scenario, **overlay** that scenario file (scenario rules win on conflict).
3. Apply by confidence: `established` silently, `tentative` with a flag, `seed`/`contested` not at all.

## How it grows

Rules are never hand-authored as facts; they are **earned from evidence**:
- `ONBOARD` writes `seed` rows from the user's samples (weak priors).
- `LEARN` adds/strengthens rows from real polish→final diffs.
- `DISTILL` (weekly) promotes stable rules, surfaces `contested` ones, prunes noise.

Every row links back to `contexts/sessions/` so any claim is re-traceable to raw edits.
