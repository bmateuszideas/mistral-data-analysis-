# Mapa źródłowa: `mistralai/cookbook` → to repo

Pełny clone cookbooka (607 plików) został przejrzany 27 października 2026. Poniżej każda istotna ścieżka, co z niej wyciągnięto i gdzie to wylądowało w tym repo.

## Skills oficjalne

| Ścieżka w cookbooku | Co zawiera | Gdzie w tym repo |
|---|---|---|
| `skills/cookbook-review/` | Kompletny skill recenzowania dokumentacji: `SKILL.md` + `voice-and-tone.md`, `checklists.md`, `ai-terms.md`, `developer-content.md`, `inclusive-language.md` + skrypt `review_pr.py` | Cały przewodnik stylu → `docs/06-mistral-writing-style.md`; wzorzec struktury przeglądu → `skills/document-review/SKILL.md`, `templates/document-review.md` |
| `skills/check-stale-cookbooks/` | Skill audytu przestarzałych treści + `check_stale.py` | Lista wzorców „starych” jako checklista audytowa → `skills/stale-content-audit/SKILL.md`, `docs/08-evaluation-and-quality.md` |

## Prompting i koncepcje

| Ścieżka w cookbooku | Co zawiera | Gdzie w tym repo |
|---|---|---|
| `mistral/prompting/prompting_capabilities.ipynb` | 4 techniki: klasyfikacja, streszczenie, personalizacja, ewaluacja | Pełne prompty + strategie → `docs/02-prompting-techniques.md`, `prompts/tasks/` (classify-intent, summarize-document, personalized-response, evaluate-output) |
| `mistral/prompting/prefix_use_cases.ipynb` | Prefix (prefill): wymuszanie języka, oszczędność tokenów, roleplay, anty-jailbreak | Odpowiedniki działające w czacie → `docs/02` (punkt 7), `prompts/system/language-lock.md`, `prompts/system/safe-assistant-sandwich.md`, `prompts/tasks/roleplay.md` |
| `concept-deep-dive/prompting/prompt_optimization.md` | Akademicki przegląd optymalizacji promptów (SPO i in.) | Wnioski praktyczne → `docs/02` |
| `concept-deep-dive/sampling/` | temperature, top_p, top_k, min_p, kary za powtórzenia | Jak to przekładać na wybór trybu fast/think w Vibe web → `docs/02` (tabela temperatur), `docs/10` |
| `concept-deep-dive/tokenization/` | Tokenizery v1/v2/v3/Tekken, chat templates, control tokens | Dlaczego system prompt działa tak a nie inaczej; limity kontekstu → `docs/02` |

## Agenci — definicje API (wyciągnięte same instrukcje)

| Ścieżka w cookbooku | Co zawiera | Gdzie w tym repo |
|---|---|---|
| `mistral/agents/agents_api/travel_assistant/` | Agent triage (router) | Wzorzec routera → `docs/03`, `docs/04` (wzorzec C), `workflows/intent-router.md` |
| `mistral/agents/agents_api/food_diet_companion/` | Agenci tool-specialist | Wzorce wąskich specjalistów → `docs/03` |
| `mistral/agents/agents_api/` (web search agent) | `temperature 0.3`, `top_p 0.95` | Uzasadnienie doboru trybu think dla researchu → `docs/02`, `docs/10` |
| `mistral/agents/agents_api/github_agent/` | Agent przeglądu kodu | → `docs/03`, `skills/code-review/SKILL.md` |
| `mistral/agents/agents_api/financial_analyst/` | Agent finansowy | → `docs/03` |
| `mistral/agents/agents_api/prd_linear_ticket/` | PRD → tickety | → `docs/04` (A2), `workflows/transcript-to-prd.md`, `templates/prd.md`, `templates/feature-list.md` |
| `mistral/agents/agents_api/podcast_research_briefing/` | Research → briefing | → `docs/03`, `skills/research-brief/SKILL.md` |

## Agenci — non-framework (architektury jako przepisy czatowe)

