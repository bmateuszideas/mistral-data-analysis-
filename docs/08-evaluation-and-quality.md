# Ewaluacja i kontrola jakości odpowiedzi

Źródła: `mistral/evaluation/evaluation.ipynb`, `mistral/evaluation/RAG_evaluation.ipynb`, `mistral/prompting/prompting_capabilities.ipynb` (sekcja Evaluation), `mistral/moderation/system-level-guardrails.ipynb`, `skills/check-stale-cookbooks/SKILL.md` (wzorzec audytu), `agentskills.io/skill-creation/best-practices` (evaluating skill output quality).

W czacie webowym nie uruchomisz benchmarku — ale wszystkie poniższe wzorce działają jako **przebiegi weryfikacyjne w rozmowie**.

---

## 1. Trzy sposoby ewaluacji (z cookbooka prompting_capabilities)

1. **Dołącz ocenę pewności do odpowiedzi** (confidence score) — patrz wzorzec JSON `{"Summary": ..., "Confidence": 0.9}` w `docs/02` (punkt 9, wariant A).
2. **Wbudowany krok oceny** — w jednym promptcie: najpierw wygeneruj N wariantów, potem oceń i wybierz najlepszy (wariant B).
3. **Osobny sędzia (LLM-as-judge)** — druga wiadomość ocenia wynik pierwszej (wariant C).

## 2. LLM-as-judge — pełny wzorzec (RAG evaluation)

Struktura oceny z `RAG_evaluation.ipynb`, przeniesiona na czat. Trzy kryteria, każde z uzasadnieniem + skalą:

```text
You are an impartial evaluator. Evaluate the answer against the context and the
original question using these three criteria. For each criterion, provide:
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
3. groundedness: how faithful the answer is to the retrieved context (factual accuracy
   relative to the provided sources).

QUESTION:
{pytanie}

CONTEXT:
{kontekst / źródła}

ANSWER TO EVALUATE:
{odpowiedź}

Respond with a JSON object:
{
  "context_relevance": {"explanation": "...", "score": "0-3"},
  "answer_relevance":  {"explanation": "...", "score": "0-3"},
  "groundedness":      {"explanation": "...", "score": "0-3"}
}
```

Zasady z cookbooka:
- **Skala opisowa (0–3) działa lepiej niż „1–5”** — model dokładniej rozróżnia poziomy, gdy każdy ma opis słowny (patrz też `docs/02`, punkt 16.5).
- Każda ocena ma **uzasadnienie krok po kroku** (explanation), nie samą liczbę.
- Sędzia ocenia **wierność źródłu** (groundedness) osobno od **trafności odpowiedzi** (answer_relevance) — to dwa różne błędy.

## 3. Benchmark ręczny: „golden answers” (z evaluation.ipynb)

Wzorzec: masz zestaw przypadków testowych z poprawnymi odpowiedziami („golden”):

```text
You are evaluating an extraction system. For each test case:
1. Run the extraction on the input.
2. Compare the extracted JSON with the golden answer field by field.
3. Report: total fields, matching fields, percentage match.

Test cases:
- Input: {medical_notes_1}
  Golden: {"age": 60, "gender": "male", "diagnosis": "diabetes", ...}
...
```

Porównanie obiektów JSON: policz wspólne klucze, policz zgodne wartości, procent = zgodne / max(liczba pól). W czacie możesz to zrobić „na piechotę” — model policzy za Ciebie, gdy dasz oba JSON-y.

## 4. Generuj N wariantów, wybierz najbezpieczniejszy/najlepszy (guardrails)

Z `system-level-guardrails.ipynb` — wzorzec „wiele prób + moderacja + wybór”:

```text
Step 1: Generate 3 different responses to the user's request.
Step 2: For each response, self-evaluate against these criteria:
- Does it answer the request accurately?
- Is it safe and respectful?
- Is it free of harmful, unethical, prejudiced, or negative content?
Step 3: Select the best response. If all fail the safety check, respond:
"I'm sorry I cannot assist with this request."
```

