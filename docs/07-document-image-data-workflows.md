# Praca z dokumentami, obrazami i danymi w czacie Vibe (web)

Źródła: `mistral/ocr/*` (document_understanding, tool_usage, data_extraction, documentChunking, product_datasheet_analysis), `mistral/image_understanding/*`, `mistral/rag/*` (basic_RAG, mistral-search-engine, RAG_via_function_calling, mistral-reference-rag), `mistral/connectors/*` (quickstart-generator — porównanie jakości: brak narzędzi vs web search vs dokumentacja), `mistral/function_calling/function_calling.ipynb` (wzorce, nie kod).

W czacie webowym **nie budujesz pipeline'u OCR** — Work robi to za Ciebie przy wgrywaniu plików. Ale **wzorce ekstrakcji i strukturyzacji** z cookbooka przenoszą się wprost. Ten dokument to ich komplet.

---

## 1. Zasada ogólna: co wgrać, a czego prosić słowami

| Masz | Zrób |
|---|---|
| PDF / zdjęcie dokumentu / screenshot | Wgraj jako plik. Poproś o ekstrakcję wg schematu (punkt 3) lub streszczenie. |
| Długi dokument (>~10–20 stron) | Wgraj plik; jeśli czat „gubi wątek”, poproś o pracę sekcja po sekcji (punkt 5). |
| Obraz (wykres, tabela, formularz, UI) | Wgraj obraz; opisz dokładnie, co wyciągnąć (punkt 4). |
| CSV / arkusz / tabela | Wgraj plik lub wklej tabelę; aktywuj wbudowany skill `data-analysis` lub opisz zadanie. |
| Link do strony / PDF online | Podaj URL — Work ma web search i Open URL. |
| Wiele dokumentów na raz | Wgraj wszystkie do jednego czatu lub do Library / Projektu. |

## 2. „Grounding”: odpowiadaj tylko na podstawie źródła

Najważniejszy wzorzec z całego cookbooka RAG — cytuj źródła i nie dopowiadaj spoza kontekstu:

```text
Answer the question using ONLY the information in the provided documents.
If the answer is not in the documents, say "Not found in the provided documents."
For every factual claim, cite the source document and section.
```

Wariant mocniejszy (z wzorca „reference RAG”):

```text
Answer the question based on the references below. Include a [citation] after each
sentence that uses information from a reference. If the references do not contain
the answer, say so explicitly instead of guessing.
```

## 3. Ekstrakcja strukturalna z dokumentu (schemat JSON)

Wzorzec z `image_description_prompting_pixtral.ipynb` i `RAG_via_function_calling.ipynb` — zawsze: rola + „tylko fakty z dokumentu” + konkretny schemat + przykład:

```text
You are a data extraction system. Given a document, extract information strictly
according to the following JSON schema. Do not invent values. If a field is missing
in the document, use null.

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
{wgraj plik}
```

Wzorzec z `data_extraction.ipynb` (annotations): dla formularzy/faktur — najpierw wypisz pola z typami, potem wypełnij wartościami; dla figur/wykresów — osobna adnotacja (opis/caption) per element.

## 4. Praca z obrazami

### 4a. Opis strukturalny obrazu (z cookbooka, Pixtral)

```text
You are an image description system. Given an image, describe it by providing a list of
elements, their names and descriptions. And end with a global short description of the
full image.

# Response Format
Respond strictly in JSON using the following schema:

{
  "elements": [
    {
      "name": "<short name of the element>",
      "description": "<detailed description of the element>"
    }
  ],
  "global_description": "<short overall description of the image>"
}
```

### 4b. Ekstrakcja tabeli z obrazu (z multimodality_meets_function_calling)

```text
Extract the text from this image and output it as a JSON object.
The JSON must contain a single key "transactions" whose value is an array of objects,
one per row of the table, with keys matching the column headers.
Then convert the JSON to a Markdown table.
```

### 4c. Ogólne zasady dla obrazów

- Bądź konkretny, co wyciągnąć („wszystkie wiersze tabeli”, „daty i kwoty”, „etykiety osi wykresu”).
- Wymuś format (JSON/tabela Markdown).
- Dla wykresów: poproś o opis trendu + ekstrakcję danych liczbowych.

## 5. Duże dokumenty — ręczny chunking w czacie

Wzorzec z `basic_RAG.ipynb` (chunkowanie) i `documentChunking` (dla dokumentów przekraczających limity):

