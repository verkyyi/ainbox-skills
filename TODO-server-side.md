# Server-side changes needed

These improvements require coordinated changes to the aInbox platform (server) alongside this repo. They are documented here for future implementation.

## 1. Rename template key `summary` to `result`

**Problem:** Both agents use `"summary"` as the primary template key in `templates.json`, even though the draft agent produces drafts, not summaries. This confuses contributors who copy the pattern.

**Proposed change:**
- Rename the `"summary"` key in `templates.json` to `"result"` across all agents
- Update the server to read `"result"` instead of `"summary"` for the primary output template
- Consider supporting both keys during a transition period for backward compatibility

**Server impact:** The server reads `templates.summary` to wrap the LLM output. This lookup needs to change to `templates.result`.

## 2. Standardize template shapes

**Problem:** Template keys use three different shapes, making it hard for contributors to know which format to use:
- `summary`, `agent` → `{ "en": "...", "zh": "..." }` (flat bilingual string)
- `guidance`, `rateLimit` → `{ "subject": {...}, "body": {...} }` (nested with subject)
- `error` → `{ "body": {...} }` (nested, no subject)

**Proposed change:** Standardize all templates to the nested shape:
```json
{
  "result": {
    "subject": { "en": "...", "zh": "..." },
    "body": { "en": "...", "zh": "..." }
  }
}
```

For templates that don't need a custom subject (like the primary output), `subject` can be `null` or omitted, and the server uses a default.

**Server impact:** The server's template rendering logic needs to handle the unified shape for all template types.

## 3. Validate variable contracts at load time

**Problem:** The new `variables:` block in SKILL.md frontmatter declares which `{variable}` placeholders each agent expects, but the server doesn't yet validate this.

**Proposed change:** When the server loads an agent's SKILL.md:
1. Parse the `variables` block
2. Check that all declared variables are provided at runtime
3. Warn (or error) if a template references a `{variable}` not in the declared list
4. Warn if a declared variable is never used in any file

**Server impact:** Add a validation step to the agent loading pipeline.
