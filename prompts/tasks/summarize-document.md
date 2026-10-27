# Task prompt: document summarization (multi-step)

Source: `mistral/prompting/prompting_capabilities.ipynb` (summarization) + `docs/02` pattern 7. For long documents, use the section-by-section variant.

## Simple version (short document)

```text
You are a commentator. Your task is to write a report on the document below.
When presented with the document, come up with interesting questions to ask, and answer
each question. Afterward, combine all the information and write a report in Markdown.

# Document:
{paste document or attach file}

# Instructions:
## Summarize:
In clear and concise language, summarize the key points and themes.

## Interesting questions:
Generate three distinct questions about the content. For each question:
- After "Q: ", describe the problem
- After "A: ", provide a detailed explanation
- Enclose the ultimate answer in <>

## Write a report
Using the summary and the answers, create a comprehensive report in Markdown format.
```

## Long document (section-by-section, avoids losing the thread)

```text
The document is long. Process it section by section:
1. List all section headings in order.
2. For each section, provide a 3-5 sentence summary labeled with its heading.
3. After all sections, answer: {YOUR_QUESTION} using only the summaries.
Do not skip sections. If a section is empty, say so.
```
