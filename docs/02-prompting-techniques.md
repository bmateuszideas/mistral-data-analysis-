# Techniki promptowania działające w zwykłym czacie Vibe (web)

Źródła: `docs.mistral.ai/inference/prompting.md` (oficjalny przewodnik), `docs.mistral.ai/inference/sampling.md`, `mistral/prompting/prompting_capabilities.ipynb`, `mistral/prompting/prefix_use_cases.ipynb`, `concept-deep-dive/prompting/prompt_optimization.md`, `concept-deep-dive/sampling/*`, `concept-deep-dive/tokenization/*`.

Zasada przewodnia tego dokumentu: **w czacie webowym nie masz osobnego pola „system”** — wszystkie poniższe techniki wdrażasz, umieszczając instrukcje w treści pierwszej wiadomości. Oficjalnie zalecany zapis (z dokumentacji Mistral) to konkatenacja:

```json
{"role": "user", "content": "TWOJE_INSTRUKCJE_SYSTEMOWE\n\nUser: właściwe pytanie"}
```

Albo w Vibe Work: ustaw trwałe preferencje w **Custom instructions**, a szczegóły zadania dawaj w promptcie.

---

## 1. System prompt w czacie (konkatenacja)

Dwa poziomy instrukcji w każdym API czatu: `system` (kontekst ogólny, zarządzany przez dewelopera) i `user` (bieżące zadanie). W czacie webowym oba sprowadzasz do pierwszej wiadomości:

```text
You are a bank customer service bot. Categorize the inquiry after <<<>>> into one of:
card arrival, change pin, exchange rate, country support, cancel transfer, charge dispute.
If none fits, answer: customer service. Respond with the category only.

#### Examples
Inquiry: How do I know if I will get my card? Category: card arrival
Inquiry: What are the exchange rates for Euros? Category: exchange rate
####
<<<
Inquiry: TWÓJ TEKST TUTAJ
>>>
```

## 2. Rola + zadanie (roleplay)

Pierwszy krok każdego dobrego promptu. Wzór oficjalny:

> "You are a `<rola>`, your task is to `<zadanie>`.”

Przykłady z cookbooka:
- "You are a bank customer service bot. Your task is to assess customer intent..."
- "You are a commentator. Your task is to write a report on an essay."
- "You are a mortgage lender customer service bot... create personalized email responses..."
- "You are an expert game developer. You produce complete, self-contained HTML files..."

Rola ustawia "pion" (dziedzinę), w którym model pracuje, i znacząco podnosi trafność.

## 3. Struktura hierarchiczna i formatowanie

Organizuj instrukcje w sekcje. Pisz tak, **jakbyś pisał dla osoby bez żadnego kontekstu** — ona ma wykonać zadanie wyłącznie na podstawie Twojego promptu.

- Używaj nagłówków Markdown: `# Kategorie`, `# Response Format`, `# Examples`, `## ` dla podsekcji.
- Używaj **delimiterów** między sekcjami i danymi: `###`, `<<<` `>>>`, `####`. Wzorzec z cookbooka: `###` oznacza blok przykładów, `<<< >>>` oznacza „tu wstawiam dane wejściowe”.
- Markdown i tagi w stylu XML są idealne, bo są czytelne dla ludzi, parsowalne maszynowo i znajome modelowi z treningu.

Oficjalny przykład dobrze zbudowanego promptu (detekcja języka, z dokumentacji):

```text
You are a language detection model, your task is to detect the language of the given text.
# Available Languages
Select the language from the following list:
- English: "en"
- French: "fr"
- Spanish: "es"
- German: "de"
Any language not listed must be classified as "other" with the code "on".
# Response Format
Your answer must follow this format:
{"language_iso": <language_code>}
# Examples
Below are sample inputs and expected outputs:
## English
User: Hello, how are you?
Answer: {"language_iso": "en"}
## French
User: Bonjour, comment allez-vous?
Answer: {"language_iso": "fr"}
```

## 4. Few-shot prompting (przykłady w promptcie)

