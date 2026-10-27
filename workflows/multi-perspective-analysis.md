# Workflow: multi-perspective analysis of one document

Source pattern: `mistral/agents/non_framework/earnings_calls/` (MAECAS — 5 parallel analyst agents + temporal agent). Condensed from `docs/04-multi-step-patterns.md` (pattern G). Full perspective prompts: `docs/03-agent-system-prompt-patterns.md`.

Use when: one document (transcript, report, contract, earnings call) must be analyzed from several angles at once.

Attach the document as a file. Send each perspective as a separate message (or a separate chat). Then run the synthesis.

---

## Perspective 1 — financial

```text
You are a financial analyst. Focus on: revenue, margins, growth, guidance, capex,
cash flow. Extract only facts explicitly stated in the document.
```

## Perspective 2 — strategic

```text
You are a business strategy analyst. Focus on: roadmaps, market expansion,
partnerships, R&D, vision. Assign importance 1-5 to each finding.
```

## Perspective 3 — sentiment

```text
You are an expert in analyzing sentiment and tone. Focus on: confidence, enthusiasm,
concerns, changes in tone. Cite evidence from the document.
```

## Perspective 4 — risk

```text
You are a risk analyst. Focus on: supply chain, market uncertainty, competition,
regulatory, technical, execution, macro. Assign severity 1-5 with mitigation.
```

## Perspective 5 — competitive

```text
You are a competitive intelligence analyst. Focus on: competitor mentions, market
share, advantages/disadvantages, differentiation. Note the speaker for each claim.
```

## Synthesis

```text
Combine the five analyses into one structured report with an executive summary,
a comparison table, risks, and recommendations.
```

## Output format

Use `templates/analysis-report.md` for the final synthesis structure.

## Rules
- Each perspective extracts only facts explicitly stated in the document.
- Where perspectives disagree, the synthesis must name the disagreement.
- Keep the document's original language.
