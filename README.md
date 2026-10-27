# mistral-data-analysis-

Kompletna baza wiedzy o wykorzystaniu **Mistral w wersji webowej (Vibe / dawniej Le Chat)** — ekstrakcja i synteza z oficjalnego repozytorium `mistralai/cookbook`, dokumentacji `docs.mistral.ai`, standardu `agentskills.io` oraz darmowych zasobów społeczności.

Zakres: **wyłącznie to, co działa w czatowej wersji webowej** (`chat.mistral.ai`, tryb Work). Bez API, bez SDK, bez self-hostingu — ale z pełnymi wzorcami promptów, które te narzędzia używają wewnętrznie.

## Jak korzystać z tego repo (dla człowieka)

1. Otwórz [docs/10-vibe-web-usage-playbook.md](docs/10-vibe-web-usage-playbook.md) — tam jest przepis użycia dzień po dniu.
2. Jeśli chcesz zrobić własnego **agenta/skill** w Vibe: skopiuj gotowy plik z [skills/](skills/) lub zbuduj własny według [docs/05-skills-format-and-howto.md](docs/05-skills-format-and-howto.md).
3. Jeśli potrzebujesz gotowego promptu do wklejenia: [prompts/](prompts/).
4. Pełne uzasadnienie każdej techniki (z odnośnikami do źródeł): [docs/](docs/).

## Jak korzystać z tego repo (dla agenta)

Przeczytaj w tej kolejności:

1. [AGENTS.md](AGENTS.md) — zasady pracy w tym repo.
2. [docs/00-sources-and-scope.md](docs/00-sources-and-scope.md) — co skąd pochodzi i czego tu celowo nie ma.
3. [docs/01-product-context-vibe-web.md](docs/01-product-context-vibe-web.md) — jak działa Vibe web (stan aktualny).
4. [docs/05-skills-format-and-howto.md](docs/05-skills-format-and-howto.md) — jak pisać Skills.
5. Reszta `docs/` według potrzeb zadania; gotowe assety w `skills/`, `prompts/`, `templates/`, `workflows/`.

## Struktura repo

| Katalog | Zawartość |
|---|---|
| `AGENTS.md` | Zasady pracy dla agenta AI w tym repo (konwencje, język, styl) |
| `docs/` | Pełna baza wiedzy, 11 dokumentów, po polsku, ze źródłami |
| `skills/` | Gotowe `SKILL.md` (standard agentskills.io) — kopiuj-wklej do Vibe |
| `prompts/system/` | Gotowe system prompty / instrukcje niestandardowe |
| `prompts/tasks/` | Gotowe prompty zadaniowe (klasyfikacja, streszczenie, ekstrakcja, przegląd...) |
| `templates/` | Szablony formatów wyjścia (brief, przegląd, raport, PRD...) |
| `workflows/` | Wzorce wieloetapowe jako przepisy do czatu (łańcuch, krytyka→poprawa, orkiestrator→synteza) |
| `references/` | Indeks źródeł: mapa cookbooka i docs.mistral.ai |

## Skąd ta wiedza

- `github.com/mistralai/cookbook` (pełny przegląd: skills, prompting, agents, moderation, data_generation, evaluation, ocr, image_understanding, rag, connectors)
- `docs.mistral.ai` (llms.txt + strony: prompting, sampling, vibe/work/*, legacy le-chat agents, quickstarty Skills)
- `agentskills.io` (specyfikacja formatu `SKILL.md` + best practices)
- Darmowe zasoby: `alirezarezvani/claude-skills` (natywnie kompatybilne z Mistral Vibe), `VoltAgent/awesome-agent-skills`, `docsbot.ai`, `prompt-architects.com`, `help.mistral.ai`

Pełna lista z opisem: [docs/00-sources-and-scope.md](docs/00-sources-and-scope.md) oraz [docs/09-free-external-resources.md](docs/09-free-external-resources.md).

## Język

- Dokumentacja organizacyjna (`README.md`, `AGENTS.md`, `docs/`): **polski**.
- Prompty, Skills, szablony (`skills/`, `prompts/`, `templates/`, `workflows/`): **angielski** — modele wykonują instrukcje po angielsku znacznie wierniej. Tłumaczenia kluczowych zwrotów znajdziesz w dokumentacji.