Podaj 2–4 przykłady wejście→wyjście. To najskuteczniejsza technika podnoszenia dokładności **i kontroli formatu**.

```text
[...]
# Examples
Input: Hello, how are you?
Output: {"language_iso": "en"}
[...]
```

W czacie możesz też zasymulować few-shot jako historię rozmowy: wyślij pary user→assistant, a potem właściwe pytanie. Wzorzec z dokumentacji (standard few-shot jako historia):

```json
[
  {"role": "system", "content": "You are a language detection model..."},
  {"role": "user", "content": "Hello, how are you?"},
  {"role": "assistant", "content": "{\"language_iso\": \"en\"}"},
  {"role": "user", "content": "Bonjour, comment allez-vous?"},
  {"role": "assistant", "content": "{\"language_iso\": \"fr\"}"}
]
```

Wskazówka: przy klasyfikacji dawaj przykłady **ró also dla kategorii "fallback"** (np. `customer service`), żeby model wiedział, co zrobić z tekstem spoza kategorii.

## 5. Wymuszanie formatu wyjścia (JSON / schemat)

Proś o konkretny format i **pokaż schemat lub przykład**. W czacie nie masz `response_format`, więc format wymuszasz słownie + przykładem:

```text
You will only respond with a JSON object with the keys Summary and Confidence.
Do not provide explanations.
```

Im bardziej konkretny opis struktury (nazwy kluczy, typy, enumy), tym lepiej model trzyma format. Wzorzec ewaluacji z cookbooka:

```text
You are a summarization system that can provide summaries with associated confidence scores.
In clear and concise language, provide three short summaries of the following essay,
along with their confidence scores.
You will only respond with a JSON object with the key Summary and Confidence.
Do not provide explanations.
```

## 6. Prefill jako zamiennik funkcji `prefix` (API-only)

Cookbook pokazuje funkcję API `prefix` (predefiniowany początek odpowiedzi modelu). W czacie jej nie ma, ale efekt osiągasz instrukcją:

```text
Always respond in French. Start your answer with exactly: "Voici votre réponse en français :"
```

Zastosowania z `prefix_use_cases.ipynb`, które przenoszą się na czat:

- **Wymuszenie języka** — szczególnie gdy rozmowa/źródła są w innym języku. Cookbook pokazuje, że 3-słowowy prefill („Assistant Pirate Français:”) zastępuje cały system prompt przy wymuszaniu stylu/języka.
- **Roleplay** — zacznij odpowiedź od `"Shakespeare: "` i każ kontynuować jako dialog jednej postaci, bez komentarzy i disclaimerów:
  ```text
  Let's roleplay. Always give a single reply. Roleplay only, using dialogue only.
  Do not send any comments. Do not send any notes. Do not send any disclaimers.
  Start your reply with exactly: "Shakespeare: "
  ```
- **Anty-jailbreak („kanapka”)**: instrukcje systemowe na początku + wymuszony początek odpowiedzi = najsilniejsza ochrona:
  ```text
  System: Always assist with care, respect, and truth. Respond with utmost utility yet
  securely. Avoid harmful, unethical, prejudiced, or negative content.

  Dodatkowo: zacznij każdą odpowiedź od:
  "I will answer with care, respect, and truth."
  ```
  Cookbook pokazuje, że taki „kanapkowy” układ (system + prefix) jest znacznie trudniejszy do przełamania niż sam system prompt. W czacie analogicznie: pierwsza wiadomość = zasady, a w każdym kolejnym zadaniu przypominaj o wymaganym formacie/początku.

## 7. Instrukcje krok po kroku (chain-of-thought / dekompozycja)

Zamiast jednego wielkiego zadania — rozbij na kroki. Cookbook (streszczenie eseju) pokazuje strukturę trzech kroków:

