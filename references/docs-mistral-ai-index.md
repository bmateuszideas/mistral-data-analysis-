# Indeks źródłowy: `docs.mistral.ai` → to repo

Stan na 27 października 2026. Pobrano pełny `llms.txt` oraz strony w wersji `.md`. Poniżej każda strona, jej zawartość i gdzie została użyta w tym repo.

## Platforma i produkty

| Strona | Zawartość | Gdzie w tym repo |
|---|---|---|
| `getting-started/platform-overview.md` | Trzy produkty: Vibe / Studio / Admin; migracja Le Chat → Vibe (28.05.2026); Chat tab wyłączany; Agenci → Skills (wywołanie `@`); Memories → Knowledge Base; Deep Research → wbudowany skill `deep-research` | `docs/01-product-context-vibe-web.md` |
| `getting-started/quickstarts/studio/create-skill.md` | Tworzenie skilla w Studio: opis jako warunek aktywacji, instrukcje, pliki, `Publish to Vibe`, wersjonowanie | `docs/05-skills-format-and-howto.md` |
| `getting-started/quickstarts/vibe-work/create-first-skill.md` | Pierwszy skill w Vibe Work: formularz (Title / Description / SKILL.md), przykład „Meeting summary” | `docs/05`, `skills/meeting-summary/SKILL.md` |

## Vibe Work (wersja webowa)

| Strona | Zawartość | Gdzie w tym repo |
|---|---|---|
| `vibe/work/get-started.md` | Start zadania w Work; źródła kontekstu (Connectors, Libraries, Skills, Files/Canvas, web search); todos; zasady bezpieczeństwa; tabela „co użyć kiedy” | `docs/01`, `docs/10-vibe-web-usage-playbook.md` |
| `vibe/work/skills.md` | Jak działają Skills; progressive disclosure; lista 12 wbudowanych skills; tworzenie z edytora i z czatu; udostępnianie | `docs/01`, `docs/05` |
| `vibe/work/custom-instructions.md` | Trwałe preferencje użytkownika; priorytet Skill > custom instructions; przykłady; czego unikać | `docs/01`, `docs/10` |
| `vibe/work/projects.md` | Projekty: ton, instrukcje, pliki, czaty w projekcie | `docs/01`, `docs/10` |
| `vibe/work/workflows.md` | Workflows (zbudowane w Studio, wywoływane z `+`); różnica Skill vs Workflow | `docs/01` |

## Promptowanie i próbkowanie (API, ale wzorce uniwersalne)

| Strona | Zawartość | Gdzie w tym repo |
|---|---|---|
| `inference/prompting.md` | Oficjalny przewodnik promptowania: system vs user, rola, struktura, formatowanie, few-shot, strukturyzowane wyjście, lista „czego unikać”, 4 pełne przykłady | `docs/02-prompting-techniques.md` |
| `inference/sampling.md` | Parametry próbkowania: N, temperature, top_p, presence/frequency penalty | `docs/02` (tabela temperatur i tryb fast/think) |

## Legacy (wycofywane, zachowane dla kontekstu)

| Strona | Zawartość | Gdzie w tym repo |
|---|---|---|
| `le-chat/knowledge-integrations/agents.md` | Legacy Agenci w Chat: tworzenie tylko na web/desktop; pola: nazwa, opis, instrukcje, temperatura ~0.15–0.2, narzędzia, wiedza | `docs/01` (mapowanie pól legacy agent → Skill), `docs/03` |

## Inne źródła oficjalne

| Źródło | Zawartość | Gdzie w tym repo |
|---|---|---|
| `docs.mistral.ai/llms.txt` | Pełny indeks dokumentacji w formacie LLM-friendly | Weryfikacja kompletności przeglądu → `docs/00-sources-and-scope.md` |
| `context7.com/mistralai/cookbook/llms.txt` (100k tokenów) | Indeksowana pełna treść cookbooka | Weryfikacja kompletności przeglądu cookbooka → `docs/00`, `references/cookbook-map.md` |
| `help.mistral.ai` | Oficjalny przewodnik użytkownika Vibe/Le Chat | `docs/09-free-external-resources.md` |
| `agentskills.io/specification` | Pełna specyfikacja formatu `SKILL.md` (frontmatter, katalogi, progressive disclosure, walidacja `skills-ref`) | `docs/05-skills-format-and-howto.md` |
| `agentskills.io/skill-creation/best-practices` | Best practices: buduj z realnych zadań, „spending context wisely”, gotchas, szablony, checklisty, pętle walidacji, plan-validate-execute | `docs/05`, `workflows/plan-validate-execute.md` |

## Jak korzystać z tego indeksu

- Szukasz oficjalnego uzasadnienia konkretnej techniki → kolumna 1 (adres na `docs.mistral.ai`, do wersji `.md` dopisz `.md` do ścieżki).
- Chcesz wiedzieć, czy dana strona została uwzględniona w tym repo → kolumna 3.
- Strona oznaczona jako „Legacy” opisuje funkcjonalność wycofywaną — w Vibe używaj zamienników z `docs/01` (Agenci → Skills, Memories → Knowledge Base).
