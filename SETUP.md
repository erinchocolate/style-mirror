# SETUP — cold start

On day one the profile is empty. Applying any style rule would be a guess. Onboarding seeds a **weak prior** from your real writing so the agent isn't starting from zero — but real learning only happens later, from your edits.

## One-time onboarding

1. **Gather 3–8 samples of your own real writing.** Use text you actually wrote and were happy with — not text an AI wrote. Spread it across the scenarios you care about (e.g. one or two boss emails, a few chat messages, a doc paragraph).

2. **Run the `ONBOARD` template** from [contexts/memory/PROMPTS.md](contexts/memory/PROMPTS.md), pasting each sample under a `[scenario: ...]` tag.

3. **What the agent does:**
   - Stores your samples verbatim in `samples/YYYYMMDD_seed_samples.md` and indexes them.
   - Fills in [rules/USER.md](rules/USER.md) with your basics (native language, where your English goes, what bothers you about current AI polish).
   - Extracts conservative style observations across the 4 dimensions into STYLE_PROFILE / LEXICON / scenarios — **all marked `confidence = seed`, `evidence = 0`.**

## What "seed" means

Seed rules are **weak priors, not facts.** Per the protocol, the agent will **not** apply them silently. In early POLISH rounds it leans on standard, natural English and offers seed-derived ideas only as flagged suggestions. A seed rule that a real edit later confirms becomes `tentative` and joins the normal evidence curve toward `established`.

## Expectations for the first ~5–10 sessions

The agent should tell you, up front, that it's still learning your voice and that flagged items are guesses. **This is the point** — your corrections, especially when you revert to your original wording, are the highest-value signal in the whole system. Edit honestly toward how you'd really say it.

## You're ready when

- `samples/` has your seed file and a row in its INDEX.
- `rules/USER.md` is filled in.
- STYLE_PROFILE / LEXICON have some `seed` rows (or are honestly empty if your samples were too few to generalize).

Then start the real loop: `POLISH` → edit → `LEARN`. See [README.md](README.md) for the full cycle.
