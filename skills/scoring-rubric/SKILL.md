---
name: scoring-rubric
description: Use when the user asks to score, rank, or evaluate candidates, options, proposals, or responses against weighted criteria. Produces a scoring breakdown with weights, justifications, and confidence levels.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Scoring with a rubric

## When to use
Activates when the user wants something evaluated against criteria with weights — e.g. candidate screening, vendor comparison, proposal scoring, response ranking.

## Procedure
1. Define the criteria and weights explicitly. If the user did not provide them, propose a rubric first and state the weights sum to 100.
2. Evaluate each option against each criterion. Justify every score in one or two sentences.
3. Assess quality and relevance, not just keyword matches.
4. Include a confidence level (High/Medium/Low) for the overall assessment.
5. Rank the options and state the winner with reasoning.

## Rules
- Weights must be explicit and sum to 100.
- Every criterion score needs a justification tied to evidence.
- Use a worded scale where possible: Very Low / Low / Neutral / Good / Very Good (map to numbers internally if needed).
- Do not invent evidence. Mark unknowns as "Not found".

## Output format

```markdown
## Scoring: <subject>

| Criterion (weight) | Option A | Option B | Option C |
|---|---|---|---|
| <criterion> (<weight>) | <score> — <justification> | ... | ... |
| **Total (100)** | **<sum>** | **<sum>** | **<sum>** |

## Ranking
1. <winner> — <why>
2. <runner-up> — <why>

## Confidence
<High/Medium/Low> — <why>
```

## Example rubric (candidate screening)
- Technical skills — 40 points
- Experience — 30 points
- Education — 15 points
- Additional qualifications — 15 points
