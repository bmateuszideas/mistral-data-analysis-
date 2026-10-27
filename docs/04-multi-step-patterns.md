# Wzorce wieloetapowe (multi-step patterns) — pełne wersje

Źródła: `mistral/agents/non_framework/agentic_workflows/serial_chain_agentic_workflow.ipynb`, `parallel_subtask_agentic_workflow.ipynb`, `mistral/data_generation/data_generation_refining_news.ipynb`, `mistral/agents/non_framework/transcript_linearticket_agent/TranscriptToLinearTicketAgent.ipynb`, `mistral/agents/non_framework/hubspot_dynamic_multi_agent/`, `mistral/agents/glm_game_generator/01-glm-game-generator.md`.

W czacie webowym nie masz „agentów, które same się wywołują”. Odtwarzasz ich architekturę jako **sekwencję wiadomości** (łańcuch szeregowy), **równoległe czaty** (podzadania) albo **Work z todos** (Vibe sam rozbija zadanie, gdy poprosisz o plan). Poniżej każdy wzorzec jako gotowy przepis.

---

## Wzorzec A: Łańcuch szeregowy (serial chain)

**Kiedy:** złożona transformacja, gdzie każdy etap używa wyniku poprzedniego (blog → wątek, transkrypcja → PRD → tickety, artykuł → krytyka → poprawiona wersja).

**Zasada:** jeden etap = jedna wiadomość, z własnym mini-system-promptem. W następnej wiadomości wklejasz wynik poprzedniej jako zmienną.

### A1. Blog post / transkrypcja → wątek na X/Twitter (4 etapy, z cookbooka)

**Etap 1 — Extract Key Information**

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
{wklej treść}
```

**Etap 2 — Create Thread Structure** (wklej wynik etapu 1 jako `{extracted_info}`)

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
{wynik etapu 1}
```

**Etap 3 — Generate Tweet Thread** (wklej wynik etapu 2 jako `{thread_structure}`)

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
{wynik etapu 2}

Format your response as a numbered list of tweets, with each tweet ready to copy and paste.
```

**Etap 4 — Add Engagement Elements** (wklej wynik etapu 3 jako `{tweet_thread}`)

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
{wynik etapu 3}
```

### A2. Transkrypcja rozmowy → PRD → lista feature'ów (z transcript→ticket)

**Etap 1 — PRD:**

```text
Based on the following call transcript, create an initial Product Requirements Document
(PRD) with some or all of these sections:
1. Title
2. Purpose
3. Scope
4. Features and Requirements
5. User Personas
6. Technical Requirements
7. Constraints
8. Success Metrics
9. Timeline and Milestones

Transcript:
{wklej transkrypcję jako plik lub tekst}

Align everything only with the information provided in the transcript. If any section is
not present in the transcript, you can skip it in the PRD.
```

**Etap 2 — self-review PRD względem transkrypcji:**

```text
Review the following PRD based on the original call transcript. Provide feedback on:
- Missing information in the PRD that is present in the transcript.
- Inconsistencies in the PRD that are not aligned with the transcript.

Transcript: {transkrypcja}
Current PRD: {wynik etapu 1}

Align the feedback only with the information provided in the transcript.
If no feedback is required, respond with "None."
```

**Etap 3 — popraw PRD** (jeśli etap 2 zwrócił uwagi): „Incorporate the feedback into the PRD and provide the full revised PRD. Do not comment on changes.”

**Etap 4 — feature'y do ticketów:**

```text
You are an AI assistant helping to create a Features list and their descriptions from a
Product Requirements Document (PRD). The description should contain a brief explanation
of the feature that includes Technical requirements (if any), Constraints (if any),
Success metrics (if any), User personas (if any), and Timeline and Milestones (if any).

PRD:
{wynik etapu 3}
```

### A3. Artykuł → krytyka wg przewodnika → poprawiony artykuł (z data_generation)

**Etap 1 — krytyka:**

```text
As a 'News Article Editor' adhering to a specific style guide, your responsibility is to
polish and restructure news articles to align with the high standards of clarity, accuracy,
and elegance set by the guide:

{wklej przewodnik stylu}

You are presented with a news article. Identify the ten (or fewer) most significant
stylistic concerns and provide examples of how they can be enhanced.

ARTICLE:
{wklej artykuł}
```

**Etap 2 — przepisanie:**

