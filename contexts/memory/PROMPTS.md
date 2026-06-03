# Prompt Templates

Copy-paste these to drive the loop. The agent reads `AGENTS.md` first every session, so you don't need to re-explain the system — just pick the right template.

---

## ONBOARD (run once, first time)

```
ONBOARD. Here are samples of my real writing. Read them, fill in rules/USER.md
with my basics, and seed the style profile (STYLE_PROFILE, LEXICON, scenarios)
with confidence=seed, evidence=0. Don't over-generalize — seed rules are weak
priors you won't apply silently. Store the samples verbatim in samples/.

[scenario: email_to_boss]
<paste a real email you wrote>

[scenario: team_chat]
<paste a few real chat messages>

[scenario: doc]
<paste a paragraph from a real doc>
```

---

## POLISH

```
POLISH [scenario: <email_to_boss | team_chat | doc>]

<paste your raw text>
```

The agent fixes correctness, applies your `established` style silently, flags any `tentative` guesses, and writes nothing. You then edit its output into your true final and send it back with LEARN.

(Scenario is optional — if you omit it, the agent infers and tells you.)

---

## LEARN

```
LEARN. Here is the final version I actually used. Diff it against your last
polish, learn from my edits, store the evidence, and update the profile.

<paste your final edited text>
```

The agent diffs polish→final, classifies each change (typo / its defect / style signal / revert), updates rules with the confidence math, writes the session evidence file, and tells you what it learned.

---

## DISTILL (weekly, or every ~10 sessions)

```
DISTILL. Recompute confidence for all rules, promote stable ones, resolve or
flag contested ones, prune stale one-offs, check whether diff_size is trending
down, and give me a short reflection.
```

---

## Tips

- Keep one POLISH and its LEARN in the **same session** so the agent still has the polish in context. If you come back later, paste the polish too.
- The more honestly you edit (toward how you'd *really* say it), the faster it learns your voice.
- When the agent flags a `tentative` guess and it's wrong, **say so** — a correction (especially a revert to your original) is the strongest signal it can get.
