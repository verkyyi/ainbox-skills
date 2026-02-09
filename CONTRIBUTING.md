# Contributing to ainbox-skills

Thanks for your interest in improving aInbox's agent skills! This repo contains the prompts, templates, and tips that power AI email assistants at `@ainbox.io`.

## How to contribute

1. **Fork** this repository
2. **Create a branch** for your changes (`git checkout -b my-change`)
3. **Make your changes** (see guidelines below)
4. **Test** your changes with real emails if possible
5. **Submit a pull request** to `main`

All changes require a pull request with at least one approval before merging.

## What you can contribute

| Area | Files | Examples |
|------|-------|---------|
| **Prompts** | `*/SKILL.md` | Improve summary accuracy, tone, or formatting |
| **Templates** | `*/assets/templates.json` | Better email wording, new language support |
| **Tips** | `*/assets/tips.json` | Add helpful tips shown to users |
| **New agents** | New folder (e.g. `research/`) | Propose a new `@ainbox.io` agent skill |

## Guidelines

- **Preserve `{variable}` placeholders** — the app depends on them at runtime. Removing or renaming a placeholder will break things.
- **Keep prompts concise** — the LLM context window is shared with email content, so shorter prompts leave more room for the actual email.
- **Follow the [Agent Skills spec](https://agentskills.io/specification)** — `SKILL.md` files must have valid YAML frontmatter with `name`, `description`, and `metadata`.
- **Support both EN and ZH** — templates and tips are bilingual. If you update one language, please update the other too (or note in your PR that a translation is needed).
- **One change per PR** — keep pull requests focused. A prompt tweak and a new agent should be separate PRs.

## Proposing a new agent

To propose a new agent (e.g. `research@ainbox.io`):

1. Create a new folder matching the email prefix (e.g. `research/`)
2. Add a `SKILL.md` with frontmatter and system prompt
3. Add an `assets/` folder with `templates.json` and `tips.json`
4. Follow the structure of the existing `summary/` agent as a reference
5. Explain the agent's purpose and use case in your PR description

## Reporting issues

If you find a problem but don't want to submit a fix, [open an issue](https://github.com/verkyyi/ainbox-skills/issues) describing:

- What the agent did
- What you expected
- The type of email that triggered the issue (no need to share actual content)

## Questions

If you have questions about contributing, open an issue or reach out via the feedback email in the app.
