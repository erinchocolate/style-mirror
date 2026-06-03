---
id: lexicon
category: style
created: 2026-06-03
updated: 2026-06-03
---

# LEXICON — avoid → prefer substitution table

> Word choice is the most directly actionable style signal, so it gets its own high-density table.
> Same confidence model as [STYLE_PROFILE.md](STYLE_PROFILE.md): apply `established` silently, flag `tentative`, do not auto-apply `seed`/`contested`.
> **`scenario` column:** blank = global; otherwise the substitution applies only in that scenario.

| id | avoid | prefer | scenario | confidence | evidence | sessions |
|----|-------|--------|----------|-----------|----------|----------|
| _(none yet — populated by ONBOARD as seed, then by LEARN)_ | | | | | | |

---

### Example rows (for reference — delete once real rows exist)

```
| lx_utilize    | utilize          | use           |               | established | 4 | s_20260603_001, s_20260606_001 |
| lx_would_like | I would like to  | I want to     |               | established | 3 | s_20260603_001, ... |
| lx_kindly     | kindly           | please / (drop) | email_to_boss | tentative   | 1 | s_20260603_001 |
| lx_leverage   | leverage         | use / build on |               | contested   | 2 | s_20260604_001, s_20260611_001 |
```

> Note the `contested` example: the user once kept "leverage" and once removed it — conflicting evidence, so it is sidelined until the tie breaks.
