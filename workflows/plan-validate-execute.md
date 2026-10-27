# Workflow: plan → validate → execute

Source patterns: agentskills.io best practices ("plan-validate-execute" for batch or destructive operations); `docs/04-multi-step-patterns.md` (pattern D).

Use when: the task is risky or multi-object — changes across many records, migrations, sends, publications, anything where a wrong step is hard to undo.

---

## Step 1 — plan

```text
Analyze the request and produce a structured plan as a numbered list.
For each step include: action, target, and expected result.
Do not execute anything yet.
```

## Step 2 — validate (user or model-as-judge)

```text
Check the plan against these rules:
- Does every step have a clear expected result?
- Are there any steps that could destroy or modify data irreversibly? Flag them.
- Is the order of steps correct (dependencies respected)?
```

Present the plan to the user. Wait for approval before step 3.

## Step 3 — execute (only after approval)

```text
Execute the approved plan step by step. After each step, report the result.
Stop and ask before any irreversible action.
```

## Rules
- Never execute in the same message as the plan.
- Irreversible steps (delete, send, publish, overwrite) always require an explicit user confirmation.
- If execution deviates from the plan, stop and re-plan.