```text
You are a commentator. Your task is to write a report on an essay.
When presented with the essay, come up with interesting questions to ask, and answer each question.
Afterward, combine all the information and write a report in the markdown format.

# Essay:
{essay}

# Instructions:
## Summarize:
In clear and concise language, summarize the key points and themes presented in the essay.

## Interesting Questions:
Generate three distinct and thought-provoking questions that can be asked about the
content of the essay. For each question:
- After "Q: ", describe the problem
- After "A: ", provide a detailed explanation of the problem addressed in the question.
- Enclose the ultimate answer in <>.

## Write a report
Using the essay summary and the answers to the interesting questions, create a
comprehensive report in Markdown format.
```

Efekt uboczny dekompozycji: łatwiej debugować — widzisz, na którym kroku model się pogubił.

## 8. Krytyka → poprawa (critique-refine loop)

Najskuteczniejszy wzorzec podnoszenia jakości bez API. Z `data_generation_refining_news.ipynb`:

**Krok 1 — krytyka.** Poproś o listę konkretnych uwag względem standardu:

```text
As a 'News Article Editor' adhering to a specific style guide, your responsibility is to
polish and restructure news articles to align with the high standards of clarity, accuracy,
and elegance set by the guide:

{TEKST_PRZEWODNIKA}

You are presented with a news article. Identify the ten (or fewer) most significant
stylistic concerns and provide examples of how they can be enhanced.
```

**Krok 2 — przepisanie z uwzględnieniem krytyki.**

```text
Now, incorporate the feedback and critiques into the news article and respond with the
enhanced version, focusing solely on stylistic improvements without altering the content.
You must provide the entire article enhanced.
Do not make ANY comments, only provide the new article improved.
Do not tell me what you changed, only provide the new article taking into consideration
the feedback you provided.
```

W czacie: krok 1 i krok 2 to dwie osobne wiadomości (w drugiej wklejasz tekst + krytykę). Cookbook używa też **wariacji system promptów** (6 lekko różnych wersji tego samego promptu) dla większej różnorodności wyników — w czacie możesz poprosić o 3 warianty odpowiedzi jednym promptem.

## 9. Ewaluacja własnego wyjścia (trzy warianty)

Z `prompting_capabilities.ipynb` + `RAG_evaluation.ipynb`:

**Wariant A — ocena z confidence score** (patrz punkt 5: JSON `Summary` + `Confidence`).

**Wariant B — wbudowany krok oceny:**

```text
You are given an essay text and need to provide summaries and evaluate them.

# Essay:
{essay}

Step 1: Provide three short summaries of the given essay. Each summary should be clear,
concise, and capture the key points. Aim for around 2-3 sentences for each summary.
Step 2: Evaluate the three summaries from Step 1 and rate which one you believe is the
best. Explain your choice by pointing out specific reasons such as clarity, completeness,
and relevance to the essay content.
```

**Wariant C — sędzia (LLM-as-judge).** Druga wiadomość ocenia wynik pierwszej. Struktura oceny RAG z cookbooka (przeniesiona na czat):

```text
You are given an essay and three summaries of the essay. Evaluate the three summaries
and rate which one is the best. Explain your choice by pointing out specific reasons such
as clarity, completeness, and relevance to the essay content.

# Essay:
{essay}

# Summaries
{wyniki do oceny}
```

Skala opisowa 0–3 używana w cookbooku (działa lepiej niż „1–5”):

```text
Score as a string between '0' and '3'.
0: No relevance/Not grounded/Irrelevant - completely unrelated or not based on the context.
1: Low relevance - minimal relevance or grounding.
2: Medium relevance - somewhat relevant or grounded.
3: High relevance/High groundedness - highly relevant or grounded.
```

Pełne kryteria RAG-as-judge: **context_relevance** (czy kontekst pasuje do zapytania), **answer_relevance** (czy odpowiedź adresuje zapytanie), **groundedness** (czy odpowiedź jest wierna kontekstowi). Każde: `explanation` (uzasadnienie krok po kroku) + `score`.

## 10. Łańcuch LLM (serial chain)

Wzorzec: wynik kroku N jest wejściem kroku N+1. W czacie = kolejne wiadomości z wklejonym wynikiem.

Pełny przykład z cookbooka — blog post → wątek na X/Twitter, 4 kroki, każdy z własnym system printem:

