# AGENTS.md — zasady pracy w tym repo

Ten plik jest przeznaczony dla każdego agenta AI (Mistral Vibe, inny agent, lub człowieka-asystenta), który wchodzi do tego repozytorium i ma z niego korzystać lub go rozwijać.

## Czym jest to repo

Baza wiedzy o używaniu **Mistral w wersji webowej (Vibe, dawniej Le Chat)** oparta na:
- oficjalnym `mistralai/cookbook` (wszystkie sekcje przejrzane),
- `docs.mistral.ai` (pełny llms.txt + strony produktowe Vibe),
- standardu `agentskills.io` (format `SKILL.md`),
- darmowych zasobów społeczności (lista w `docs/09-free-external-resources.md`).

Zakres celowy: **tylko czatowa wersja webowa** (`chat.mistral.ai`, tryb Work). Brak tu kodu API/SDK — celowo, patrz `docs/00-sources-and-scope.md`.

## Kolejność czytania przy pierwszym wejściu

1. Ten plik (`AGENTS.md`).
2. `README.md` — struktura i nawigacja.
3. `docs/00-sources-and-scope.md` — źródła i zakres.
4. `docs/01-product-context-vibe-web.md` — aktualny stan produktu Vibe web.
5. Zadanie konkretne → dobierz dokument z `docs/` i assety z `skills/`, `prompts/`, `templates/`, `workflows/`.

## Konwencje językowe

- **Dokumentacja organizacyjna** (`README.md`, `AGENTS.md`, `docs/**`, `references/**`): język **polski**.
- **Prompty, Skills, szablony** (`skills/**`, `prompts/**`, `templates/**`, `workflows/**`): język **angielski** — modele wykonują instrukcje po angielsku znacznie wierniej. Nie tłumacz promptów "dosłownie"; adaptuj.
- W dokumentach polskich, gdy podajesz gotowy prompt — zawsze cytuj go w oryginale (angielskim) w bloku kodu.

## Konwencje stylu (obowiązują przy pisaniu wszystkich plików)

Wzorowane na oficjalnym Mistral Writing Style Guide (pełna wersja: `docs/06-mistral-writing-style.md`):

- Mów wprost, konkretnie, bez marketingowych ozdobników. Zaczynaj zdania od czasownika.
- Nagłówki: sentence case, bez kropki na końcu, bez `&` i `+`, równoległa struktura.
- Listy: Oxford comma (przecinek przed "and"), jedna spacja po kropce, bez spacji wokół myślnika.
- Każdy nagłówek ma pod sobą przynajmniej jedno zdanie wprowadzające, zanim pojawi się kod/tabela/lista.
- Każdy blok kodu ma tag języka (```markdown`, ```text, ```yaml`).
- Liczby: 0–9 słownie, od 10 cyframi; procenty jako `95%`; daty jako `June 1`.
- Terminologia: `model` (nie "the AI" jako rzeczownik), `agent`, `tool`, `Connector(s)` wielką literą dla produktu, `Vibe` (nie "Le Chat"), `Studio`, `Workflows`, `RAG` (pierwsze użycie: rozwiń jako *retrieval-augmented generation*).
- Nie antropomorfizuj modeli: model **zwraca / wykrywa / generuje**, nie "wie / myśli / halucynuje". Zamiast "hallucynuje" pisz "generuje niedokładną odpowiedź".
- Zakazane słowa: *utilize, leverage, seamless, robust, powerful, easy, simple, just, please, note that, in order to, there is/are, you can* (gdy niepotrzebne), *master/slave, whitelist/blacklist, kill switch*.
- Język inkluzywny: people-first ("person with a disability"), neutralność płciowa, różnorodne imiona w przykładach (np. Amara, Kai, Sofia, Tariq), neutralne placeholdery (`user@example.com`, `your-api-key`).
- Nigdy nie umieszczaj prawdziwych sekretów, kluczy API, tokenów w żadnym pliku. Używaj placeholderów.

## Konwencje techniczne — Skills

- Każdy skill to katalog `skills/<nazwa-skill>/SKILL.md` zgodny ze standardem agentskills.io (szczegóły: `docs/05-skills-format-and-howto.md`).
- Frontmatter: wymagane `name` (1–64 znaki, małe litery + myślniki, zgodne z nazwą katalogu) i `description` (1–1024 znaki, pisane jako warunek aktywacji: "Use when...").
- Ciało `SKILL.md`: poniżej 500 linii / 5000 tokenów; dłuższy materiał do `references/` wewnątrz katalogu skilla.
- Opcjonalne podkatalogi w skillu: `scripts/`, `references/`, `assets/`.
- Jeśli skill odnosi się do plików, używaj ścieżek względnych, maks. jeden poziom w głąb.

## Zasady pracy agenta przy zadaniach z tego repo

1. **Najpierw klasyfikuj zadanie** (wzorzec routera w `docs/03-agent-system-prompt-patterns.md`): czy to pojedyncza odpowiedź, analiza dokumentu, wieloetapowy workflow, pisanie/review dokumentu, czy budowa skilla?
2. **Dobierz assety**: gotowy skill/prompt/szablon/wzorzec z odpowiedniego katalogu zamiast pisać od zera.
3. **Przy pracy wieloetapowej** stosuj wzorce z `workflows/` (łańcuch szeregowy, krytyka→poprawa, orkiestrator→workerzy→synteza).
4. **Przy generowaniu dłuższych dokumentów** wymuszaj format przez szablon z `templates/`, nigdy tylko przez opis słowny.
5. **Po wygenerowaniu** zawsze zaproponuj przebieg weryfikacyjny (wzorzec sędziego: `docs/08-evaluation-and-quality.md`).
6. **Przy edycji istniejących plików** zachowaj ich strukturę i język; nie przepisuj niepotrzebnie.
7. **Nie wymyślaj faktów** o produkcie Mistral — jeśli czegoś nie ma w `docs/`, oznacz to jako "do weryfikacji" zamiast zgadywać.

## Aktualizacja wiedzy

- Repo jest migawką stanu na dzień utworzenia (patrz data w `docs/00-sources-and-scope.md`).
- Produkty Mistral szybko się zmieniają (rebrand Le Chat → Vibe, wyłączanie zakładki Chat, nowe modele). Przy każdym użyciu sprawdzaj aktualność kluczowych założeń w `docs/01-product-context-vibe-web.md`.
- Przy dopisywaniu nowych skills/promptów trzymaj się struktur istniejących katalogów.

## Czego nie rób

- Nie dodawaj kodu wymagającego klucza API ani sekretów.
- Nie rozszerzaj zakresu o API/SDK/self-hosting — to celowo poza zakresem (patrz `docs/00-sources-and-scope.md`).
- Nie wprowadzaj marketingowego języka ani superlatywów ("najlepszy", "rewolucyjny").
- Nie zostawiaj pustych sekcji ani nagłówków bez treści.
