# Skills — format, specyfikacja i kompletny przewodnik tworzenia

Źródła: `agentskills.io/specification`, `agentskills.io/skill-creation/best-practices`, `docs.mistral.ai/vibe/work/skills.md`, `docs.mistral.ai/getting-started/quickstarts/studio/create-skill.md`, `docs.mistral.ai/getting-started/quickstarts/vibe-work/create-first-skill.md`, `mistralai/cookbook` (`skills/cookbook-review`, `skills/check-stale-cookbooks`).

## Czym jest Skill

Skill to **pakiet wiedzy proceduralnej**: metoda, checklist, szablon, format wyjścia lub procedura domenowa, którą pakujesz raz, a Work (Vibe) stosuje ją za każdym razem, gdy zadanie pasuje. Skill różni się od zapisanego promptu: prompt = „taki tekst”, skill = „**kiedy** tego użyć + **jak** to zrobić + opcjonalne pliki”.

Standard: otwarty **Agent Skills** (`agentskills.io`), pierwotnie od Anthropic, przyjęty przez Mistral Vibe i wiele innych narzędzi. Skill napisany raz działa wszędzie.

## Jak działa Skill (progressive disclosure — trzy etapy)

1. **Discovery (odkrywanie):** przy starciu sesji Work ładuje tylko `name` + `description` każdego skilla (~100 tokenów na skill) — tyle, żeby wiedzieć, kiedy może być przydatny.
2. **Activation (aktywacja):** gdy zadanie pasuje do opisu, Work ładuje pełną treść `SKILL.md` do kontekstu.
3. **Execution (wykonanie):** Work wykonuje instrukcje, opcjonalnie ładując wskazane pliki (references/assets) na żądanie.

Konsekwencja: możesz mieć włączonych wiele skills bez przeciążania kontekstu — tylko pasujące rozwiną się w pełne instrukcje.

## Struktura katalogu

```text
nazwa-skillu/                  # nazwa katalogu = wartość pola `name`
├── SKILL.md                    # WYMAGANE: metadane + instrukcje
├── scripts/                    # opcjonalnie: wykonywalny kod (self-contained)
├── references/                 # opcjonalnie: dokumentacja ładowana na żądanie
│   ├── REFERENCE.md
│   └── domain-specific.md      # np. finance.md, legal.md
└── assets/                     # opcjonalnie: szablony, obrazy, dane
    └── template.md
```

## Format SKILL.md

Plik musi zaczynać się od frontmatter YAML (od bajtu 0), potem treść Markdown.

### Frontmatter

| Pole | Wymagane | Ograniczenia |
|---|---|---|
| `name` | TAK | 1–64 znaki; tylko małe litery `a-z`, cyfry `0-9` i myślniki `-`; nie może zaczynać się ani kończyć myślnikiem; bez podwójnych myślników; **musi być zgodna z nazwą katalogu** |
| `description` | TAK | 1–1024 znaki; opisuje **co robi skill i kiedy go użyć**; dodaj konkretne słowa-klucze, które pomogą agentowi rozpoznać pasujące zadanie |
| `license` | nie | nazwa licencji lub odniesienie do pliku licencji |
| `compatibility` | nie | max 500 znaków; wymagania środowiskowe (produkt, paczki, sieć) |
| `metadata` | nie | dowolna mapa klucz→wartość (stringi), np. autor, wersja |
| `allowed-tools` | nie | eksperymentalne; lista pre-zatwierdzonych narzędzi (oddzielone spacjami) |

Minimalny przykład:

```markdown
---
name: skill-name
description: A description of what this skill does and when to use it.
---
```

Przykład rozszerzony:

```markdown
---
name: pdf-processing
description: Extract PDF text, fill forms, merge files. Use when handling PDFs.
license: Apache-2.0
metadata:
  author: example-org
  version: "1.0"
---
```

### Najważniejsze pole: `description` jako warunek aktywacji