1. **Extract Key Information** — system: „You are an expert content analyst... extract key insights, main arguments, surprising facts, statistics, quotable moments... Organize as a structured list with priority rankings.” User: lista 5 kategorii ekstrakcji + „include a relevance score from 1-10”.
2. **Create Thread Structure** — system: „You are an expert social media strategist specializing in Twitter/X... strong hook, clear flow, satisfying conclusion.” User: „create a structured outline for a Twitter thread of 5-8 tweets”.
3. **Generate Tweet Thread** — system: „You are a master of Twitter communication... Keep each tweet under 280 characters.” User: wytyczne (hook, numeracja 1/7, prosty język, CTA na końcu).
4. **Add Engagement Elements** — system: „You are a social media engagement expert...” User: 1–3 hashtagi na tweet, CTA, emoji oszczędnie, nadal ≤280 znaków.

W czacie: każdy krok osobna wiadomość, w następnej wklejasz wynik poprzedniej jako „{EXTRACTED INFORMATION}" / „{THREAD STRUCTURE}" itd.

## 11. Orkiestrator + równolegli workerzy + synteza (parallel subtask)

Z `parallel_subtask_agentic_workflow.ipynb`, przeniesione na czat:

**Krok 1 — dekompozycja (orkiestrator):**

```text
Analyze this task and break it down into 3-5 distinct, specialized subtasks that could
be executed in parallel:

Task: {TWOJE_ZADANIE}

For each subtask:
1. Assign a unique task_id
2. Define a specific type that describes the subtask's focus
3. Write a clear description explaining what needs to be done
4. Provide a search query if the subtask requires additional information

First, provide a brief analysis of your understanding of the task.
Then, define the subtasks that would collectively solve this problem effectively.
```

System prompt orkiestratora:

```text
You are a task orchestrator that specializes in breaking down complex problems into
smaller, well-defined subtasks that can be solved independently and in parallel.
Think carefully about the most logical way to decompose the given task.
```

**Krok 2 — workerzy.** Dla każdego podzadania osobna wiadomość (możesz wysłać kilka czatów równolegle — w Vibe Work każdy czat to osobna sesja):

```text
Complete the following subtask based on the given information:

Original Task: {zadanie}
Subtask Type: {typ}
Subtask Description: {opis}

Please provide a detailed response for this specific subtask only.
```

System prompt workera: „You are a specialized agent focused on solving a specific aspect of a larger task.”

**Krok 3 — synteza.** Wklej wszystkie wyniki workerów i poproś o połączenie w jedną spójną odpowiedź (w oryginale synteza też była osobnym wywołaniem LLM).

## 12. Router intencji / triage

Zanim odpowiesz „właściwie”, najpierw sklasyfikuj, co użytkownik chce, i rozgałęź. Pełny, gotowy prompt klasyfikatora bankowego (z cookbooka + dokumentacji):

```text
You are a bank customer service bot. Your task is to assess customer intent and
categorize customer inquiry into one of the predefined categories.

# Categories
The main categories available are the following:
- card_arrival: Inquiries about the arrival of the card, or if it is lost.
- change_pin: Inquiries about changing the pin code of the card.
- exchange_rate: Inquiries about the exchange rate of the card.
- country_support: Inquiries about the countries supported by the card.
- cancel_transfer: Inquiries about canceling a transfer.
- charge_dispute: Inquiries about a charge dispute.

If the text doesn't fit into any of the above categories, classify it as:
- customer_service: Inquiries about customer service in general.

# Answer Format
You will only respond with the category among the categories listed above without any
explanations or notes, in a single self-contained compound term.

## Examples
User: How do I know if I will get my card, or if it is lost? ...
Answer: card_arrival
User: I am planning an international trip to Paris and would like to inquire about the
current exchange rates for Euros...
Answer: exchange_rate
User: What countries are getting support? ...
Answer: country_support
User: Can I get help starting my computer? ...
Answer: customer_service
```

Wariant JSON (gdy wynik ma być dalej przetwarzany):

```text
# Answer Format
You will only respond with the following JSON format:
{"category": <category_name>}
Category name must correspond to one of the categories listed above.
```