```text
The document is long. Process it section by section:
1. First, list all section headings in order.
2. For each section, provide a 3-5 sentence summary.
3. After all sections, answer: {pytanie} using only the summaries.
```

Albo prościej — dwa przebiegi:

```text
Przebieg 1: "Summarize each section of this document in 3 sentences. Label each summary
with its heading."
Przebieg 2 (wklej streszczenia): "Using only these summaries, answer: {pytanie}."
```

Zasada z cookbooka RAG: mniejsze porcje = lepsza precyzja; większe = ryzyko „zgubienia” istotnych fragmentów. W czacie wybierasz sekcje.

## 6. Wiele źródeł — router RAG („zapytaj właściwą bazę”)

Wzorzec z `RAG_via_function_calling.ipynb`: najpierw klasyfikuj, do którego źródła skierować pytanie:

```text
You are a QnA router. Classify the question into one of the data sources:
- HR: holidays, benefits, salary, office, employees
- Product: product specifics, features, pricing, packaging
- Finance: revenue, costs, liabilities, financial reports
- Other: anything unrelated to the above

Respond with the source name only.

Question: {pytanie}
```

W czacie: masz wiele wgranych plików / Libraries — klasyfikator mówi, który dokument otworzyć jako pierwszy.

## 7. Web search — kiedy włączać i jak „pilnować jakości”

Z `mistral-search-engine.ipynb`: wbudowane narzędzia to `websearch` (zwykłe wyszukiwanie) i `web_search_premium` (wyszukiwanie + zweryfikowane źródła prasowe). Agent w cookbookie:

```text
You have the ability to perform web searches with `web_search` to find up-to-date
information.
```

(z parametrami temperature 0.3, top_p 0.95 — czyli „raczej precyzyjnie”.)

W czacie: Work używa web search automatycznie w trybie think; możesz wymusić: „sprawdź w internecie aktualne informacje o X i podaj źródła z datami”.

**Zasada jakości z quickstart-generator (connectors):** odpowiedź rośnie jakościowo, gdy dasz modelowi **właściwe źródła**: sama wiedza modelu < web search < oficjalna dokumentacja (np. przez Context7/DeepWiki connector) < dokumentacja + web. W czacie: wgraj oficjalne dokumenty do Library, zamiast polegać na pamięci modelu.

## 8. Łączenie wielu źródeł w jednej odpowiedzi

Wzorzec z connectorów (DeepWiki + web search):

```text
Answer the question using the best available source for each sub-topic:
- For official API/behavior questions, rely on the attached official documentation.
- For recent news, releases, and community knowledge, use web search.
Cite which source you used for each part of the answer.
```

## 9. Dane tabelaryczne (CSV/arkusze)

W Vibe Work aktywuje się wbudowany skill **`data-analysis`** (inspekcja, czyszczenie, transformacja, agregacja, anomalie). Ręczny odpowiednik:

```text
You are a data analyst. Given the attached dataset:
1. Describe the columns, types, and row count.
2. Report missing values and obvious data quality issues.
3. Answer the user's question: {pytanie}
4. Show the exact steps (filters, aggregations) you applied.
5. Present results as a Markdown table.
```

Zasady: nigdy nie „zgaduj” wartości; pokaż kroki transformacji; wyniki jako tabela.

## 10. Porównanie dokumentów / wersji

```text
Compare the two documents and produce:
1. A summary of what changed (added / removed / modified).
2. A table: | Section | Document A | Document B | Nature of change |
3. A list of inconsistencies between the documents.
Use only the content of the two documents.
```

## 11. „Czat z dokumentem” — trwały kontekst

W Vibe web: wgraj dokumenty do **Library** (indeksowane, dostępne we wszystkich czatach) lub do **Projektu** (pliki wspólne dla grupy czatów). Nie musisz wgrywać ich ponownie w każdej rozmowie.

## 12. Czego unikać przy pracy z dokumentami

- Nie wklejaj całego 100-stronicowego PDF jako tekst w prompt — wgraj plik.
- Nie proś o „przeczytanie wszystkiego i odpowiedź” bez struktury — najpierw streszczenie sekcji, potem odpowiedź.
- Nie akceptuj odpowiedzi bez cytatów, gdy fakty są krytyczne — wymuś `source_quote`.
- Nie mieszaj faktów z dokumentu z wiedzą modelu — każ „grounding” (punkt 2).
