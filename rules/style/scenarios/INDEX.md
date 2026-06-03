# Scenarios Index

Per-scenario style overrides. Each file holds **only the rules that differ** from the baseline in `../STYLE_PROFILE.md` and `../LEXICON.md`. Everything not overridden is inherited.

When polishing, the user tags `[scenario: <name>]`, or you infer it. Load the baseline, then overlay the matching file (scenario rules win on conflict).

| scenario | file | when to use |
|----------|------|-------------|
| email_to_boss | [email_to_boss.md](email_to_boss.md) | updates / requests to a manager or senior |
| team_chat | [team_chat.md](team_chat.md) | casual messages to peers (Slack / Teams) |
| doc | [doc.md](doc.md) | written docs, reports, specs — broad audience |

**Adding a scenario:** copy an existing file, change the frontmatter `id` and the heading, start with an empty override table. Add a row here.