To **nie jest opis dla człowieka** — to warunek, po którym agent decyduje, czy aktywować skill. Pisz jako trigger:

- Źle: „Helps with contracts.” / „This Skill helps with..."
- Dobrze: „Use when reviewing customer contracts to identify renewal dates, termination clauses, liability caps, and missing approvals."
- Jeszcze lepiej (ze słowami-kluczami): „Creates professional README.md files. Use when the user asks to \"write a README\", \"create a readme\", \"document this project\", or \"generate project documentation\"."

Zasada: opis = CO + KIEDY, z konkretnymi słowami, których użyje użytkownik.

### Ciało (Markdown po frontmatter)

Brak ograniczeń formatu — pisz to, co pomaga agentowi wykonać zadanie. Zalecane sekcje:

- instrukcje krok po kroku,
- przykłady wejść i wyjść,
- edge case'y (patrz sekcja „Gotchas” niżej).

**Budżet kontekstu:** całe `SKILL.md` ładuje się do kontekstu po aktywacji. Trzymaj je **poniżej 500 linii / 5000 tokenów**. Dłuższy materiał przenieś do `references/` i każ agentowi, **kiedy** ma dany plik wczytać (np. „Read `references/api-errors.md` if the API returns a non-200 status code”).

### Odwołania do plików

Ścieżki względne od katalogu skilla, maks. jeden poziom w głąb:

```markdown
See [the reference guide](references/REFERENCE.md) for details.
Run the extraction script:
scripts/extract.py
```

### Walidacja

```bash
skills-ref validate ./nazwa-skillu
```

Sprawdza poprawność frontmatter i konwencje nazewnictwa.

## Jak tworzyć Skill w Vibe Work (3 ścieżki)

### Ścieżka 1: Z czatu (ZALECANA)

1. Wykonaj zadanie, w którym wypracowujesz podejście warte powtórzenia (styl pisania, checklistę przeglądu, metodę researchu).
2. Poproś Work: **„turn this into a Skill”** / „save this as a Skill”.
3. Work wygeneruje szkic `SKILL.md`, pokaże zawartość i zaproponuje do przeglądu.
4. Przejrzyj, edytuj, zapisz.

Zaleta: skill zbudowany z realnego użycia zawiera wzorce, które faktycznie zadziałały — łącznie z detalami, o których nie pomyślałbyś pisząc od zera.

### Ścieżka 2: Z edytora

1. `Context > Skills` w sidebarze → **New Skill**.
2. Wypełnij formularz:
   - **Title** — krótka, opisowa nazwa (np. „Brand voice copywriting”).
   - **Description** — kiedy Work ma użyć tego skilla (pisz jako „Use when...”).
   - **SKILL.md** — pełne instrukcje: ton, format, kroki, przykłady, „czego unikać”.
3. Opcjonalnie dodaj pliki (szablony, brand guidelines, przykładowe wyjścia).
4. **Create Skill** → przetestuj na prawdziwym zadaniu → iteruj.

### Ścieżka 3: Ze Studio (dla zespołów)

1. W Studio: `Build > Skills` → New Skill (te same pola + wersjonowanie: każda zmiana = nowa wersja, starsze zostają do rollbacku).
2. Aby skill był dostępny w Vibe Work: na stronie skilla kliknij **Publish to Vibe**.
3. Udostępnianie: `Share` — private (tylko Ty) lub cały workspace. Admin może force-enable dla wszystkich.

## Używanie skilla w zadaniu

- **Automatycznie:** Work sam aktywuje pasujący skill (widać go w todos panelu).
- **Jawnie:** wpisz `/{nazwa-skilla}` na początku promptu.
- **Wzmianką:** „Use the contract-review Skill on this PDF”.

## Zarządzanie

Z `Context > Skills`: edytuj title/description/SKILL.md/pliki; wyłącz bez kasowania; usuń (workspace skills wymagają uprawnień); udostępnij. **Edycja działa od nowych czatów** — aktywne czaty trzymają starą wersję.

