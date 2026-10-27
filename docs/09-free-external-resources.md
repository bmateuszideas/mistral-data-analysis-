# Darmowe zasoby zewnętrzne — kompletna lista

Wszystkie poniższe zasoby są **darmowe** i albo wprost kompatybilne z Mistral Vibe, albo w formacie `SKILL.md` (agentskills.io), który Vibe przyjmuje. Stan na październik 2026 — przed użyciem zweryfikuj aktualność.

## Standard i dokumentacja referencyjna

| Zasób | Co to jest | Link |
|---|---|---|
| **Agent Skills — specyfikacja** | Oficjalny otwarty standard formatu `SKILL.md` (katalog + frontmatter YAML + progressive disclosure). **Mistral Vibe oficjalnie go wspiera.** | [agentskills.io/specification](https://agentskills.io/specification) |
| **Agent Skills — best practices** | Jak pisać dobre skille: z realnych zadań, budżet kontekstu, gotchas, szablony, checklisty, pętle walidacji | [agentskills.io/skill-creation/best-practices](https://agentskills.io/skill-creation/best-practices) |
| **Agent Skills — llms.txt** | Indeks dokumentacji standardu dla agentów | [agentskills.io/llms.txt](https://agentskills.io/llms.txt) |
| **Dokumentacja Mistral (cała jako jeden plik)** | Pełna dokumentacja Mistral w markdown — idealna jako kontekst do wklejenia | [docs.mistral.ai/llms.txt](https://docs.mistral.ai/llms.txt) |
| **Context7 — cookbook** | Indeksowana treść mistralai/cookbook (100k tokenów) | [context7.com/mistralai/cookbook/llms.txt](https://context7.com/mistralai/cookbook/llms.txt) |
| **Context7 — ogólnie** | Biblioteka dokumentacji dla LLM; w Vibe możesz podłączyć Context7 jako Connector (oficjalna dokumentacja przez MCP) | [context7.com](https://context7.com) |
| **Mistral Help Center** | Oficjalny przewodnik użytkownika Vibe/Le Chat | [help.mistral.ai](https://help.mistral.ai) |
| **Mistral Cookbook (GitHub)** | Oficjalne przykłady — źródło całej tej bazy | [github.com/mistralai/cookbook](https://github.com/mistralai/cookbook) |

## Biblioteki gotowych Skills (format agentskills.io — kopiuj `SKILL.md`)

| Repo | Liczba | Uwagi |
|---|---|---|
| [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 380+ skills, 30+ agentów, 70+ komend | **Natywnie kompatybilne z Mistral Vibe** (Vibe CLI: `./scripts/vibe-install.sh` → `~/.vibe/skills/`). Kategorie: inżynieria, marketing, produkt, compliance, doradztwo C-level, research, operacje biznesowe, finanse. Największa darmowa biblioteka. |
| [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 1000+ | Wyselekcjonowane skille od oficjalnych zespołów deweloperskich i społeczności. Kompatybilne z Claude Code, Codex, Gemini CLI, Cursor i in. |
| [skillmatic-ai/awesome-agent-skills](https://github.com/skillmatic-ai/awesome-agent-skills) | katalog | Lista platform (w tym Mistral Vibe) i kolekcji; przewodniki „czym są skills”, „Skills vs MCP”. |
| [heilcheng/awesome-agent-skills](https://github.com/heilcheng/awesome-agent-skills) | lista | Wyselekcja jakościowa (realne użycia), nie ilość. |
| [uhub/awesome-agent-skills](https://github.com/uhub/awesome-agent-skills) | lista | Frameworki, biblioteki, katalogi skills. |
| [agensi.io](https://www.agensi.io) | 2000+ skills | Katalog + referencja formatu + instalator. |
| [awesomeclaude.ai/awesome-claude-skills](https://awesomeclaude.ai/awesome-claude-skills) | 240+ | Katalog z komendami instalacji: `npx skills add owner/repo --skill nazwa` (działa też dla innych agentów wspierających format). |
| [github.com/agentskills/agentskills](https://github.com/agentskills/agentskills) | — | Repozytorium samego standardu (specyfikacja + `skills-ref` do walidacji). |

**Jak użyć skilla z GitHuba w Vibe Work (web):** otwórz `SKILL.md` na GitHubie → skopiuj treść (frontmatter + body) → w Vibe: `Context > Skills > New Skill` → wklej. Alternatywa dla Vibe CLI: pobierz katalog skilla do `~/.vibe/skills/<nazwa>/`.

## Gotowe prompty i system instructions

| Zasób | Co znajdziesz | Link |
|---|---|---|
| **docsbot.ai — Mistral prompts** | Darmowe, gotowe system prompty „Mistral agent setup instructions”, „Mistral agent prompt system” — kopiuj-wklej | [docsbot.ai/prompts/technical/mistral-agent-setup-instructions](https://docsbot.ai/prompts/technical/mistral-agent-setup-instructions) |
| **Prompt Architects — 30 szablonów Mistral (2026)** | Copy-paste szablony pod aktualne modele (Medium 3.5, Small 4, Large 3, Ministral 3), z przełącznikiem reasoning i szablonem czatu | [prompt-architects.com/blog/338-mistral-prompt-templates](https://prompt-architects.com/blog/338-mistral-prompt-templates) |
| **Prompt engineering library (community)** | Szersze biblioteki promptów ogólnego użytku | wyszukaj „prompt library free 2026” |

## Przewodniki praktyczne (community)

| Zasób | Temat |
|---|---|
| Reddit r/MistralAI — „Le Chat Deep Dive Series — Chapter One: Agents” | Praktyczny tutorial tworzenia agentów (legacy Chat): pola agenta, temperatura ~0.15–0.2, testowanie w panelu podglądu |
| Reddit r/MistralAI — wątki o agentach | Wskazówki użytkowników Pro (wybór modelu, workflow marketingowy) |
| YouTube — „How to Create AI Agents in Mistral Le Chat with CUSTOM Instructions & Settings” | Wideo-tutorial: agenci + custom instructions |
| [techjacksolutions.com — How to Use Mistral & Le Chat (2026)](https://techjacksolutions.com/ai-tools/mistral/how-to-use-mistral/) | Kompletny przewodnik praktyka: konto, pierwsza rozmowa, web search, Canvas, obrazy, agenci, ceny |
| [qwe.edu.pl — Mistral AI Now Summit: Vibe Tutorial (2026)](https://www.qwe.edu.pl/tutorial/mistral-ai-now-summit-vibe-tutorial/) | Notatki z premiery Vibe: Work Mode, Skills, różnica subskrypcja Vibe vs API |
| [learn.techwithdarin.com/guides/mistral](https://learn.techwithdarin.com/guides/mistral/) | „Practitioner's Field Guide” — Vibe vs La Plateforme, modele, rebrand |

## Narzędzia i playgroundy

| Narzędzie | Do czego |
|---|---|
| [artefact2.github.io/llm-sampling](https://artefact2.github.io/llm-sampling/index.xhtml) | Wizualizacja samplerów (temperature, top-p, top-k) — pomoc przy zrozumieniu, jak „myśli” model (wskazane w cookbooku `concept-deep-dive/sampling`) |
| Mistral Studio Playground | Testowanie promptów z różnymi modelami i parametrami (wymaga konta; samo playground bez klucza API w wersji demo) — [console.mistral.ai](https://console.mistral.ai) |
| Context7 / DeepWiki (MCP) | Oficjalna dokumentacja dowolnej biblioteki jako źródło wiedzy — w Vibe podłącz jako Connector |

## Jak wybierać zasoby (zasady bezpieczeństwa)

1. **Skills z GitHuba = tekst, który trafia do kontekstu modelu.** Czytaj `SKILL.md` przed włączeniem; unikaj skillów z `scripts/` wykonujących nieznany kod (w Vibe web skrypty i tak nie mają jak działać — to zaleta wersji webowej).
2. **Preferuj wyselekcjonowane listy** (VoltAgent, heilcheng) nad „pierwszym lepszym repo z 5000 skillami”.
3. **alirezarezvani/claude-skills** jest największe, ale przy 380+ skillów wybieraj pojedyncze — nie instaluj wszystkiego naraz (każdy opis zajmuje ~100 tokenów kontekstu na starcie sesji).
4. **Wersjonuj własne kopie** — zapisz używane skille we własnym repo (tak jak to), żeby aktualizacje upstream nie psuły Twoich promptów.
5. **Sprawdzaj daty** — ekosystem skills ruszył w grudniu 2025; część repo jest świeża i mało przetestowana.
