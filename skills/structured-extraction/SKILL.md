---
name: structured-extraction
description: Use when the user asks to extract structured data (JSON, tables, fields) from a document, email, invoice, contract, resume, form, or free text. Returns data in an exact schema, with null for missing values.
license: Apache-2.0
metadata:
  author: mistral-data-analysis
  version: "1.0"
---

# Structured extraction

## When to use
Activates when the user wants data pulled out of unstructured or semi-structured content into a defined structure.

## Procedure
1. Confirm the target schema. If the user did not provide one, propose one first (field names, types, required/optional) and ask for confirmation only if the task is large.
2. Extract strictly from the source. Do not invent values.
3. For every extracted value that is a fact (date, amount, name, quote), include a short verbatim `source_quote` from the document.
4. Missing values become `null` (or "Not found" in prose output).

## Rules
- Respond with JSON only when a schema was given or proposed. No explanations outside the JSON.
- Never normalize silently: keep the original wording in `source_quote`, put your normalized value in the target field.
- If the document is ambiguous about a field, set it to null and add an entry to an `ambiguities` array: `{"field": "...", "reason": "..."}`.

## Default output shape

```json
{
  "data": { "<schema fields>": "<values>" },
  "ambiguities": [{"field": "...", "reason": "..."}]
}
```

## Example schema (contract)

```json
{
  "parties": [{"name": "...", "role": "..."}],
  "dates": {"signed": "YYYY-MM-DD|null", "renewal": "YYYY-MM-DD|null", "termination_notice_days": "number|null"},
  "money": [{"label": "...", "amount": "number|null", "currency": "..."}],
  "liability_cap": "string|null",
  "termination_rights": ["..."],
  "missing_approvals": ["..."]
}
```
