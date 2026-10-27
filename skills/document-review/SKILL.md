---
name: document-review
description: Use when the user asks to review, audit, proofread, or validate a document, contract, PRD, proposal, or report for completeness, consistency, risks, or quality. Returns a prioritized issue list with suggested fixes.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Document review

## When to use
Activates when the user asks to review a document for quality, completeness, compliance, or consistency.

## Procedure
1. Read the target document in full.
2. Check it against the four review axes below.
3. Output the review in the format at the end of this skill.

## Review axes
- **Completeness**: missing sections, missing owners/dates/numbers, placeholders left in.
- **Consistency**: internal contradictions, terms used with two meanings, numbers that don't add up.
- **Clarity**: vague words ("interesting", "many", "better"), sentences over 30 words, missing context for a new reader.
- **Risks**: unverifiable claims, missing approvals, undefined responsibilities, irreversible commitments.

## Rules
- Use a decision-tree style for conditional rules (if/otherwise), never two contradictory instructions.
- Never invent facts. Mark unknown values as "Not found".
- Quote the exact passage you are flagging.
- Suggest a concrete rewrite or action for every issue.

## Output format

```markdown
## Review: <document name>

### Summary
<2-4 sentences: overall quality and the most critical issues>

### Critical issues (must fix before publishing)
- **[Issue type]** <location>: <what is wrong and why it matters>
  - Suggested fix: <concrete rewrite or action>

### Moderate issues (should fix)
- **[Issue type]** <location>: <what is wrong>
  - Suggested fix: <concrete rewrite or action>

### Minor issues (nice to fix)
- **[Issue type]** <location>: <what is wrong>
  - Suggested fix: <concrete rewrite or action>

### What's working well
- <positive observation>
```

Issue type labels: `[Missing section]`, `[Contradiction]`, `[Clarity]`, `[Accuracy]`, `[Risk]`, `[Format]`, `[Completeness]`.