| Ścieżka w cookbooku | Co zawiera | Gdzie w tym repo |
|---|---|---|
| `mistral/agents/non_framework/agentic_workflows/serial_chain_agentic_workflow.ipynb` | Łańcuch szeregowy (blog → wątek, 4 kroki) | → `docs/04` (A1), `workflows/serial-chain-content-repurposing.md` |
| `mistral/agents/non_framework/agentic_workflows/parallel_subtask_agentic_workflow.ipynb` | Orkiestrator + równolegli workerzy + synteza | → `docs/04` (B), `workflows/orchestrator-parallel-synthesis.md` |
| `mistral/agents/non_framework/earnings_calls/` (MAECAS) | 5 agentów równoległych: financial, strategic, sentiment, risk, competitor + temporal | Pełne prompty perspektyw → `docs/03`, `docs/04` (G), `workflows/multi-perspective-analysis.md`, `templates/analysis-report.md` |
| `mistral/agents/non_framework/recruitment_agent/` | JobAnalysisAgent, ResumeAnalysisAgent, MatchingAgent (rubryka 40/30/15/15), EmailCommunicationAgent | Wzorce ekstrakcji strukturalnej i scoringu → `docs/03`, `skills/structured-extraction/SKILL.md`, `skills/scoring-rubric/SKILL.md` |
| `mistral/agents/non_framework/transcript_linearticket_agent/` | PRDAgent (z pętlą feedbacku) + TicketCreationAgent | → `docs/04` (A2), `workflows/transcript-to-prd.md`, `prompts/tasks/generate-prd.md` |
| `mistral/agents/non_framework/hubspot_dynamic_multi_agent/` | Dynamiczny multi-agent (lead → sub-agenty) | Architektura → `docs/04` (F) |
| `mistral/agents/non_framework/industrial_knowledge_agent/` | RAG + function calling agent przemysłowy | Koncepcje: cytowanie źródeł, „odpowiadaj tylko na podstawie kontekstu” → `docs/07`, `prompts/system/grounded-qa.md` |
| `mistral/agents/glm_game_generator/01-glm-game-generator.md` | Generowanie kodu z review loop; system = ograniczenia, user = wymagania | → `docs/02`, `docs/04` (E), `skills/code-review/SKILL.md` |
| `mistral/agents/glm_code_modernizer/` | Modernizacja kodu z pętlą review | → `docs/04` (E) |

## Moderacja, generowanie danych, ewaluacja

| Ścieżka w cookbooku | Co zawiera | Gdzie w tym repo |
|---|---|---|
| `mistral/moderation/system-level-guardrails.ipynb` | Moderacja 9 kategorii; generowanie N wariantów + wybór najbezpieczniejszego | Wzorzec „wygeneruj kilka wariantów, wybierz najlepszy” → `docs/08`, `prompts/tasks/evaluate-output.md` |
| `mistral/data_generation/data_generation_refining_news.ipynb` | Pipeline: krytyka wg przewodnika → przepisanie; wariacje system promptów; retry z backoff | → `docs/04` (A3), `workflows/critique-refine-loop.md`, `skills/critique-and-rewrite/SKILL.md` |
| `mistral/evaluation/evaluation.ipynb` | Ewaluacja: dokładność na danych golden | → `docs/08` |
| `mistral/evaluation/RAG_evaluation.ipynb` | LLM-as-judge: context_relevance, answer_relevance, groundedness, skala 0–3 | Pełny prompt sędziego → `docs/08`, `prompts/tasks/evaluate-output.md` |

## Dokumenty, obrazy, dane, RAG, connectors

| Ścieżka w cookbooku | Co zawiera | Gdzie w tym repo |
|---|---|---|
| `mistral/ocr/` (5 notebooków: document_understanding, tool_usage, data_extraction, documentChunking, product_datasheet_analysis) | OCR dokumentów, narzędzie `open_urls`, chunking dużych PDF | Wzorce pracy z dokumentami w czacie → `docs/07` |
| `mistral/image_understanding/` | Opis obrazu wg schematu JSON, ekstrakcja tabeli z obrazu | → `docs/07`, `prompts/tasks/extract-structured-data.md` |
| `mistral/rag/` (basic_RAG, mistral-search-engine, RAG_via_function_calling, mistral-reference-rag) | RAG od zera, router RAG, web search agent | Koncepcje → `docs/02`, `docs/07` |
| `mistral/connectors/` (7 przepisów + quickstart-generator + database-advisor) | Łączenie narzędzi: web search + Context7/DeepWiki; porównanie jakości odpowiedzi wraz z kolejnymi źródłami | Zasada „więcej dobrych źródeł = lepsza odpowiedź” → `docs/07` |
| `mistral/lechat_custom_mcp_server/` | Własny serwer MCP podłączony do Vibe | Kontekst: w web Vibe podłączasz gotowe Connectors → `docs/01` |

## Celowo pominięte z cookbooka

| Ścieżka | Powód pominięcia |
|---|---|
| `third_party/` (37 integracji: Streamlit, Chainlit, LangChain, LlamaIndex, CrewAI...) | Wymagają kodu/frameworków — niedziałające w czacie webowym; ich wzorce promptowe włączone tam, gdzie dotyczą czatu |
| `mistral/finetuning/`, `mistral/classifier/`, `mistral/batch/` | Wymagają API/kluczy/Studio jobs |
| `mistral/audio/`, `mistral/ocr/` (wersje API z kluczami) | Usługi API, nie czat webowy |
| `mistral/deployment/` (vLLM, self-hosting) | Self-hosting poza zakresem |
| `mistral/observability/`, `mistral/admin/` | Admin API, billing — poza zakresem |
| Notebooki API--only w `mistral/ocr/`, `mistral/rag/` | Kod SDK — wyciągnięto tylko wzorce promptowe i architektoniczne |

## Jak korzystać z tej mapy

- Chcesz zobaczyć oryginalny materiał dla konkretnej techniki → ścieżka w kolumnie 1 (repo: `github.com/mistralai/cookbook`).
- Chcesz zobaczyć, co z tego zostało wyciągnięte → kolumna 3 (pliki w tym repo).
- Szukasz czegoś, czego tu nie ma → sprawdź najpierw kolumnę „Celowo pominięte” — być może temat jest poza zakresem czatu webowego.
