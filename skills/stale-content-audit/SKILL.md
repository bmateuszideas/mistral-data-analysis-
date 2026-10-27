---
name: stale-content-audit
description: Use when the user asks to audit prompts, skills, documentation, or notes for outdated content — deprecated models, old SDK patterns, renamed products, or changed terminology. Returns a prioritized list of stale findings with references.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Stale content audit

## When to use
Activates when the user wants to check whether their AI-related content is out of date.

## Procedure
1. Read the target content in full.
2. Scan for the stale patterns in the table below.
3. For each finding, quote the exact text and give a reference (official docs) that confirms it is stale.
4. Output findings grouped by severity.

## Stale pattern table

| Pattern found | Issue | Replacement / note |
|---|---|---|
| `MistralClient`, `from mistralai.client import`, `ChatMessage(` | deprecated SDK v0 | current SDK: `Mistral(api_key=...)` |
| `client.chat(` without `.complete` | deprecated method | `client.chat.complete(...)` |
| `client.embeddings(` without `.create` | deprecated method | `client.embeddings.create(...)` |
| `client.fine_tuning`, `/v1/fine_tuning/` | deprecated API | fine-tuning API is deprecated |
| `mistral-tiny`, `mistral-medium` | deprecated model | current model list in docs |
| `open-mistral-7b`, `open-mixtral-8x7b`, `open-mixtral-8x22b` | verify model status | check current models overview |
| Model IDs with date suffix (`-2309`, `-2402`) | pinned old version | use `-latest` aliases |
| `Le Chat` as product name | renamed product | **Vibe** (since May 2026) |
| `MCP connectors` (referring to the product) | wrong product name | **Connectors** |
| `whitelist` / `blacklist`, `master` / `slave` | biased terminology | allowlist/denylist, primary/subordinate |

## Output format

```markdown
## Stale content report: <target>

### Critical (factually wrong / deprecated)
- <found text> — <why stale> — Reference: <link>

### Moderate (verify / likely outdated)
- <found text> — <why> — Reference: <link>

### Clean
- <what checked out fine>
```

## Rules
- Every finding must include a quoted passage from the target and a reference link.
- Methods with `_async` suffix (e.g. `create_async`) are valid — never flag them.
- When unsure whether something is stale, put it under Moderate, not Critical.
