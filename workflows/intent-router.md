# Workflow: intent router

Source pattern: router/triage agents from the cookbook (travel assistant triage, support triage). Condensed from `docs/04-multi-step-patterns.md` (pattern C). Full bank classifier: `prompts/tasks/classify-intent.md`.

Use when: one entry point, multiple possible intents, and each intent has its own procedure (a skill or a prompt).

In Vibe Work the router step is often built in — Work recognizes the intent. Use this workflow explicitly when you need deterministic routing, e.g. inside a custom Skill or a Project with many procedures.

---

## Step 1 — classify

```text
Step 1: Categorize the user's request into exactly one of:
- summary: user wants a summary of provided material
- extraction: user wants structured data extracted from provided material
- analysis: user wants analysis, comparison, or evaluation
- writing: user wants new content written or rewritten
- research: user wants current information from the web
- other: none of the above

Respond with the category only.
```

## Step 2 — execute the matching path

In the next message, execute the task according to the category, using the matching procedure:

| Category | Procedure |
|---|---|
| summary | `prompts/tasks/summarize-document.md` or `@meeting-summary` |
| extraction | `skills/structured-extraction/SKILL.md` or `prompts/tasks/extract-structured-data.md` |
| analysis | `skills/scoring-rubric/SKILL.md`, `workflows/multi-perspective-analysis.md`, or `prompts/tasks/evaluate-output.md` |
| writing | `skills/critique-and-rewrite/SKILL.md` or `workflows/critique-refine-loop.md` |
| research | `skills/research-brief/SKILL.md` or `workflows/orchestrator-parallel-synthesis.md` |
| other | answer directly, ask one clarifying question |

## Rules
- One category per request. If two categories fit, pick the dominant one and note the secondary.
- Never execute a procedure before the category is decided.
- The category decision must not leak into the user-facing answer.
