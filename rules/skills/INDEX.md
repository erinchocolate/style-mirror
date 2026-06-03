# Skills Index

Reusable workflows the agent follows. When you're about to act, find the matching procedure here first.

## Workflows

- [workflow_polish.md](workflow_polish.md) ✅ — **POLISH mode.** Fix correctness, then apply learned style by confidence, without imposing guesses or inventing content. Writes nothing.
- [workflow_diff_learning.md](workflow_diff_learning.md) ✅ — **LEARN mode (core).** Diff your polish vs the user's final, classify each change, update rules with the confidence math, write the session evidence file. Includes the session-file template.
- [workflow_weekly_distill.md](workflow_weekly_distill.md) ✅ — **DISTILL (periodic).** Recompute confidence, resolve `contested`, prune noise, check the diff-size trend, reflect.

## The two-mode contract

| You receive… | Mode | Skill |
|--------------|------|-------|
| raw text to polish | POLISH | workflow_polish |
| the user's final/edited text | LEARN | workflow_diff_learning |
| a request to clean up the profile | DISTILL | workflow_weekly_distill |

POLISH never writes. LEARN and DISTILL are the only writers of style rules.
