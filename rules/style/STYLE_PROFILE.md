---
id: style_profile
category: style
created: 2026-06-03
updated: 2026-06-05
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
| ph_explicit_place | names the location explicitly ("at the show") rather than compressing to a pronoun ("there") | tentative | 1 | s_20260604_001 |
| ph_not_only_position | in the "not only … but also …" correlative, places "not only" **before** the verb ("not only built X but also Y") rather than after it ("built not only X but also Y") | tentative | 1 | s_20260605_003 |

## Dimension B — Sentence & structure

Sentence length, active vs passive, how clauses connect, paragraph shape.

| id | rule | confidence | evidence | sessions |
|----|------|-----------|----------|----------|
| st_no_comma_before_conj | omits the comma before a coordinating conjunction ("and"/"so") **joining two independent clauses** (e.g. "goals and studied", "AI agents and shared", "impaired so when", "chat window so my focus"). Scope bound: does **not** extend to serial/list commas — user kept the Oxford comma in "hackathons, workshops, and training". | established | 5 | s_20260604_001, s_20260605_002, s_20260605_003 |

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
