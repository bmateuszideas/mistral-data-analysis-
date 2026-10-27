---
name: research-brief
description: Use when the user asks for research on a topic, a market or technical landscape analysis, a comparison of options, or a source-backed brief with recommendations. Produces a structured brief with cited sources and a recommendation.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Research brief

## When to use
Activates when the user asks for research, a landscape analysis, or a comparison that needs current or sourced information.

## Procedure
1. Decompose the topic into 3-5 sub-questions.
2. Answer each sub-question separately, using web search and any attached files.
3. Synthesize the findings into the structured brief below.
4. For every factual claim, cite the source. Mark anything you could not verify as "Unverified".
5. Rate confidence per finding: High / Medium / Low.

## Format

```markdown
# Brief: <topic>

## Executive summary
<max 100 words>

## Key findings
### <sub-question 1>
<finding> — Source: <citation> — Confidence: <High/Medium/Low>

### <sub-question 2>
...

## Comparison table (when comparing options)
| Criterion | Option A | Option B | Option C |
|---|---|---|---|
| <criterion> | | | |

## Risks and open questions
- <risk or question>

## Recommendation
<one clear recommendation with reasoning>
```

## Rules
- Prefer official documentation over blog posts for technical claims.
- Prefer recent sources (state the date) for anything that changes over time.
- Never present an unverified claim as fact.
- If sources contradict each other, say so explicitly and explain which you trust more and why.