W czacie router działa tak: pierwsza wiadomość klasyfikuje, druga wykonuje zadanie zgodnie z kategorią (każda kategoria ma własny mini-system-prompt).

## 13. Personalizacja odpowiedzi

Wzorzec z cookbooka (kredyt hipoteczny) — sekcja faktów + oryginalna wiadomość klienta:

```text
You are a mortgage lender customer service bot, and your task is to create personalized
email responses to address customer questions. Answer the customer's inquiry using the
provided facts below. Ensure that your response is clear, concise, and directly addresses
the customer's question. Address the customer in a friendly and professional manner.
Sign the email with "Lender Customer Support."

# Facts
30-year fixed-rate: interest rate 6.403%, APR 6.484%
15-year fixed-rate: interest rate 5.705%, APR 5.848%
...

# Email
Dear mortgage lender,
What's your 30-year fixed-rate APR, how is it compared to the 15-year fixed rate?
Regards,
Anna
```

Zasada: **fakty jako twarde dane** (nie proś modelu, żeby „znał” Twoje dane — wklej je), klient w oryginale, odpowiedź spersonalizowana i podpisana.

## 14. Wybór „temperatury” — co ustawiać, gdy możesz (i jak to przenieść na czat)

W czacie webowym nie ustawiasz temperatury ręcznie. Masz dwa pokrętła:

- **fast/think toggle** w Work (szybka odpowiedź vs głęboka analiza),
- precyzję instrukcji (im bardziej jednoznaczna, tym bardziej „deterministyczna” odpowiedź).

Zasady z `concept-deep-dive/sampling` i dokumentacji, przetłumaczone na wybór trybu:

| Typ zadania | Zalecane podejście | Uzasadnienie |
|---|---|---|
| Klasyfikacja, ekstrakcja, matematyka, tłumaczenie techniczne | **fast**, prompt maksymalnie precyzyjny, temperatura „0” w duchu | deterministyczność = dokładność; klasyfikator powinien zawsze wybrać najlepszy token |
| Streszczenia, raporty, analiza | **think**, temperatura niska (0.15–0.2 w duchu) | mało powtórzeń, wierność źródłu |
| Burza mózgów, pisanie kreatywne, roleplay, slogany | **think**, luźniejsze instrukcje, możesz prosić o N wariantów | wyższa temperatura = różnorodność; uważaj — za wysoka psuje trzymanie instrukcji |
| Generowanie wielu wariantów | poproś wprost: „daj 3 różne wersje” | w czacie nie ma parametru `n`; prośba słowna działa równie dobrze |

Mechanika (dla zrozumienia): temperatura skaluje logity w softmax — 0 = zawsze najbardziej prawdopodobny token; wyższa = płaska rozkład, więcej różnorodności. Top-P (jądro) ucina „ogon” mało prawdopodobnych tokenów. Możesz łączyć samplery (najpierw top-k, potem top-p), kolejność ma znaczenie. Presence penalty (−2..2) penalizuje tokeny już użyte (różnorodność słownictwa); frequency penalty penalizuje proporcjonalnie do częstości (silniejszy w długich tekstach). W czacie oba efekty osiągasz instrukcją: „używaj zróżnicowanego słownictwa, unikaj powtórzeń”.

## 15. Praca z kontekstem i limitami (tokenizacja — praktyczne wnioski)

Z `concept-deep-dive/tokenization/`:

- Model „liczy” **tokeny**, nie znaki. Polski tekst jest zazwyczaj „droższy” tokenowo niż angielski (więcej tokenów na znak).
- System prompt zajmuje miejsce w oknie kontekstu — długi, rozwlekły system prompt = mniej miejsca na rozmowę i dokumenty.
- Praktyczne wnioski dla czatu:
  - Instrukcje **krótkie i konkretne** (każde słowo pracuje na wynik).
  - Długie dokumenty wklejaj **jako plik** (Work czyta PDF/CSV), nie jako tekst w promptcie, gdy przekraczają komfortową długość.
  - Gdy czat „gubi wątek” przy długich dokumentach — poproś o streszczenie sekcji po sekcji i pracuj na streszczeniach.

