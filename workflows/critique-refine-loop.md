# Workflow: critique and refine loop

Source pattern: `mistral/data_generation/data_generation_refining_news.ipynb`. Condensed from `docs/04-multi-step-patterns.md` (pattern A3) and `docs/08-evaluation-and-quality.md`.

Use when: the user wants a text improved against a style guide, tone, or quality bar — an article, email, report, or proposal. Also the base loop for the `critique-and-rewrite` skill.

Two steps in one chat. Step 2 consumes both the original text and the critique.

---

## Step 1 — critique against the style guide

```text
As a 'News Article Editor' adhering to a specific style guide, your responsibility is to
polish and restructure news articles to align with the high standards of clarity, accuracy,
and elegance set by the guide:

{paste style guide}

You are presented with a news article. Identify the ten (or fewer) most significant
stylistic concerns and provide examples of how they can be enhanced.

ARTICLE:
{paste article}
```

## Step 2 — rewrite with the feedback applied

```text
Now, incorporate the feedback and critiques into the news article and respond with the
enhanced version, focusing solely on stylistic improvements without altering the content.
You must provide the entire article enhanced.
Do not make ANY comments, only provide the new article improved.
Do not tell me what you changed.

ARTICLE:
{original article}

FEEDBACK:
{critique from step 1}
```

## Optional step 3 — re-check

```text
Review the enhanced article against the same style guide. List any remaining violations.
If none remain, respond with "Clean."
```

## Rules
- Style-only changes in step 2: facts, numbers, and quotes must not change.
- If the critique contradicts the style guide, the style guide wins.
- Maximum 2-3 loop iterations; then deliver the best version with a note on remaining concerns.
- Keep the original language of the text.