```text
Now, incorporate the feedback and critiques into the news article and respond with the
enhanced version, focusing solely on stylistic improvements without altering the content.
You must provide the entire article enhanced.
Do not make ANY comments, only provide the new article improved.
Do not tell me what you changed.

ARTICLE:
{oryginał}

FEEDBACK:
{wynik etapu 1}
```

Cookbook radzi też: miej **2–3 wariacje** tego samego promptu systemowego i mieszaj je między przebiegami — wyniki są bardziej zróżnicowane.

---

## Wzorzec B: Orkiestrator → równolegli workerzy → synteza (parallel subtask)

**Kiedy:** zadanie da się rozbić na niezależne części (np. analiza dokumentu z 5 perspektyw, research wielowątkowy, przegląd kodu z kilku kątów).

**W czacie:** etap 1 w jednym czacie; etapy 2 w osobnych czatach (równolegle — Vibe obsługuje wiele otwartych konwersacji); etap 3 w pierwszym czacie z wklejonymi wynikami.

**Etap 1 — orkiestrator (dekompozycja):**

```text
System: You are a task orchestrator that specializes in breaking down complex problems
into smaller, well-defined subtasks that can be solved independently and in parallel.
Think carefully about the most logical way to decompose the given task.

User: Analyze this task and break it down into 3-5 distinct, specialized subtasks that
could be executed in parallel:

Task: {TWOJE_ZADANIE}

For each subtask:
1. Assign a unique task_id
2. Define a specific type that describes the subtask's focus
3. Write a clear description explaining what needs to be done
4. Provide a search query if the subtask requires additional information

First, provide a brief analysis of your understanding of the task.
Then, define the subtasks that would collectively solve this problem effectively.
Remember to make the subtasks complementary, not redundant, focusing on different aspects
of the problem.
```

**Etap 2 — worker (dla każdego podzadania osobny czat):**

```text
System: You are a specialized agent focused on solving a specific aspect of a larger task.

User: Complete the following subtask based on the given information:

Original Task: {zadanie główne}
Subtask Type: {typ}
Subtask Description: {opis}

{opcjonalnie: sekcja „Relevant Information” z wynikami wyszukiwania}

Please provide a detailed response for this specific subtask only.
```

**Etap 3 — synteza:**

```text
System: You are a synthesis expert. Combine multiple partial analyses into one coherent,
structured final report.

User: Here are the results from {N} parallel analyses of the task "{zadanie}".
Synthesize them into a single final report. Resolve contradictions explicitly.
Do not add information that is not present in the partial results.

ANALYSIS 1 ({typ}):
{wynik 1}

ANALYSIS 2 ({typ}):
{wynik 2}
...
```

---

## Wzorzec C: Router / triage (wybór ścieżki)

**Kiedy:** jedno wejście, wiele możliwych intencji; dalej każda intencja ma własną procedurę.

**W czacie:** najpierw klasyfikacja (może być wbudowana — Work sam rozpoznaje intencję), potem wykonanie ścieżki.

```text
Step 1: Categorize the user's request into exactly one of:
- summary: user wants a summary of provided material
- extraction: user wants structured data extracted from provided material
- analysis: user wants analysis, comparison, or evaluation
- writing: user wants new content written or rewritten
- research: user wants current information from the web
- other: none of the above

Respond with the category only.

Step 2 (następna wiadomość): wykonaj zadanie zgodnie z kategorią, używając odpowiedniej
procedury (każda kategoria = osobny prompt z docs/02 lub osobny skill).
```

Pełny klasyfikator bankowy (6 kategorii + fallback) znajdziesz w `docs/02` (punkt 12) i w `prompts/tasks/classify-intent.md`.

---

## Wzorzec D: Plan → walidacja → wykonanie (plan-validate-execute)

**Kiedy:** operacje ryzykowne lub wieloobiektowe (zmiany w wielu rekordach, migracje, wysyłki, publikacje).

```text
Etap 1 — plan:
Analyze the request and produce a structured plan as a numbered list.
For each step include: action, target, and expected result.
Do not execute anything yet.

Etap 2 — walidacja (użytkownik lub model-sędzia):
Check the plan against these rules:
- Does every step have a clear expected result?
- Are there any steps that could destroy or modify data irreversibly? Flag them.
- Is the order of steps correct (dependencies respected)?

Etap 3 — wykonanie (po akceptacji planu):
Execute the approved plan step by step. After each step, report the result.
Stop and ask before any irreversible action.
```

Wersja z agentskills.io („plan-validate-execute” dla batch/destrukcyjnych operacji): najpierw plan w strukturze, walidacja względem źródła prawdy (checklisty/skryptu), dopiero potem wykonanie.