## 16. Czego UNIKAĆ (oficjalna lista z dokumentacji Mistral)

To lista „good to know” prosto z `docs.mistral.ai/inference/prompting`:

1. **Niejasne przymiotniki ilościowe i jakościowe**: „too long”, „too short”, „many”, „few”, „interesting”, „better”, „things”, „stuff”. → Podawaj **miary obiektywne** i definiuj, co dokładnie rozumiesz.
2. **Sprzeczne instrukcje** w długim system promptcie (np. „if new data → update record” + „if data is new → create record”). → Używaj **drzewka decyzyjnego** w Markdown:
   ```markdown
   ## How to update database records
   Follow these steps:
   - If the data does not include new information (i.e., it already exists in a record):
       - Ignore this data.
   - Otherwise, if the data is not related to any existing record in the same table:
       - Create a new record.
   - Otherwise, if the related record is larger than 100 characters:
       - Create a new record.
   - Otherwise, if the data directly contradicts the existing record:
       - Delete the existing record and create a new one.
   - Otherwise:
       - Update the existing record to include the new data.
   ```
3. **Nie każ modelowi liczyć słów/znaków**. → Podaj liczby jako dane:
   ```text
   Existing records:
     - { record: "User: Alice, Age: 30", charCount: 15 }
   New data:
     - { data: "User: Charlie, Age: 35", charCount: 17 }
   ```
4. **Nie generuj zbędnych tokenów** — każ modelowi płaci się za wygenerowane tokeny; proś tylko o to, co niezbędne (np. nie pełną treść rekordu przy operacji NO_OP).
5. **Skale opisowe zamiast numerycznych**: zamiast „oceń 1–5” użyj:
   ```text
   Rate these options using this scale:
   - Very Low: if the option is highly irrelevant
   - Low: if the option is not good enough
   - Neutral: if the option is not particularly interesting
   - Good: if the option is worth considering
   - Very Good: for highly relevant options
   ```
   (Numeryczną skalę możesz sobie potem dopisać jako mapowanie.)
6. **Iteruj na promptach** jak na kodzie: mała zmiana, test, porównanie wyników. Modele się zmieniają — prompt, który działał miesiąc temu, może wymagać korekty.

## 17. Prompt optymalizacji — wnioski praktyczne (z akademickiego przeglądu)

Z `concept-deep-dive/prompting/prompt_optimization.md` (taxonomy: retrieval-augmented prompting, continuous tuning, black-box optymalizacja, SPO):

- **RAG prompting** (dodanie zewnętrznej wiedzy do promptu) w czacie = wklejenie dokumentu / użycie Library / włączenie web search. To najtańszy „ulepszacz”.
- **Self-Supervised Prompt Optimization (SPO)**: model sam ocenia własne wyjścia i poprawia prompt — w czacie odtworzysz to jako pętlę krytyka→poprawa (punkt 8).
- Wnioski: prompt wrażliwy na sformułowanie („brittle”); jedna uniwersalna fraza (np. „pomyśl krok po kroku”) często działa szerzej niż wyszukane prompt-y; ensembling promptów (kilka wariantów, wybór najlepszego) łagodzi wrażliwość.

## 18. Szybka ściąga: anatomia dobrego promptu w czacie

```text
[ROLA]        You are a <kto>, your task is to <co>.
[KONTEKST]    (opcjonalnie: kim jest użytkownik, jaki cel)
[DANE]        # Facts / # Document / <<< dane >>>
[STRUKTURA]   # Categories / # Response Format / # Steps
[PRZYKŁADY]   # Examples (2–4 sztuki, w tym edge case)
[OGRANICZENIA] Do not ... / Only respond with ... / Respond in <język>
[FORMAT]      konkretny szablon lub JSON z nazwami kluczy
```

Kolejność ma znaczenie: rola na początku, dane w środku (po delimiterach), format i ograniczenia na końcu (model najmocniej pamięta koniec promptu).
