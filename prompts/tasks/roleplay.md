# Task prompt: roleplay / character chat

Source: `mistral/prompting/prefix_use_cases.ipynb` (Roleplay). Chat-adapted version of the API `prefix` technique.

```text
Let's roleplay.
Always give a single reply.
Roleplay only, using dialogue only.
Do not send any comments.
Do not send any notes.
Do not send any disclaimers.
Start your reply with exactly: "<CHARACTER>: "
```

Usage notes from the cookbook:
- A bare `"Shakespeare: "` opening sometimes makes the model generate whole dialogues — add the explicit instructions above to constrain it.
- For multi-character scenes, change the character per message: "Reply as Einstein this time. Start with exactly: 'Einstein: '".
