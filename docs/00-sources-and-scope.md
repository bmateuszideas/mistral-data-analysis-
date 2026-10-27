# Źródła i zakres (sources and scope)

## Kiedy powstało

Repo utworzone i wypełnione: **27 października 2026** (stan wiedzy na ten dzień).

## Źródła pierwotne (wszystko zweryfikowane bezpośrednio, nie z pamięci)

### 1. `github.com/mistralai/cookbook` (oficjalny cookbook Mistral)

Pełny clone repozytorium; przejrzane wszystkie sekcje istotne dla wersji czatowej:

| Ścieżka w cookbooku | Co zawiera | Co z tego wyciągnięto |
|---|---|---|
| `skills/cookbook-review/` | kompletny skill recenzowania dokumentacji: `SKILL.md` + `voice-and-tone.md`, `checklists.md`, `ai-terms.md`, `developer-content.md`, `inclusive-language.md` + skrypt `review_pr.py` | cały przewodnik stylu Mistral (→ `docs/06`), wzorzec struktury przeglądu dokumentu (→ `templates/`, `skills/`) |
| `skills/check-stale-cookbooks/` | skill audytu przestarzałych treści + `check_stale.py` | listę wzorców "starych" (deprecatet SDK v0, stare modele) jako checklistę audytową (→ `docs/08`, `skills/`) |
| `mistral/prompting/prompting_capabilities.ipynb` | 4 techniki: klasyfikacja, streszczenie, personalizacja, ewaluacja | pełne prompty + strategie (few-shot, delimitery, rola, step-by-step, JSON, łańcuch LLM) (→ `docs/02`, `prompts/tasks/`) |
| `mistral/prompting/prefix_use_cases.ipynb` | prefix (prefill): wymuszanie języka, oszczędność tokenów, roleplay, anty-jailbreak | odpowiedniki działające w czacie (→ `docs/02`, `prompts/system/`) |
| `concept-deep-dive/prompting/prompt_optimization.md` | akademicki przegląd optymalizacji promptów (SPO i in.) | kontekst teoretyczny; wnioski praktyczne (→ `docs/02`) |
| `concept-deep-dive/sampling/` | temperature, top_p, top_k, min_p, repetition/presence/frequency penalty | zasady doboru "temperatury" i jak to przekładać na wybór trybu fast/think w Vibe web (→ `docs/02`) |
| `concept-deep-dive/tokenization/` | tokenizery v1/v2/v3/Tekken, chat templates, control tokens | kontekst: dlaczego system prompt działa tak a nie inaczej; uwaga na limity kontekstu (→ `docs/02`) |
| `mistral/agents/agents_api/` (travel_assistant, food_diet_companion, financial_analyst, github_agent, podcast_research_briefing, prd_linear_ticket) | definicje agentów: `instructions`, `description`, `tools`, handoffs, completion_args (temperature 0.3, top_p 0.95) | wszystkie system prompty agentów — gotowe wzorce (→ `docs/03`) |
| `mistral/agents/non_framework/agentic_workflows/` (serial_chain, parallel_subtask) | łańcuch szeregowy (blog→Twitter thread, 4 kroki) i równoległe podzadania (orkiestrator + workerzy + synteza) | wzorce wieloetapowe jako przepisy czatowe (→ `docs/04`, `workflows/`) |
| `mistral/agents/non_framework/earnings_calls/` (MAECAS) | 5 agentów równoległych: financial, strategic, sentiment, risk, competitor + temporal | system prompty analityczne ze strukturą JSON i skalami (→ `docs/03`) |
| `mistral/agents/non_framework/recruitment_agent/` | JobAnalysisAgent, ResumeAnalysisAgent, MatchingAgent (rubryka 40/30/15/15), EmailCommunicationAgent | wzorce ekstrakcji strukturalnej i scoringu (→ `docs/03`, `prompts/tasks/`) |
| `mistral/agents/non_framework/transcript_linearticket_agent/` | PRDAgent (z pętlą feedbacku) + TicketCreationAgent | szablon PRD, pętla "wygeneruj → oceń zgodność → popraw" (→ `docs/04`, `templates/`) |
| `mistral/agents/non_framework/hubspot_dynamic_multi_agent/`, `industrial_knowledge_agent/` | dynamiczny multi-agent, RAG+function calling agent przemysłowy | architektury: lead agent → sub-agenty → synteza (→ `docs/04`) |
| `mistral/agents/glm_game_generator/`, `glm_code_modernizer/` | generowanie kodu z review loop | wzorzec: system = ograniczenia formatu, user = opis + konkretne wymagania (→ `docs/02`) |
| `mistral/moderation/system-level-guardrails.ipynb` | moderacja 9 kategorii, generowanie N wariantów + wybór najbezpieczniejszego | wzorzec "wygeneruj kilka wariantów, wybierz najlepszy" (→ `docs/08`) |
| `mistral/data_generation/data_generation_refining_news.ipynb` | pipeline: krytyka wg przewodnika → przepisanie z uwzględnieniem krytyki; wariacje system promptów; retry z exponential backoff | wzorzec krytyka→poprawa (→ `docs/04`, `workflows/`) |
| `mistral/evaluation/evaluation.ipynb`, `RAG_evaluation.ipynb` | ewaluacja: dokładność na danych golden, LLM-as-judge ze strukturą (context_relevance, answer_relevance, groundedness, skala 0–3) | wzorce oceny jakości odpowiedzi (→ `docs/08`, `prompts/tasks/`) |
| `mistral/image_understanding/` | opis obrazu wg schematu JSON, ekstrakcja tabeli z obrazu | wzorce pracy z obrazami w czacie (→ `docs/07`) |
| `mistral/ocr/` (document_understanding, tool_usage, data_extraction, documentChunking, product_datasheet_analysis) | OCR dokumentów, narzędzie `open_urls`, chunking dużych PDF | wzorce pracy z dokumentami w czacie (→ `docs/07`) |
| `mistral/rag/` (basic_RAG, mistral-search-engine, RAG_via_function_calling, mistral-reference-rag) | RAG od zera, router RAG, web search agent | koncepcje: cytowanie źródeł, router intencji, „odpowiadaj tylko na podstawie kontekstu” (→ `docs/02`, `docs/07`) |
| `mistral/connectors/` (7 przepisów + quickstart-generator + database-advisor) | łączenie narzędzi: web search + Context7/DeepWiki; porównanie jakości odpowiedzi wraz z kolejnymi źródłami | uzasadnienie zasady „więcej dobrych źródeł = lepsza odpowiedź" (→ `docs/07`) |
| `mistral/lechat_custom_mcp_server/` | własny serwer MCP (kółko i krzyżyk) podłączony do Vibe | kontekst: w web Vibe podłączasz **gotowe Connectors**, nie budujesz serwerów (→ `docs/01`) |
| `third_party/` (37 integracji) | Streamlit, Chainlit, LangChain, LlamaIndex, CrewAI, etc. | celowo pominięte (UI/frameworki), ale ich wzorce promptowe włączone tam, gdzie dotyczą czatu |

