# Kontekst produktu: Mistral Vibe w wersji webowej (stan: październik 2026)

Źródła: `docs.mistral.ai/getting-started/platform-overview.md`, `docs.mistral.ai/vibe/work/*`, `docs.mistral.ai/le-chat/knowledge-integrations/agents.md`, materiały prasowe/community.

## Trzy produkty Mistral (żeby się nie pogubić)

| Produkt | Do czego służy | Czy to "nasz" temat |
|---|---|---|
| **Vibe** | Jednolity agent do pracy i kodu: czat web/mobile (tryb **Work**) + CLI/edytor (tryb **Code**) | **TAK — tylko tryb Work (web)** |
| **Studio** | Konsola deweloperska: klucze API, Playground, agenci API, Workflows, ewaluacje | Nie (ale stąd publikuje się Skills do Vibe i Workflows) |
| **Admin** | Zarządzanie organizacją, billing, SSO, workspace'y | Nie |

## Rebrand: Le Chat → Vibe

- **28 maja 2026**: Le Chat przemianowano na **Vibe**; połączono dotychczasowego czatowego asystenta i agenta kodowego (Vibe CLI) w jeden produkt i jedną subskrypcję.
- Adres `chat.mistral.ai` pozostaje. Konto, plan, historia i ustawienia przeszły automatycznie.
- **Zakładka Chat jest stopniowo wyłączana** (najpierw plany samoobsługowe; Enterprise może mieć ją w oknie migracji 6 miesięcy). Jedna powierzchnia: **Work**.
- Mapowanie funkcji legacy → nowe:
  - **Agenci (Chat)** → **Skills** (wywołanie przez `@nazwa` lub `/nazwa`)
  - **Memories** → **Knowledge Base**
  - **Deep Research** → wbudowany skill **`deep-research`**
  - **Code Interpreter** → natywny sandbox w Work (plany płatne)

## Vibe Work — jak pracuje (tryb webowy)

- Włączasz na `chat.mistral.ai`, przełącznik trybu **Work**.
- Opisujesz **wynik**, jaki chcesz uzyskać, językiem naturalnym. Work sam: zbiera kontekst, rozbija zadanie na kroki, woła narzędzia, **pyta o zgodę przed wrażliwymi akcjami** (wysyłka maila, kasowanie, zmiany w zewnętrznych narzędziach).
- Przełącznik **fast/think**: szybka odpowiedź konwersacyjna vs głęboka, wieloetapowa praca agentowa (z widocznym panelem todos po prawej).
- Dobry pierwszy prompt zawiera: wynik, źródła/narzędzia do użycia, grupę docelową, ograniczenia (długość, ton, format, termin).

### Źródła kontekstu dostępne w Work

| Możliwość | Co daje |
|---|---|
| **Connectors** | Podłączone narzędzia: e-mail, kalendarz, Slack, Notion, GitHub, Google Drive, SharePoint... |
| **Libraries** | Zbiory dokumentów już wgrane i zaindeksowane |
| **Skills** | Powtarzalne metody, checklisty, szablony (patrz `docs/05`) |
| **Files & Canvas** | Wgrywasz dokumenty, arkusze, PDF, obrazy; wyniki przeglądasz w Canvas |
| **Web search / Open URL** | Publiczne informacje i czytanie konkretnych stron |

### Kontrola nad pracą Work

- Panel **todos** po prawej — widać, co Work robi.
- **Akceptuj/odrzucaj** wrażliwe akcje, gdy poprosi.
- Przycisk **stop** (czarный kwadrat) — zatrzymuje, gdy idzie w złą stronę.
- Możesz przekierować kolejną wiadomością, gdy wybierze złe źródło.
- **Wynik traktuj jako szkic** — przeczytaj, zweryfikuj fakty/tabele/daty, poproś o korektę, dopiero potem użyj.

## Mechanizmy personalizacji (trzy poziomy)

To kluczowa hierarchia — zapamiętaj:

| Poziom | Gdzie ustawiasz | Co robi | Priorytet |
|---|---|---|---|
| **Custom instructions** | `Context > Instructions` | Trwałe preferencje na WSZYSTKIE zadania: rola, domena, język, format, ton | najniższy |
| **Projects** | `New Project` w sidebarze | Grupa czatów + własny ton, instrukcje i wspólne pliki dla jednego obszaru (klient, produkt, inicjatywa) | średni |
| **Skills** | `Context > Skills` | Procedura dla konkretnego typu zadania; aktywuje się automatycznie, gdy opis pasuje; wywołanie: `/{nazwa}`, `@nazwa` lub wzmianka w promptcie | **najwyższy** (nadpisuje custom instructions, gdy skill jest aktywny) |

