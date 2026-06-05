# Workflow: Polish (POLISH mode)

Runs when the user pastes **raw** text to be polished. Produces a corrected version that already leans toward their learned voice — without imposing low-confidence guesses or inventing content.

## Step 1 — Determine the scenario

- If tagged `[scenario: <name>]`, use it.
- Otherwise infer from content and audience, and **state the inference** in Notes.
- Map to a file in `rules/style/scenarios/`. Unknown scenario → use baseline only, and note it.

## Step 2 — Load the style layers

1. Baseline: `rules/style/STYLE_PROFILE.md` + `rules/style/LEXICON.md`.
2. Overlay: the matching `rules/style/scenarios/<scenario>.md` (scenario rules win on conflict).
3. Note each rule's confidence — it decides whether you apply it (next step).

## Step 3 — Fix correctness first

Grammar, spelling, agreement, genuinely unnatural phrasing. **Non-negotiable and comes before any style shaping.** A correct sentence that isn't yet "in voice" is fine; a stylish sentence that's wrong is not.

## Step 4 — Apply style by confidence

| confidence | action |
|------------|--------|
| `established` | apply **silently** |
| `tentative` | apply, but **flag** in Notes ("guessing from past edits; change if wrong") |
| `seed` | do **not** impose; at most offer as a flagged suggestion |
| `contested` | do **not** apply; keep the wording closest to the user's raw input |

Hard preferences stated explicitly in `rules/USER.md` may be applied with high confidence regardless of the evidence curve.

## Step 5 — Preserve meaning

Never add information, claims, hedges, or pleasantries the user didn't write. Polish only what's there. Prefer the **smallest** change that makes the text right and in-voice. If meaning is unclear, **ask before polishing**.

The steps above produce the **default polish** — a faithful mirror. The next step is a *separate* teaching layer that never touches it.

## Step 5.5 — Offer learning suggestions (optional)

Spot **1–3** places where a more natural / more native speaker would phrase the same idea differently, and present them as a separate block so the user can learn and adopt at will.

Guardrails:
- **Only if genuinely useful.** If the text already reads naturally, skip this entirely.
- **Preserve meaning.** Each suggestion is an alternative *phrasing* of what they already said — never new content, never a fancier word for its own sake, never generic/corporate.
- **Never auto-apply.** Suggestions stay out of the Polished text and out of every profile file. They are the user's to take or ignore.

## Step 6 — Output

```
**Polished:**
> <the polished text>

**💡 更地道的说法 (learn — optional):**
- "<your phrase>" → "<more native phrasing>" — <3–8 word why>

**Notes:** (ONLY if you applied a tentative guess or inferred the scenario)
- (tentative) used "Hi" not "Dear" — guessing from past chats; change it if wrong
- inferred scenario = email_to_boss
```

Omit the **💡 block** when there is nothing worth teaching. Omit **Notes** when everything applied was `established` or pure correctness.

## Step 7 — Write nothing

POLISH mode never touches files. The user will edit your output and send it back; learning happens then, in LEARN mode (`workflow_diff_learning.md`).

## Cold-start behavior

If the profile is empty or only `seed` rules exist: lean on standard, natural English, offer seed ideas only as flagged suggestions, and tell the user you're still learning their voice. Invite corrections — their reverts are the most valuable signal you can get.
