# style-mirror

A file-based, markdown-first project that **polishes your English while learning your personal voice**. There is no app, no database, no backend — just markdown files and an AI agent that reads and updates them through conversation. Structurally it mirrors `../context-infrastructure`.

## The idea

When AI polishes your English, the result often doesn't sound like *you*. And the most valuable signal — the edits **you** make on top of the AI's version — usually gets thrown away. style-mirror captures exactly that signal.

The highest-value data is not your raw text, and not your final text — it is the **diff between the agent's polish and your final version**. That diff is where you diverge from "standard / AI English." That divergence *is* your style.

## The loop

```
1. POLISH   You paste raw text (+ optional scenario).
            Agent fixes grammar, then applies your learned style.
            Uncertain style guesses are flagged, not imposed.

2. EDIT     You edit the agent's polish into your true final text.

3. LEARN    You send the final text back.
            Agent diffs (its polish → your final), extracts style
            signals, stores the raw evidence, and updates your
            style profile with confidence tracking.

4. NEXT     The next POLISH reads your profile first, so the output
            gets closer to your voice every time.
```

## How to use

All interaction is via copy-paste prompt templates in [contexts/memory/PROMPTS.md](contexts/memory/PROMPTS.md):

- **First time?** Run `ONBOARD` (see [SETUP.md](SETUP.md)) — paste a few samples of your real writing to seed the profile.
- **Polish something:** Use the `POLISH` template.
- **Teach it your edit:** Use the `LEARN` template with your final text.
- **Weekly cleanup:** Use the `DISTILL` template to promote stable rules and prune noise.

The agent's full operating protocol lives in [AGENTS.md](AGENTS.md) — it reads that at the start of every session.

## Why confidence matters

A single edit never changes the agent's behavior silently. A style rule must be seen **≥3 times across ≥2 sessions** before it gets applied without asking. One contradiction demotes it instantly. Slow to promote, fast to demote — this keeps the agent from overfitting to a one-off mood.

## Layout

| Path | What |
|------|------|
| [AGENTS.md](AGENTS.md) | The agent's per-session protocol (POLISH / LEARN modes). The heart. |
| [rules/style/](rules/style/) | The learned voice: STYLE_PROFILE, LEXICON, per-scenario overrides. |
| [rules/skills/](rules/skills/) | Detailed workflows the agent follows. |
| [contexts/sessions/](contexts/sessions/) | Raw, re-traceable before/after/final evidence. |
| [contexts/memory/](contexts/memory/) | Prompt templates + cross-session observations. |
| [samples/](samples/) | Your authentic writing samples (cold-start seed). |