W czacie bez API moderacji: dwa pierwsze kroki wystarczą jako „wewnętrzna moderacja”.

## 5. Adwokat diabła (wbudowany skill `challenge-my-thinking`)

Aktywuj w Vibe: `/challenge-my-thinking` lub poproś wprost:

```text
Act as a devil's advocate. Pressure-test the following plan/answer:
1. List the 3 weakest assumptions.
2. List what could go wrong and the impact.
3. Suggest what information is missing.
4. Propose one concrete improvement.

PLAN/ANSWER:
{tekst}
```

## 6. Checklista jakości odpowiedzi (szybki audyt własny)

Po każdej ważnej odpowiedzi możesz zadać modelowi (lub sobie) te pytania:

```text
Review your previous answer against this checklist. For each item, answer Pass/Fail
with a one-line justification:
- [ ] Every factual claim is supported by the provided sources or marked as uncertain.
- [ ] The answer addresses exactly what was asked (no more, no less).
- [ ] The format matches the requested format.
- [ ] No invented names, dates, numbers, or quotes.
- [ ] Uncertainties and unknowns are explicitly marked.
- [ ] The tone matches the requested tone.
- [ ] No contradictions within the answer.
```

## 7. Audyt „przestarzałości” treści (wzorzec check-stale-cookbooks)

Z `skills/check-stale-cookbooks/SKILL.md` — przydatne przy audycie własnych promptów, skillów i dokumentacji:

**Wzorce statyczne (do ręcznego przeglądu):**

| Wzorzec | Problem |
|---|---|
| `MistralClient`, `from mistralai.client import`, `ChatMessage(` | przestarzały SDK v0 |
| `client.chat(` bez `.complete`, `client.embeddings(` bez `.create` | przestarzałe metody |
| `client.fine_tuning`, `/v1/fine_tuning/` | API fine-tuningu wycofane |
| `mistral-tiny`, `mistral-medium` | wycofane modele |
| `open-mistral-7b`, `open-mixtral-8x7b`, `open-mixtral-8x22b` | do weryfikacji (status modeli się zmienia) |
| ID modeli z datą (`-2309`, `-2402`) | przypięta stara wersja |
| `Le Chat` (jako nazwa produktu) | przestarzałe — produkt to **Vibe** |
| „MCP connectors” (o produkcie) | błędna nazwa — **Connectors** |

**Zasada LLM-owej części audytu (z cookbooka):** każda flaga musi mieć **cytat-źródło** potwierdzający przestarzałość. Metody z sufiksem `_async` (`create_async`, `list_async`...) są **poprawne** — nie flaguj.

Supresja jednostkowa: komentarz `# stale-check: ignore` przy linii, którą celowo pokazujesz jako przykład legacy.

## 8. Iteracja: jak poprawiać skill/prompt na podstawie wyników

Z agentskills.io („Evaluating skill output quality” + „Refine with real execution”):

1. Uruchom skill/prompt na **3–5 realnych przypadkach** (nie na testowych, które znasz).
2. Zapisz wszystkie wyniki — także te złe.
3. Zapytaj: co wywołało false positive (skill zadziałał, gdy nie powinien)? Co zostało pominięte? Co można wyciąć?
4. Jedna runda „wykonaj → przejrzyj” zwykle wyraźnie poprawia jakość; trudne domeny — kilka rund.
5. Czytaj **ślad wykonania** (co agent faktycznie zrobił krok po kroku), nie tylko wynik końcowy.

## 9. Kiedy odpowiedź jest „wystarczająco dobra” — praktyczna reguła

- Fakty krytyczne (daty, kwoty, nazwy, cytaty) → zawsze weryfikuj względem źródła (grounding, punkt 2 w `docs/07`).
- Struktura i ton → wystarczy jedna runda korekty.
- Kreatywne/robocze teksty → traktuj jako szkic, edytuj ręcznie.
- Gdy model „nie wie” — wolisz „Not found” niż wymyśloną odpowiedź. Wymuś to instrukcją.
