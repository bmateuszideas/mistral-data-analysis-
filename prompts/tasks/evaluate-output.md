# Task prompt: evaluate an output (LLM-as-judge)

Source: `mistral/evaluation/RAG_evaluation.ipynb`, `mistral/prompting/prompting_capabilities.ipynb` (Evaluation). Full context: `docs/08`.

## Judge with three criteria (context relevance, answer relevance, groundedness)

```text
You are an impartial evaluator. Evaluate the answer against the context and the original
question using these three criteria. For each criterion, provide:
- explanation: step-by-step reasoning
- score: a string between "0" and "3"

SCALE
0: No relevance/Not grounded/Irrelevant — completely unrelated or not based on the context.
1: Low relevance — minimal relevance or grounding.
2: Medium relevance — somewhat relevant or grounded.
3: High relevance/High groundedness — highly relevant or grounded.

CRITERIA
1. context_relevance: how well the retrieved context aligns with the user's query.
2. answer_relevance: how well the answer addresses the user's original query.
3. groundedness: how faithful the answer is to the retrieved context.

QUESTION:
{question}

CONTEXT:
{context / sources}

ANSWER TO EVALUATE:
{answer}

Respond with a JSON object:
{
  "context_relevance": {"explanation": "...", "score": "0-3"},
  "answer_relevance":  {"explanation": "...", "score": "0-3"},
  "groundedness":      {"explanation": "...", "score": "0-3"}
}
```

## Pick-the-best variant (compare N outputs)

```text
You are given {N} candidate outputs for the same task. Evaluate them and rate which one
is the best. Explain your choice by pointing out specific reasons such as clarity,
completeness, and relevance to the task.

TASK:
{task}

CANDIDATE OUTPUTS:
{outputs}
```

## Devil's advocate variant (mirrors built-in `challenge-my-thinking`)

```text
Act as a devil's advocate. Pressure-test the following plan or answer:
1. List the 3 weakest assumptions.
2. List what could go wrong and the impact.
3. Suggest what information is missing.
4. Propose one concrete improvement.

PLAN/ANSWER:
{text}
```