---

## Wzorzec E: Pętla review-and-fix (self-correction loop)

**Kiedy:** generowanie kodu, długich dokumentów, czegokolwiek, gdzie pierwsza wersja ma błędy.

Z cookbooka (glm_game_generator): wygeneruj → przejrzyj pod kątem listy konkretnych failure-modów → popraw.

```text
Etap 1: Wygeneruj {kod/dokument} zgodnie z wymaganiami.

Etap 2 (ta sama lub kolejna wiadomość):
Review what you generated against this checklist:
- [ ] Każdy przeciwnik ma właściwość health, która maleje przy trafieniu
- [ ] Kolizje są sprawdzane dla każdej encji (x, y, width, height)
- [ ] ...
List every violation you find, then provide the corrected full version.

Etap 3 (jeśli nadal są naruszenia): powtórz etap 2, maks. 2–3 iteracje.
```

**Wzorzec ogólny „krytyk”:** „Przejrzyj własny wynik jak adwokat diabła: znajdź 3 najsłabsze punkty i zaproponuj konkretne poprawki.”

---

## Wzorzec F: Lead agent → dynamiczne sub-agenty → synteza (HubSpot)

**Kiedy:** zapytanie jest złożone i nie wiadomo z góry, jakie podzadania będą potrzebne.

```text
Etap 1 — Lead (analiza intencji):
Analyze the user's request. Identify the goal, the entities involved, and what data or
actions are needed. Output: a short plan (max 5 steps) naming the sub-tasks.

Etap 2 — sub-agenty (po jednym na krok planu, w kolejnych wiadomościach/czatach):
Dla każdego kroku: "Execute step {n}: {opis}. Report the result in a structured format."

Etap 3 — Synthesis:
Combine the results of all steps into a final answer for the user: executive summary,
details, and any recommended actions.
```

---

## Wzorzec G: Wiele perspektyw na jeden dokument (MAECAS-style)

Jeden dokument (transkrypcja, raport, umowa), N analiz równoległych, jedna synteza.

```text
W czacie: wklej dokument jako plik. Potem wyślij N osobnych promptów (lub N osobnych czatów):

Perspektywa 1 — finansowa:
  "You are a financial analyst... Focus on: revenue, margins, growth, guidance, capex,
   cash flow. Extract only facts explicitly stated in the document."

Perspektywa 2 — strategiczna:
  "You are a business strategy analyst... Focus on: roadmaps, market expansion,
   partnerships, R&D, vision. Assign importance 1-5."

Perspektywa 3 — sentyment:
  "You are an expert in analyzing sentiment and tone... Focus on: confidence, enthusiasm,
   concerns, changes in tone. Cite evidence from the document."

Perspektywa 4 — ryzyka:
  "You are a risk analyst... Focus on: supply chain, market uncertainty, competition,
   regulatory, technical, execution, macro. Assign severity 1-5 with mitigation."

Perspektywa 5 — konkurencja:
  "You are a competitive intelligence analyst... Focus on: competitor mentions, market
   share, advantages/disadvantages, differentiation. Note the speaker."

Synteza:
  "Combine the five analyses into one structured report with an executive summary,
   a comparison table, risks, and recommendations."
```

Pełne wersje każdej perspektywy: `docs/03-agent-system-prompt-patterns.md`.

---

## Jak wybierać wzorzec

| Sytuacja | Wzorzec |
|---|---|
| Transformacja A→B→C→D, każdy etap żywi się poprzednim | **A: łańcuch szeregowy** |
| Zadanie = suma niezależnych części | **B: orkiestrator + workerzy + synteza** |
| Nie wiesz, co użytkownik chce, wiele intencji | **C: router** |
| Zmiany/wysyłki/migracje — ryzyko błędu | **D: plan → walidacja → wykonanie** |
| Kod/dokumenty, pierwsza wersja ma błędy | **E: review-and-fix** |
| Złożone zapytanie, plan zależy od danych | **F: lead → sub-agenty → synteza** |
| Jeden dokument, wiele punktów widzenia | **G: wiele perspektyw** |

Kilka wzorców można łączyć: np. C (router) → B (równoległe analizy) → A etap syntezy. W Vibe Work część tej pracy wykona za Ciebie sam Work (todos + własny plan) — Twoja rola to dobry opis wyniku i ewentualnie jawna prośba o plan przed wykonaniem.
