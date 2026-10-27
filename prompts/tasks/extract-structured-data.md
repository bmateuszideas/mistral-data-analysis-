# Task prompt: structured data extraction

Source: `mistral/image_understanding/image_description_prompting_pixtral.ipynb`, `mistral/ocr/data_extraction.ipynb`, `mistral/evaluation/evaluation.ipynb`. Full skill version: `skills/structured-extraction/SKILL.md`.

## Generic version (attach the document/image)

```text
You are a data extraction system. Given the document below, extract information strictly
according to the following JSON schema. Do not invent values. If a field is missing in
the document, use null.

# Schema
{
  "document_type": "<string>",
  "date": "<YYYY-MM-DD or null>",
  "parties": [{"name": "<string>", "role": "<string>"}],
  "key_terms": [{"term": "<string>", "value": "<string>", "source_quote": "<string>"}],
  "amounts": [{"label": "<string>", "value": <number>, "currency": "<string>"}]
}

# Rules
- "source_quote" must be a short verbatim quote from the document.
- Respond with JSON only. No explanations.

# Document
{attach file}
```

## Image / table version

```text
Extract the text from this image and output it as a JSON object. The JSON must contain a
single key "transactions" whose value is an array of objects, one per row of the table,
with keys matching the column headers. Then convert the JSON to a Markdown table.
```

## Structured image description version

```text
You are an image description system. Given an image, describe it by providing a list of
elements, their names and descriptions. End with a global short description of the full
image.

# Response Format
Respond strictly in JSON using the following schema:

{
  "elements": [
    {"name": "<short name>", "description": "<detailed description>"}
  ],
  "global_description": "<short overall description>"
}
```
