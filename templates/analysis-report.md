# Template: analysis report (multi-perspective synthesis)

Used by: `workflows/multi-perspective-analysis.md`, `workflows/orchestrator-parallel-synthesis.md`

```markdown
# Analysis: <subject>

## Executive summary
<max 100 words, covering the main conclusions from all perspectives>

## Findings by perspective
### Financial
- <finding>

### Strategic
- <finding>

### Sentiment
- <finding>

### Risk
- <finding> — Severity: <1-5> — Mitigation: <mitigation>

### Competitive
- <finding> — Speaker: <who said it>

## Comparison table
| Aspect | Finding A | Finding B | ... |
|---|---|---|---|
| <aspect> | | | |

## Contradictions between perspectives
- <contradiction> — Resolution: <which perspective is better supported and why>

## Risks
- <risk> — Severity: <1-5>

## Recommendations
1. <recommendation>
2. <recommendation>
```

Rules: every finding must trace back to a partial analysis — do not add information that is absent from the inputs. Name contradictions explicitly instead of hiding them.