### 2. `docs.mistral.ai` (oficjalna dokumentacja)

Pobrane: pełny `llms.txt` oraz strony w wersji `.md`:

- `inference/prompting.md` — oficjalny przewodnik promptowania (system vs user, rola, struktura, formatowanie, few-shot, strukturyzowane wyjście, lista „czego unikać", 4 pełne przykłady promptów) → `docs/02`
- `inference/sampling.md` — parametry próbkowania (N, temperature, top_p, presence/frequency penalty) → `docs/02`
- `getting-started/platform-overview.md` — trzy produkty: Vibe / Studio / Admin; migracja Le Chat → Vibe; Chat tab wyłączany; Agenci → Skills (wywołanie `@`) → `docs/01`
- `vibe/work/get-started.md` — start zadania w Work, kontekst (Connectors, Libraries, Skills, Files/Canvas, web search), todos, zasady bezpieczeństwa, tabela „co użyć kiedy” → `docs/01`, `docs/10`
- `vibe/work/skills.md` — jak działają Skills, progressive disclosure, lista 12 wbudowanych skills, tworzenie z edytora i z czatu, udostępnianie → `docs/05`
- `vibe/work/custom-instructions.md` — trwałe preferencje, priorytet Skill > custom instructions, przykłady, czego unikać → `docs/01`, `docs/10`
- `vibe/work/projects.md` — projekty: ton, instrukcje, pliki, czaty w projekcie → `docs/01`, `docs/10`
- `vibe/work/workflows.md` — Workflows (zbudowane w Studio, wywoływane z `+`), różnica Skill vs Workflow → `docs/01`
- `getting-started/quickstarts/studio/create-skill.md` — skill w Studio: opis jako warunek, instrukcje, pliki, `Publish to Vibe`, wersjonowanie → `docs/05`
- `getting-started/quickstarts/vibe-work/create-first-skill.md` — pierwszy skill w Vibe Work: formularz (Title/Description/SKILL.md), przykład „Meeting summary” → `skills/`
- `le-chat/knowledge-integrations/agents.md` — legacy Agenci w Chat (tworzenie tylko na web/desktop, pola: nazwa, opis, instrukcje, temperatura ~0.15–0.2, narzędzia, wiedza) → `docs/01`, `docs/03`

### 3. `context7.com/mistralai/cookbook/llms.txt`

Indeksowana pełna treść cookbooka (100k tokenów) — użyte do weryfikacji kompletności przeglądu.

### 4. `agentskills.io` (otwarty standard Agent Skills)

- `agentskills.io/specification` — pełna specyfikacja formatu `SKILL.md` (frontmatter, katalogi, progressive disclosure, walidacja `skills-ref`) → `docs/05`
- `agentskills.io/skill-creation/best-practices` — best practices: buduj z realnych zadań, „spending context wisely”, gotchas, szablony, checklisty, pętle walidacji, plan-validate-execute → `docs/05`

### 5. Darmowe zasoby zewnętrzne (wszystkie zweryfikowane przez wyszukiwanie)

- `github.com/alirezarezvani/claude-skills` — 380+ skills, 30+ agentów; **natywnie kompatybilne z Mistral Vibe** (ten sam standard agentskills.io; instalacja `./scripts/vibe-install.sh` do `~/.vibe/skills/`)
- `github.com/VoltAgent/awesome-agent-skills` — 1000+ wyselekcjonowanych skills
- `github.com/skillmatic-ai/awesome-agent-skills`, `github.com/heilcheng/awesome-agent-skills`, `github.com/uhub/awesome-agent-skills` — katalogi i listy
- `agensi.io` — 2000+ skills + referencja formatu SKILL.md
- `awesomeclaude.ai/awesome-claude-skills` — katalog z komendami instalacji (`npx skills add owner/repo --skill nazwa`)
- `docsbot.ai/prompts/technical/mistral-agent-*` — darmowe gotowe system prompty
- `prompt-architects.com/blog/338-mistral-prompt-templates` — 30 szablonów promptów dla modeli Mistral (2026)
- `help.mistral.ai` — oficjalny przewodnik użytkownika Vibe/Le Chat
- Reddit r/MistralAI — wątki praktyczne o agentach (m.in. tutorial „Le Chat Deep Dive — Agents”)
- YouTube: „How to Create AI Agents in Mistral Le Chat with CUSTOM Instructions & Settings”

## Celowo POMINIĘTE (poza zakresem — decyzja projektowa)

To nie jest lenistwo — te tematy wymagają API/kodu/kluczy i **nie działają w czacie webowym**:

- API/SDK (Python, TypeScript, curl), klucze API, endpointy
- Function calling jako kod, własne narzędzia w Pythonie
- RAG produkcyjny z wektorówką (FAISS, Pinecone, Chroma, Milvus, Neo4j, Qdrant...)
- Fine-tuning, klasyfikatory, batch API, jobs
- Audio realtime (Voxtral), TTS/STT jako usługa
- Self-hosting (vLLM), kwantyzacja (EXL2/GGUF), tokenizery jako kod
- Budowanie własnych serwerów MCP (w Vibe web podłączasz gotowe Connectors)
- UI: Streamlit, Chainlit, Panel, Gradio, Mesop, Solara
- Frameworki third_party: LangChain, LlamaIndex, CrewAI, CAMEL, AutoGen, PydanticAI...
- Observability (Langfuse, Phoenix, MLflow...), Admin API, billing
- Moderacja jako API (w czacie są wbudowane zabezpieczenia)

**Ale ich wzorce promptowe i architektoniczne zostały włączone** tam, gdzie da się je odtworzyć w czacie — to najcenniejsza część cookbooka.

## Jak czytać dalej

- Chcesz **używać** Vibe lepiej → `docs/10-vibe-web-usage-playbook.md`
- Chcesz **pisać prompty** → `docs/02-prompting-techniques.md`
- Chcesz **budować agentów/skills** → `docs/03` + `docs/05`
- Chcesz **wzorce wieloetapowe** → `docs/04` + `workflows/`
- Chcesz **pisać dokumenty/instrukcje profesjonalnie** → `docs/06`
- Chcesz **analizować dokumenty/obrazy/dane** → `docs/07`
- Chcesz **oceniać jakość odpowiedzi** → `docs/08`
- Szukasz **gotowych zasobów** → `docs/09`