Uwagi praktyczne:
- Custom instructions: krótkie, konkretne, bez sprzeczności z własnymi promptami, **bez sekretów**. Nie używaj ich jako "briefu projektowego" — od tego są Projects/prompt.
- Project: czat należy tylko do jednego projektu; usunięcie projektu nie kasuje czatów.
- Skills: admin workspace może wymusić włączenie (force-enable) dla wszystkich; edycja skilla działa od nowych czatów.

## Wbudowane Skills (działają od razu)

Można wyłączyć w `Context > Skills`. Wywołanie jawne: `/nazwa-skilla`.

| Skill | Aktywuje się przy |
|---|---|
| `challenge-my-thinking` | "sprawdź mój plan", adwokat diabła, przegląd ryzyk |
| `data-analysis` | inspekcja, czyszczenie, transformacja, agregacja danych (`.csv`, `.json`, tabele, metryki, anomalie) |
| `deep-research` | dogłębny research, synteza źródłowa, analiza rynku/technologii |
| `doc-coauthoring` | współtworzenie propozycji, specyfikacji, decyzji, RFC |
| `document-review` | przegląd dokumentów pod kompletność, zgodność, spójność, jakość |
| `internal-comms` | raporty statusowe, aktualizacje dla kierownictwa, FAQ, raporty incydentów |
| `meeting-prep` | przygotowanie do spotkań, brief z kalendarza |
| `research-synthesis` | synteza wielu źródeł/notatek/plików w strukturalny brief |
| `skill-creator` | tworzenie/aktualizacja/usuwanie Skills |
| `stakeholder-translator` | przepisanie treści dla innej grupy odbiorców (kierownictwo, inżynierowie) |
| `structured-extraction` | ekstrakcja danych strukturalnych (tabele, JSON) z maili, PDF, tekstu |
| `vibe-work-onboarding` | przewodnik po możliwościach Vibe Work dla nowych użytkowników |

Przykład użycia wbudowanego skilla: `/deep-research Jakie są główne podejścia do long-context LLM w 2026?` — Work planuje wyszukiwanie, wykonuje wiele zapytań, syntetyzuje źródła i zwraca strukturalny brief z cytowaniami w Canvas.

## Workflows (trzecia kategoria rozszerzeń)

- **Workflow** = zakodowany proces zbudowany przez deweloperów w **Studio**, opublikowany jako asystent czatowy.
- W Work wywołujesz go przez menu **`+` → Workflows`** (jawne wybranie — Work nie wybiera workflow automatycznie na podstawie promptu, w przeciwieństwie do Skills).
- Różnica w jednym zdaniu: **Skill mówi Work, JAK ma się zachować; Workflow to COŚ, co Work wywołuje** (deterministyczna logika, integracje, pipeline danych).

## Dawni Agenci z zakładki Chat (legacy) — co warto wiedzieć

Agenci istnieli jako osobna funkcja czatu; migrują w stronę Skills. Jeśli masz jeszcze dostęp do zakładki Chat (Enterprise), agent miał te pola — to dobra mapa, jak pisać dzisiejsze Skills:

| Pole legacy Agenta | Odpowiednik w Skillu |
|---|---|
| Nazwa | `name` w frontmatter + Title |
| "Purpose of this Agent" (krótki opis) | `description` w frontmatter (warunek aktywacji "Use when...") |
| Instructions (system prompt) | ciało `SKILL.md` |
| Temperatura (~0.15–0.2 zalecane) | brak odpowiednika w Vibe web — kontrolujesz przez tryb fast/think i precyzję instrukcji |
| Narzędzia / wiedza (Connectors, biblioteki) | podłączone do Work globalnie; skill może polecać ich użycie w instrukcjach |
| Tworzenie tylko na web/desktop | Skills: też przez `Context > Skills` (web) lub Studio |

Praktyczna rada z community: wybór modelu w legacy agentach miał znaczenie (nie każdy model wspierał narzędzia/multimodalność) — w Vibe Work model wybiera się na poziomie konta/planu i przełącznikiem fast/think, więc skup się na jakości instrukcji.

## Limity i uwagi

- Część funkcji (Code Interpreter/sandbox, część Workflows) jest **zależna od planu** — błędy typu "your account tier does not have access" są normalne na darmowym planie.
- Work może poprosić o podłączenie/uwierzytelnienie brakującego narzędzia w trakcie zadania.
- Ustawienia organizacji mogą ograniczać dostępne narzędzia i skills.
- Darmowy plan istnieje ("Free for quick answers and simple everyday tasks"); Pro/Team/Enterprise odblokowują głębszą pracę agentową.
