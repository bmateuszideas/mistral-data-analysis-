---
name: meeting-summary
description: Use when the user pastes meeting notes, a call transcript, or a recording summary and asks for a summary, action items, or follow-ups. Produces an executive summary, action items with owners, and open questions.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Meeting summary

## When to use
Activates when the user provides meeting notes or a transcript and wants a structured summary.

## Procedure
When given meeting notes or a transcript, produce:

1. A one-paragraph executive summary (max 80 words).
2. Action items as a bulleted list with owners and due dates if available.
3. A list of open questions and decisions needed.

## Format

```markdown
## Executive summary
<max 80 words>

## Action items
- [ ] <action> — Owner: <name or [TBD]> — Due: <date or [TBD]>

## Open questions
- <question or decision needed>
```

## Rules
- Tone: professional, concise.
- Don't invent information that isn't in the source.
- If owners or dates are missing, mark them as [TBD].
- Do not add commentary about the meeting quality unless asked.
