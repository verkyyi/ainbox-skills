# ainbox-skills

Agent Skills for [aInbox](https://ainbox.io) — community-editable prompts and templates for AI email assistants.

Built on the [Agent Skills](https://agentskills.io) open standard.

## Structure

Each folder maps to an email address receiver at `@ainbox.io`:

```
summary/              → summary@ainbox.io
├── SKILL.md          # System prompt (the AI's instructions)
└── assets/
    ├── templates.json  # Email reply templates (EN/ZH)
    └── tips.json       # Rotating footer tips (EN/ZH)
```

Future agents follow the same pattern — `research/` → `research@ainbox.io`, etc.

## How it works

1. The aInbox app fetches `SKILL.md` and assets from this repo at startup
2. When you push changes to `main`, a GitHub webhook notifies the app to reload
3. The updated prompts and templates take effect immediately — no app restart needed

## SKILL.md

The `SKILL.md` file follows the [Agent Skills specification](https://agentskills.io/specification):

- **YAML frontmatter**: `name`, `description`, and `metadata`
- **Markdown body**: The system prompt passed directly to the LLM

The prompt contains `{variable}` placeholders that are interpolated at runtime (see below).

## Runtime variables

These placeholders are replaced by the aInbox app at runtime:

### In SKILL.md (system prompt)

| Variable | Description |
|----------|-------------|
| `{dailyLimit}` | Daily summary quota per sender (e.g. `50`) |

### In templates.json

| Variable | Where | Description |
|----------|-------|-------------|
| `{greeting}` | all templates | Personalized greeting (e.g. "Hi John," or "Hi,") |
| `{summary}` | summary, agent | LLM-generated summary or response text |
| `{tip}` | summary, agent | Rotating footer tip (from tips.json) |
| `{originalSubject}` | guidance.subject | Original email subject line |
| `{limit}` | rateLimit.body | Total daily limit number |
| `{hoursLeft}` | rateLimit.body | Hours until daily limit resets |
| `{refSection}` | rateLimit.body | Referral promotion section (or empty) |
| `{feedbackEmail}` | rateLimit.body | Feedback email address |

### In tips.json

| Variable | Description |
|----------|-------------|
| `{dailyLimit}` | Daily summary quota per sender |
| `{remaining}` | Remaining summaries for the sender today |
| `{limit}` | Total daily limit number |
| `{referralCode}` | Sender's referral code |
| `{referralBonus}` | Bonus summaries per successful referral |
| `{feedbackEmail}` | Feedback email address |

## Contributing

Contributions are welcome! You can help improve:

- **Prompts** — make summaries more accurate, concise, or useful
- **Templates** — improve email formatting and wording
- **Tips** — add helpful tips for users
- **Translations** — add or improve language support

### Guidelines

- Keep prompts focused and concise — the LLM context window is shared with email content
- Test your changes with real emails before submitting a PR
- Preserve all `{variable}` placeholders — the app depends on them
- Follow the [Agent Skills specification](https://agentskills.io/specification) for SKILL.md format

## License

[Apache-2.0](LICENSE)
