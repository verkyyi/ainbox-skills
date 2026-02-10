# ainbox-skills

Agent Skills for [aInbox](https://ainbox.io) — community-editable prompts and templates for AI email assistants.

Built on the [Agent Skills](https://agentskills.io) open standard.

## Structure

Each folder maps to an email address receiver at `@ainbox.io`:

```
summary/              → summary@ainbox.io
├── SKILL.md          # System prompt + config (the AI's instructions)
├── assets/
│   └── templates.json  # Email reply templates (EN/ZH)
└── examples/           # Sample emails for testing changes
    └── *.txt
```

To create a new agent, copy `_template/` and follow the instructions inside. See [Contributing](#contributing).

## How it works

1. The aInbox app fetches `SKILL.md` and assets from this repo at startup
2. When you push changes to `main`, a GitHub webhook notifies the app to reload
3. The updated prompts and templates take effect immediately — no app restart needed

## SKILL.md

The `SKILL.md` file follows the [Agent Skills specification](https://agentskills.io/specification):

- **YAML frontmatter**: `name`, `description`, `metadata`, and `config`
- **Markdown body**: The system prompt passed directly to the LLM

The prompt and templates contain `{placeholder}` strings that are interpolated at runtime (see below).

### Frontmatter sections

| Section | Purpose |
|---------|---------|
| `name` | Agent identifier — must match the folder name and email prefix |
| `description` | One-sentence description of the agent |
| `metadata` | `author` and `version` |
| `config` | Runtime configuration consumed by the platform (see below) |

## Config reference

The `config:` block only needs fields that differ from server defaults. Omitted fields use the defaults shown below. `fromEmail` and `fromName` are derived from the agent's folder name (e.g. `draft/` → `draft@ainbox.io`, `draft`).

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `openaiModel` | string | `gpt-5` | LLM model ID passed to the API |
| `maxCompletionTokens` | int | `4096` | Max tokens the LLM may generate per reply |
| `temperature` | float\|null | `null` | Sampling temperature (`null` = model default) |
| `maxBodyChars` | int | `24000` | Truncate inbound email body beyond this character limit |
| `dailyLimit` | int | `50` | Max requests per sender per calendar day (UTC) |
| `fromEmail` | string | `{name}@ainbox.io` | Envelope-from address on outbound replies |
| `fromName` | string | `{name}` | Display name on outbound replies |
| `feedbackEmail` | string | `feedback@ainbox.io` | Shown in error and rate-limit templates |
| `fallbackForwardTo` | string | `""` | Forward unprocessable emails to this address (empty = discard) |
| `autoReplyGuidance` | bool | `true` | Send a guidance reply when the agent can't determine the mode |
| `allowedSenders` | string[] | `[]` | Allowlist of sender addresses (empty = allow everyone) |
| `blockedSenders` | string[] | `[]` | Blocklist of sender addresses (checked before allowlist) |
| `senderLimitOverrides` | object | `{}` | Per-sender daily limit overrides, e.g. `{"vip@co.com": 100}` |
| `dryRun` | bool | `false` | `true` = log replies but don't actually send them |

## Runtime variables

These `{placeholder}` strings are replaced by the aInbox app at runtime:

### In SKILL.md (system prompt)

| Variable | Description |
|----------|-------------|
| `{dailyLimit}` | Daily quota per sender (e.g. `50`) |

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
| `{feedbackEmail}` | error.body, rateLimit.body | Feedback email address |

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for full details.

Quick start:
1. Copy `_template/` to a new folder named after your agent's email prefix
2. Fill in `SKILL.md` and `templates.json`
3. Add example emails in `examples/` so reviewers can test
4. Submit a PR

## License

[Apache-2.0](LICENSE)
