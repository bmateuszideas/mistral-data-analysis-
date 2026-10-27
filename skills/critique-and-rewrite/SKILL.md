---
name: critique-and-rewrite
description: Use when the user asks to improve, refine, rewrite, or polish a text (article, email, report, proposal) against a style guide, tone, or quality bar. Produces a critique followed by the full rewritten text.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Critique and rewrite

## When to use
Activates when the user wants a text improved against a standard — a style guide, a tone of voice, a quality checklist, or general clarity/accuracy/elegance.

## Procedure
**Step 1 — Critique.** Identify the ten (or fewer) most significant concerns and, for each, give a concrete example of how to enhance it. If the user provided a style guide, critique strictly against that guide.

**Step 2 — Rewrite.** Incorporate the feedback and respond with the enhanced version of the full text. Do not alter the factual content — stylistic improvements only, unless the user asked otherwise.

## Output format

```markdown
## Critique
1. <concern> — Example: <quote> → Suggestion: <rewrite>
...

## Rewritten text
<the full enhanced text>
```

## Rules
- Provide the entire rewritten text, not a diff.
- Do not comment on the changes in the rewritten section.
- If the source is missing information the rewrite needs, mark it as [TBD] rather than inventing it.
- Keep the original language of the text unless asked to translate.
