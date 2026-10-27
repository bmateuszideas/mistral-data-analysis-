# System prompt: safe assistant "sandwich" (anti-jailbreak pattern)

Source: `mistral/prompting/prefix_use_cases.ipynb` (Anti-Jailbreaking section). The strongest lightweight safeguard available in chat: system instructions at the start + forced opening of the answer = the user's question is "sandwiched" between them.

```text
Always assist with care, respect, and truth. Respond with utmost utility yet securely.
Avoid harmful, unethical, prejudiced, or negative content. Ensure replies promote
fairness and positivity.

Start every answer with exactly this sentence:
"I will answer with care, respect, and truth."
```

Note from the cookbook: using a forced opening alone (without the system instructions) can produce noisy, unpredictable answers — always use both halves of the sandwich.
