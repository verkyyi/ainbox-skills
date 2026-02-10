# Contributing to ainbox-skills

Thanks for your interest in improving aInbox's agent skills! This repo contains the prompts, templates, and tips that power AI email assistants at `@ainbox.io`.

## How to contribute

1. **Fork** this repository
2. **Create a branch** for your changes (`git checkout -b my-change`)
3. **Make your changes** (see guidelines below)
4. **Test** your changes with example emails (see `examples/` in each agent folder)
5. **Submit a pull request** to `main`

All changes require a pull request with at least one approval before merging.

## What you can contribute

| Area | Files | Examples |
|------|-------|---------|
| **Prompts** | `*/SKILL.md` | Improve accuracy, tone, or formatting |
| **Templates** | `*/assets/templates.json` | Better email wording, new language support |
| **Examples** | `*/examples/*.txt` | Add test cases for edge cases |
| **New agents** | New folder (e.g. `research/`) | Propose a new `@ainbox.io` agent skill |

## Agent folder structure

Every agent folder must contain these files:

```
my-agent/
├── SKILL.md              # YAML frontmatter (config) + markdown system prompt
├── assets/
│   └── templates.json    # Email reply templates (EN/ZH bilingual)
└── examples/             # Sample input/output pairs for testing
    └── *.txt
```

See `_template/` for an annotated scaffold with every field explained.

### SKILL.md frontmatter

The YAML frontmatter has these required sections:

| Section | Purpose |
|---------|---------|
| `name` | Must match the folder name and email prefix |
| `description` | One-sentence summary of the agent |
| `metadata` | `author` (your GitHub username) and `version` |
| `agent` | Public profile: `address`, `displayName`, `tagline`, `modes`, `capabilities` |
| `config` | Runtime settings — see [Config reference](README.md#config-reference) in the README |
| `variables` | Declares every `{variable}` your prompt and assets expect |

### templates.json

Must contain all five template keys:

| Key | Purpose | Shape |
|-----|---------|-------|
| `summary` | Primary output (wraps LLM response) | `{ "en": "...", "zh": "..." }` |
| `agent` | Direct email response | `{ "en": "...", "zh": "..." }` |
| `guidance` | Sent when mode is unclear | `{ "subject": { ... }, "body": { ... } }` |
| `error` | Sent on processing failure | `{ "body": { "en": "...", "zh": "..." } }` |
| `rateLimit` | Sent when daily limit is reached | `{ "subject": { ... }, "body": { ... } }` |

### examples/

Each `.txt` file contains a sample email input and expected output, separated by `--- Expected output ---`. These help reviewers validate prompt changes without sending real emails.

## Guidelines

- **Preserve `{variable}` placeholders** — the app depends on them at runtime. Removing or renaming a placeholder will break things. Check the `variables:` section in SKILL.md to see which variables the agent declares.
- **Keep prompts concise** — the LLM context window is shared with email content, so shorter prompts leave more room for the actual email.
- **Follow the [Agent Skills spec](https://agentskills.io/specification)** — `SKILL.md` files must have valid YAML frontmatter.
- **Support both EN and ZH** — templates are bilingual. If you update one language, please update the other too (or note in your PR that a translation is needed).
- **Update examples** — if your prompt change affects output format, update the relevant examples to match.
- **One change per PR** — keep pull requests focused. A prompt tweak and a new agent should be separate PRs.

## Creating a new agent

1. Copy `_template/` to a new folder matching your agent's email prefix (e.g. `research/`)
2. Fill in every field in `SKILL.md` — the template has inline comments explaining each one
3. Write your system prompt in the markdown body
4. Customize `templates.json` for your agent's purpose
5. Add 2-3 example emails in `examples/` covering the main modes
6. Update the `variables:` block to declare every `{variable}` your files use
7. Explain the agent's purpose and use case in your PR description

Use the existing `summary/` and `draft/` agents as real-world references.

## Reporting issues

If you find a problem but don't want to submit a fix, [open an issue](https://github.com/verkyyi/ainbox-skills/issues) describing:

- What the agent did
- What you expected
- The type of email that triggered the issue (no need to share actual content)

## Questions

If you have questions about contributing, open an issue or reach out via the feedback email in the app.