## Best practices (agentskills.io + Mistral) — kompletna lista

### Buduj z realnej ekspertyzy
- Nie proś LLM o skill „z niczego” — wynik będzie ogólnikowy („handle errors appropriately”). Najpierw **wykonaj zadanie ręcznie/w czacie**, zanotuj: co zadziałało, gdzie poprawiałeś agenta, jakie były formaty we/wy, jaki kontekst był potrzebny.
- Możesz też zsyntezować skill z istniejących artefaktów: runbooki, style guide'y, komentarze z code review, historia fixów w repozytorium, realne przypadki błędów.

### Mądrze wydawaj kontekst
- Po aktywacji cały `SKILL.md` konkuruje o uwagę z historią rozmowy i innymi skillami. Każde zdanie musi zarabiać na miejsce.
- **Dodawaj to, czego agentowi brakuje; pomijaj to, co zna.** Nie opisuj, czym jest PDF czy HTTP. Opisuj konwencje projektu, procedury domenowe, nieoczywiste edge case'y, konkretne narzędzia/API.
- Test: „Czy agent zrobiłby to źle bez tej instrukcji?” Jeśli nie — wytnij.

### Jedna robota = jeden skill
- Skill = spójna jednostka pracy (jak funkcja). Zbyt wąski → wiele skills ładuje się naraz; zbyt szeroki → trudno precyzyjnie aktywować. Przykład granicy: „query the database and format results” = OK; to samo + „and administer the database” = za dużo.

### Umiarkowany poziom szczegółowości
- Zbyt szczegółowy skill dezorientuje agenta (próbuje niepasujących ścieżek). Zbyt ogólny = bezużyteczny. Złoty środek: zwięzłe kroki + jeden działający przykład.

### Progressive disclosure dla dużych skills
- `SKILL.md` < 500 linii / 5000 tokenów = tylko to, co potrzebne na każdym przebiegu.
- Reszta do `references/` — i **powiedz agentowi, kiedy ma czytać każdy plik**.

### Kalibracja szczegółowości (specificity vs freedom)
- **Daj swobodę**, gdy wiele podejść jest OK i zadanie to znosi. Wyjaśnij DLACZEGO — agent rozumiejący cel podejmuje lepsze decyzje.
- **Bądź nakazowy**, gdy operacja jest krucha, liczy się spójność lub kolejność jest krytyczna:
  ```markdown
  ## Database migration
  Run exactly this sequence:
  python scripts/migrate.py --verify --backup
  Do not modify the command or add additional flags.
  ```

### Domyślne rozwiązania, nie menu
- Źle: „You can use pypdf, pdfplumber, PyMuPDF, or pdf2image...”
- Dobrze: „Use pdfplumber for text extraction. For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.”

### Procedury, nie deklaracje
- Skill ma uczyć **jak podejść** do klasy problemów, nie odpowiadać na pojedyncze instancje:
  - Źle (konkretna odpowiedź): „Join the `orders` table to `customers` on `customer_id`, filter where `region = 'EMEA'`, and sum the `amount` column.”
  - Dobrze (metoda wielokrotnego użytku):
    ```markdown
    1. Read the schema from `references/schema.yaml` to find relevant tables
    2. Join tables using the `_id` foreign key convention
    3. Apply any filters from the user's request as WHERE clauses
    4. Aggregate numeric columns as needed and format as a markdown table
    ```

### Gotchas (najcenniejsza sekcja)

Lista konkretnych faktów, które zaskakują model — korekty błędów, których agent bez tego nie uniknie:

```markdown
## Gotchas
- The `users` table uses soft deletes. Queries must include `WHERE deleted_at IS NULL`
  or results will include deactivated accounts.
- The user ID is `user_id` in the database, `uid` in the auth service, and `accountId`
  in the billing API. All three refer to the same value.
- The `/health` endpoint returns 200 as long as the web server is running, even if the
  database connection is down. Use `/ready` for full service health.
```

