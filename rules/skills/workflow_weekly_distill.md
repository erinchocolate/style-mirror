# Workflow: Weekly Distill

A periodic (weekly, or every ~10 sessions) cleanup pass. LEARN mode keeps adding rows fast; DISTILL keeps the profile honest, promotes what's stable, and surfaces conflicts. Triggered by the `DISTILL` prompt template.

## Step 1 — Recompute confidence from evidence

For every rule in STYLE_PROFILE, LEXICON, and scenario files, re-derive confidence from its `evidence` and `sessions` (don't trust the stored label — recompute it):

```
established  if evidence >= 3 AND distinct_sessions >= 2 AND no contradiction
contested    if there is supporting AND contradicting evidence
tentative    if evidence >= 1
seed         if evidence == 0 (sample-derived only, never confirmed by a diff)
```

Fix any label that drifted. Report what changed (promotions, demotions).

## Step 2 — Resolve contested rules

For each `contested` rule, look at the conflicting sessions:
- If one side now clearly outweighs the other (e.g. 4 vs 1), resolve to that side and set the appropriate confidence.
- If it's genuinely scenario-dependent, **split** it: move the variants into the relevant `scenarios/` files and remove the global contested rule.
- If still a true tie, leave it `contested` and note it for the user to settle.

## Step 3 — Prune noise

- Drop `tentative` rules with `evidence == 1` that are **older than ~30 days** and have never recurred — they were probably one-off moods, not style. (Keep the session evidence file; only remove the rule row.)
- Merge near-duplicate rules into one, summing evidence and unioning session links.

## Step 4 — Update the dimension-D index & scenario deltas

Refresh the "dominant override" column in STYLE_PROFILE §D and the scenario INDEX so the at-a-glance summaries match the current rules.

## Step 5 — Trend check (is learning working?)

Scan `contexts/sessions/INDEX.md`:
- Is `diff_size` trending **down** over time? → your polish is converging on the user's voice. Good.
- Flat or rising? → note it in OBSERVATIONS as 🟡; maybe rules aren't being applied, or the user's needs shifted.

## Step 6 — Reflect

Append a short weekly reflection to `contexts/memory/OBSERVATIONS.md`: what got promoted, what's contested, and one observation about the user's evolving voice. Keep it to a few lines.

## Step 7 — Report

Tell the user: N rules promoted to established, M contested needing their call, K pruned, and the diff-size trend.
