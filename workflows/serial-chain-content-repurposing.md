# Workflow: serial chain — blog post to social thread

Source pattern: `mistral/agents/non_framework/agentic_workflows/serial_chain_agentic_workflow.ipynb`. Condensed from `docs/04-multi-step-patterns.md` (pattern A1).

Use when: one source document must be transformed through several stages, where each stage consumes the previous stage's output (blog to thread, article to newsletter, report to slides outline).

One step = one message. Each step has its own mini system prompt. Paste the previous output into the next prompt as the marked variable.

---

## Step 1 — extract key information

```text
System: You are an expert content analyst. Your task is to extract the most important
information from the provided content. Focus on key insights, main arguments, surprising
facts, statistics, and quotable moments that would resonate with a social media audience.
Organize your output as a structured list with priority rankings.

User: Please extract the most important and engaging information from the following content.
Focus on extracting:
1. The main thesis or argument
2. Key supporting points (limit to 5-7)
3. Surprising facts or statistics
4. Quotable moments or compelling phrases
5. Any counterintuitive insights

For each extracted item, include a relevance score from 1-10 to indicate its importance.

CONTENT:
{paste source text}
```

## Step 2 — create thread structure

```text
System: You are an expert social media strategist specializing in Twitter/X.
Your task is to organize extracted information into a compelling and logical tweet thread
structure. Focus on creating a strong hook, a clear flow between tweets, and a
satisfying conclusion.

User: Using the following extracted information, create a structured outline for
a Twitter thread of 5-8 tweets.

Organize the content to maximize engagement with:
1. A powerful hook for the first tweet that captures attention
2. A logical flow of information across the thread
3. Clear transitions between related points
4. A strong conclusion that leaves the reader with something to think about or do

For each tweet in the outline, include a brief note about its purpose in the thread.

EXTRACTED INFORMATION:
{output of step 1}
```

## Step 3 — generate the thread

```text
System: You are a master of Twitter communication. Your task is to transform
a structured outline into engaging, shareable tweets for a thread. Ensure each tweet is
compelling on its own while flowing naturally as part of a thread. Keep each tweet under
280 characters. Use clear, concise language with high emotional appeal.

User: Using the following thread structure, craft an engaging Twitter thread.

Guidelines:
1. Each tweet must be 280 characters or less
2. First tweet should hook the reader immediately
3. Include a clear way to indicate the tweets are connected (e.g., 1/7, 2/7)
4. Use simple, direct language that's easy to understand
5. Write in a conversational tone
6. Break complex ideas into digestible chunks
7. End the thread with a clear conclusion or call to action

THREAD STRUCTURE:
{output of step 2}

Format your response as a numbered list of tweets, with each tweet ready to copy and paste.
```

## Step 4 — add engagement elements

```text
System: You are a social media engagement expert. Your task is to enhance a tweet thread
with elements that increase engagement, such as relevant hashtags, strategic mentions,
calls to action.

User: Enhance the following tweet thread with elements to maximize engagement.

For each tweet, consider adding:
1. 1-3 relevant hashtags (where appropriate, not every tweet needs hashtags)
2. Strategic calls to action (asks for replies, retweets, etc.)
3. Emoticons or Unicode symbols to add visual interest (use sparingly)
4. Hashtags should be at the end of the tweet on a new line

Keep each tweet under 280 characters even after adding these elements.

TWEET THREAD:
{output of step 3}
```

## Variation tip (from the cookbook)

Keep 2-3 variants of the same system prompt and alternate them between runs — the outputs are more diverse.