Zasada iteracji: **każda Twoja korekta agenta → dopisz do sekcji Gotchas.**

### Szablony formatu wyjścia

Gdy wynik ma mieć konkretny format — daj **szablon**, nie opis. Agenty trzymają się szablonów lepiej niż opisów słownych. Krótki szablon inline w `SKILL.md`; dłuższy do `assets/` z odwołaniem.

```markdown
## Report structure
Use this template, adapting sections as needed:

# [Analysis Title]

## Executive summary
[One-paragraph overview of key findings]

## Key findings
- Finding 1 with supporting data
- Finding 2 with supporting data

## Recommendations
1. Specific actionable recommendation
2. Specific actionable recommendation
```

### Checklisty dla wieloetapowych przepływów

```markdown
## Form processing workflow
Progress:
- [ ] Step 1: Analyze the form (run `scripts/analyze_form.py`)
- [ ] Step 2: Create field mapping (edit `fields.json`)
- [ ] Step 3: Validate mapping (run `scripts/validate_fields.py`)
- [ ] Step 4: Fill the form (run `scripts/fill_form.py`)
- [ ] Step 5: Verify output (run `scripts/verify_output.py`)
```

### Pętle walidacji (validation loops)

„Zrób → sprawdź → popraw → dopiero kontynuuj”:

```markdown
## Editing workflow
1. Make your edits
2. Run validation: `python scripts/validate.py output/`
3. If validation fails:
   - Review the error message
   - Fix the issues
   - Run validation again
4. Only proceed when validation passes
```

W czacie (bez skryptów) odpowiednikiem walidacji jest **krytyk/sędzia** (patrz `docs/08`) lub własna checklista.

### Iteracja z użyciem trace'ów

Po uruchomieniu skilla na realnym zadaniu przejrzyj, **co agent faktycznie zrobił** (kolejność kroków), nie tylko wynik końcowy. Typowe przyczyny marnowania czasu: zbyt ogólne instrukcje (agent próbuje wielu podejść), instrukcje niepasujące do zadania (agent mimo to je wykonuje), zbyt wiele opcji bez domyślnego wyboru.

## Antywzorce (czego unikać w skillach)

- Opis „Helps with X” zamiast warunku „Use when..." — skill nigdy się nie aktywuje.
- Łączenie wielu niepowiązanych procedur w jeden skill.
- Niemal-duplikaty (dwa skille o podobnych opisach) — dezorientują Work i rozdymają kontekst. Edytuj istniejący zamiast zapisywać wariant.
- Sekrety/klucze w treści skilla — skille są trwałe i współdzielone.
- Zakazy bez uzasadnienia („Do not...") tam, gdzie wystarczy domyślne rozwiązanie.
- Instrukcje sprzeczne wewnątrz jednego skilla — używaj drzewek decyzyjnych.

## Szablon startowy SKILL.md (kopiuj i dostosuj)

```markdown
---
name: nazwa-skillu
description: Use when <konkretny przypadek użycia, ze słowami-kluczami użytkownika>.
  Produces <konkretny wynik w konkretnym formacie>.
license: Apache-2.0
metadata:
  author: twoj-nick
  version: "1.0"
---

# Nazwa skilla

## Kiedy stosować
Jedno zdanie: ten skill aktywuje się, gdy użytkownik <...>.

## Procedura
1. Krok pierwszy...
2. Krok drugi...

## Format wyjścia
```text
SZABLON WYJŚCIA
```

## Gotchas
- Fakt, który model zgubi bez tej instrukcji...

## Czego unikać
- Nie wymyślaj danych spoza źródła. Nieznane wartości oznacz jako "Not found".
```

Gotowe, wypełnione przykłady: katalog `skills/` w tym repo.
