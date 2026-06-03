# Workflow: Diff Learning (LEARN mode)

The core learning procedure. Runs when the user sends back their **final, edited** text. Turns the diff between your polish and their final into durable style knowledge.

## Inputs

- **v1** = your most recent POLISH output this session. (If not in context, ask the user to paste it.)
- **final** = the text the user actually used, pasted via the `LEARN` template.
- **scenario** = from the original POLISH, or re-inferred.

## Step 1 — Diff and classify

Compute the change set `v1 → final`. Classify **every** change into exactly one bucket:

| bucket | what it looks like | what to do |
|--------|--------------------|------------|
| **typo / spelling** | single-char fix, or a change that makes text *less* standard-correct | **ignore** — not a style signal |
| **your defect** | you introduced a wrong meaning / dropped info; user fixed it back to correct | log under §6 of the session file as a polish defect; **no** style rule |
| **style signal** | user swapped grammatically-fine text for a different but also-fine word / phrasing / structure / tone | **extract** — this is the gold |
| **revert** | user restored their *original raw* wording that you had changed | **extract, weight heavily** — you over-corrected away from their voice |

When unsure whether something is a style signal or a defect, ask yourself: *was my version actually wrong?* If yes → defect. If both versions are correct and the user still changed it → style.

## Step 2 — Map each signal to a dimension + id

| dimension | prefix | goes in |
|-----------|--------|---------|
| word substitution | `lx_` | LEXICON.md |
| phrasing habit (multi-word) | `ph_` | STYLE_PROFILE.md §A |
| sentence / structure | `st_` | STYLE_PROFILE.md §B |
| tone / formality | `tn_` | STYLE_PROFILE.md §C |

If the signal only holds for this scenario (not globally), put it in `scenarios/<scenario>.md` instead, with a scenario-flavored id (e.g. `tc_` for team_chat).

## Step 3 — Update the rule (confidence math)

For each style/revert signal, find an existing rule by `id`:

- **Exists, consistent** → `evidence += 1`; append this session id to its `sessions` list; recompute confidence.
- **Exists, contradicts** (final goes the *opposite* way of the rule) → set `confidence = contested`; stop applying it. Keep both sessions in the list as the conflicting evidence.
- **New** → create as `confidence = tentative`, `evidence = 1`, `sessions = [this session]`.

**Promotion check** (apply after every update):

```
if evidence >= 3 AND count(distinct days/sessions) >= 2:
    confidence = established
elif evidence >= 1:
    confidence = tentative   # unless contested
```

A `seed` rule confirmed by its first real diff becomes `tentative` and joins the normal curve. A single edit can never reach `established`.

## Step 4 — Write the evidence file

Create `contexts/sessions/YYYYMMDD_NNN_<scenario>.md` using the template below. Store the **verbatim** raw / v1 / final — this is the re-traceable record every rule links back to.

## Step 5 — Update the index

Append one row to `contexts/sessions/INDEX.md`:

```
| YYYY-MM-DD | s_YYYYMMDD_NNN | <scenario> | <comma-sep signal ids> | <diff_size> |
```

`diff_size` = a rough count of style+revert changes (ignore typos/defects). Tracking this over time shows whether your polish is converging on the user's voice (it should trend down).

## Step 6 — Cross-cutting note (optional)

If a signal is surprising, spans scenarios, or contradicts something you thought was settled, drop a 🔴/🟡/🟢 line in `contexts/memory/OBSERVATIONS.md`.

## Step 7 — Report to the user

One or two sentences, naming each rule and its new confidence. Example:
> Learned: you prefer **"want to"** over "would like to" (3rd time → now **established**). You dropped **"kindly"** again (still **tentative**, 2nd time). Logged as `s_20260603_001`.

---

## Session file template

```markdown
---
id: s_YYYYMMDD_NNN
scenario: <scenario>
created: YYYY-MM-DD
signals: [<id1>, <id2>, ...]      # ids this session created or strengthened
diff_size: <n>                     # count of style+revert changes
---

# Session YYYYMMDD_NNN — <scenario>

## 1. Raw input (user)
> <verbatim raw text>

## 2. Polish v1 (agent)
> <verbatim agent polish>

Style guesses applied this round (id — confidence at the time):
- <lx_xxx — established>
- <tn_yyy — tentative>

## 3. Final (user)
> <verbatim user final text>

## 4. Diff (v1 → final)
| # | Agent wrote | User changed to | type | verdict |
|---|-------------|-----------------|------|---------|
| 1 | "I would like to" | "I want to" | phrasing | style signal |
| 2 | "recieve" | "receive" | spelling | typo / ignore |
| 3 | "Please find attached" | "I've attached" | tone | style signal |

## 5. Extracted signals
- `lx_would_like` (word) — prefer "want to" over "would like to" → evidence +1 (now N, <confidence>)
- `tn_drop_corporate_boilerplate` (tone) — removes "please find attached" phrasing → new, tentative

## 6. Polish defects (if any)
- <e.g. "I changed 'Q3' to 'Q2' — meaning error; user fixed. Be careful with numbers.">

## 7. Notes
- <ambiguous calls, things to watch next time>
```
