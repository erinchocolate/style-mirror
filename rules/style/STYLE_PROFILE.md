---
id: style_profile
category: style
created: 2026-06-03
updated: 2026-06-03
---

# STYLE_PROFILE — the user's learned voice

> **Confidence legend:** `seed` < `tentative` < `established` < (sidelined) `contested`.
> Apply `established` silently. Apply `tentative` but flag it. Do **not** auto-apply `seed` or `contested`.
> **Promotion:** `evidence >= 3 AND distinct_sessions >= 2` → `established`. A single edit can never reach `established`.
> Slow to promote, fast to demote: any contradiction drops a rule a level and flags `contested`.
> `evidence` = number of supporting observations. `sessions` = the session ids that produced them.

Word-level substitutions live in [LEXICON.md](LEXICON.md). This file holds higher-order habits.

---

## Dimension A — Word / phrasing preferences

Phrasing habits above the single-word level (multi-word constructions, openers, hedges). Single words → LEXICON.

| id | rule | confidence | evidence | sessions |
|----|------|-----------|----------|----------|
| _(none yet — populated by ONBOARD as seed, then by LEARN)_ | | | | |

## Dimension B — Sentence & structure

Sentence length, active vs passive, how clauses connect, paragraph shape.

| id | rule | confidence | evidence | sessions |
|----|------|-----------|----------|----------|
| _(none yet)_ | | | | |

## Dimension C — Tone & formality

Formal vs casual, direct vs hedged, warmth, openers/sign-offs, emoji, exclamation.

| id | rule | confidence | evidence | sessions |
|----|------|-----------|----------|----------|
| _(none yet)_ | | | | |

## Dimension D — Scenario differentiation

Scenario-specific overrides live in [scenarios/](scenarios/). This table only indexes which scenarios exist and their dominant delta vs. the baseline above.

| scenario | file | dominant override vs. baseline |
|----------|------|-------------------------------|
| email_to_boss | [scenarios/email_to_boss.md](scenarios/email_to_boss.md) | _(tbd)_ |
| team_chat | [scenarios/team_chat.md](scenarios/team_chat.md) | _(tbd)_ |
| doc | [scenarios/doc.md](scenarios/doc.md) | _(tbd)_ |

---

### Example row (for reference — delete once real rows exist)

```
| ph_want_over_would_like | prefers "I want to" over "I would like to" | established | 4 | s_20260603_001, s_20260605_002, s_20260609_001 |
```
