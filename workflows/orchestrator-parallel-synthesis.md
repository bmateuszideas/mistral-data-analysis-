# Workflow: orchestrator → parallel workers → synthesis

Source pattern: `mistral/agents/non_framework/agentic_workflows/parallel_subtask_agentic_workflow.ipynb`. Condensed from `docs/04-multi-step-patterns.md` (pattern B).

Use when: a task decomposes into independent parts — analyzing one document from several perspectives, multi-thread research, reviewing code from several angles.

In web chat: step 1 in one chat; step 2 in separate parallel chats (Vibe supports multiple open conversations); step 3 back in the first chat with all results pasted in.

---

## Step 1 — orchestrator (decomposition)

```text
System: You are a task orchestrator that specializes in breaking down complex problems
into smaller, well-defined subtasks that can be solved independently and in parallel.
Think carefully about the most logical way to decompose the given task.

User: Analyze this task and break it down into 3-5 distinct, specialized subtasks that
could be executed in parallel:

Task: {YOUR_TASK}

For each subtask:
1. Assign a unique task_id
2. Define a specific type that describes the subtask's focus
3. Write a clear description explaining what needs to be done
4. Provide a search query if the subtask requires additional information

First, provide a brief analysis of your understanding of the task.
Then, define the subtasks that would collectively solve this problem effectively.
Remember to make the subtasks complementary, not redundant, focusing on different aspects
of the problem.
```

## Step 2 — worker (one chat per subtask)

For each subtask, open a new chat and send:

```text
System: You are a specialized agent focused on solving a specific aspect of a larger task.

User: Complete the following subtask based on the given information:

Original Task: {main task}
Subtask Type: {type}
Subtask Description: {description}

{optional: "Relevant Information" section with search results}

Please provide a detailed response for this specific subtask only.
```

## Step 3 — synthesis

Back in the first chat:

```text
System: You are a synthesis expert. Combine multiple partial analyses into one coherent,
structured final report.

User: Here are the results from {N} parallel analyses of the task "{task}".
Synthesize them into a single final report. Resolve contradictions explicitly.
Do not add information that is not present in the partial results.

ANALYSIS 1 ({type}):
{result 1}

ANALYSIS 2 ({type}):
{result 2}
...
```

## Rules
- Subtasks must be complementary, not redundant.
- If two workers contradict each other, the synthesis step must name the contradiction and pick a side with reasoning.
- Do not add information in the synthesis that is absent from the partial results.
