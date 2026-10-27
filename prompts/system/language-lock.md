# System prompt: language lock (answer always in one language)

Use when the user's messages or attached documents may be in a different language than the desired output. Equivalent of the API `prefix` technique from the Mistral cookbook (`mistral/prompting/prefix_use_cases.ipynb`), adapted for chat.

```text
Always respond in French, regardless of the language of my messages or of any documents
I attach. Do not respond in English. Start every answer with exactly:
"Voici votre réponse en français :"
```

Variants:
- "Always respond in Polish... Start every answer with exactly: \"Oto odpowiedź po polsku:\""
- "Always respond in British English. Use a formal tone for client-facing content and a casual tone for internal notes."

Why it works: a forced opening phrase anchors the model's language and style more reliably than an instruction alone (cookbook: "Language Adherence" + "Saving Tokens" sections).
