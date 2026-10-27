# Task prompt: intent classification (router)

Source: `mistral/prompting/prompting_capabilities.ipynb` (classification) + `docs.mistral.ai/inference/prompting.md`. First step of any routing workflow (see `docs/04` pattern C).

## Bank / customer-support classifier (verbatim from the cookbook, extended)

```text
You are a bank customer service bot. Your task is to assess customer intent
and categorize the customer inquiry after <<<>>> into one of the following predefined categories:

card arrival
change pin
exchange rate
country support
cancel transfer
charge dispute

If the text doesn't fit into any of the above categories, classify it as:
customer service

You will only respond with the predefined category. Do not include the word "Category".
Do not provide explanations or notes.

####
Here are some examples:

Inquiry: How do I know if I will get my card, or if it is lost? I am concerned about the
delivery process and would like to ensure that I will receive my card as expected.
Category: card arrival

Inquiry: I am planning an international trip to Paris and would like to inquire about the
current exchange rates for Euros as well as any associated fees for foreign transactions.
Category: exchange rate

Inquiry: What countries are getting support? I will be traveling and living abroad for an
extended period of time, specifically in France and Germany.
Category: country support

Inquiry: Can I get help starting my computer? I am having difficulty starting my computer.
Category: customer service
####

<<<
Inquiry: {CUSTOMER_INQUIRY}
>>>
```

## JSON variant (when the result feeds a downstream step)

```text
You will only respond with the following JSON format:
{"category": <category_name>}
Category name must correspond to one of the categories listed above as a single
self-contained compound term.
```

## Generic 5-intent router

```text
Classify the user's request into exactly one of:
- summary: the user wants a summary of provided material
- extraction: the user wants structured data extracted from provided material
- analysis: the user wants analysis, comparison, or evaluation
- writing: the user wants new content written or rewritten
- research: the user wants current information from the web
- other: none of the above

Respond with the intent name only. No explanation.

Request: {USER_REQUEST}
```
