---
name: code-review
description: Use when the user asks to review, audit, or improve code, a pull request, a diff, or a code snippet for bugs, security issues, readability, or best practices. Returns findings grouped by severity with concrete fixes.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Code review

## When to use
Activates when the user shares code and asks for review, feedback, or a security/quality check.

## Procedure
1. Read the code and understand its purpose before judging it.
2. Check, in order: correctness/bugs → security → readability/maintainability → performance (only if relevant) → tests.
3. For each finding, quote the exact lines and propose a concrete fix.

## Review checklist
- [ ] Correctness: edge cases, off-by-one, null/undefined handling, error paths.
- [ ] Security: injection (SQL, shell, template), secrets in code, unsafe deserialization, missing auth checks, sensitive data in logs.
- [ ] Readability: naming, function length, dead code, misleading comments.
- [ ] Maintainability: coupling, duplication, missing type annotations where the project uses them.
- [ ] Tests: are the risky paths covered?

## Output format

```markdown
## Summary
<2-3 sentences: overall impression and the most important finding>

## Critical (fix before merge)
- <file:line> — <issue> — Fix: <concrete change>

## Important (should fix)
- <file:line> — <issue> — Fix: <concrete change>

## Suggestions (nice to have)
- <file:line> — <suggestion>

## What's good
- <positive observation>
```

## Rules
- Be specific: file, line, what is wrong, why it matters, how to fix.
- Don't nitpick style the project's linter already enforces.
- If the code is fine, say so — don't invent issues.
- Match the project's existing conventions (naming, structure) instead of imposing external ones.
