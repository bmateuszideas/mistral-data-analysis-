# Playbook: jak używać Vibe (web) — przepis dzień po dniu

To jest dokument „operacyjny” — konkretne kroki, nie teoria. Odniesienia do teorii w nawiasach.

## Dzień 0: pierwsze 15 minut (konfiguracja pod Twoje potrzeby)

1. Wejdź na [chat.mistral.ai](https://chat.mistral.ai), zaloguj się.
2. Upewnij się, że jesteś w trybie **Work** (przełącznik trybu w sidebarze).
3. **Custom instructions** (`Context > Instructions`): ustaw trwałe preferencje, np.:
   > I'm a <twoja rola> working in <domena>. Structure technical answers with sections: Problem, Analysis, Solution, Next steps. Keep answers concise, use bullet points. Always respond in <język> unless I ask otherwise.
   (źródło: `docs.mistral.ai/vibe/work/custom-instructions` — przykłady w `docs/01`)
4. **Wyłącz wbudowane skills, których nie potrzebujesz** (`Context > Skills`) — każdy opis zajmuje miejsce w kontekście.
5. Przetestuj wbudowany skill: wpisz `/deep-research` + krótkie pytanie badawcze. Zobacz, jak wygląda praca z todos.

## Codzienna praca: szybka odpowiedź vs głęboka analiza

| Potrzebujesz | Zrób |
|---|---|
| Szybka odpowiedź (definicja, krótkie wyjaśnienie) | Przełącznik **fast**, zwykłe pytanie |
| Analiza, research, dokument, wieloetapowa praca | Przełącznik **think** (tryb Work), opisz **wynik**, źródła, ograniczenia (`docs/01`) |
| Kontrola nad przebiegiem | Obserwuj panel **todos**; akceptuj/odrzucaj wrażliwe akcje; **stop** gdy idzie źle; koryguj kolejną wiadomością |
| Zawsze weryfikuj wynik | Przeczytaj, sprawdź fakty/daty/źródła, poproś o korektę — wynik to szkic (`docs/01`, `docs/08` punkt 9) |

## Powtarzalne zadanie → zrób z tego Skill (najważniejszy nawyk)

Gdy trzeci raz robisz to samo (format raportu, przegląd dokumentu, streszczenie spotkania):

1. Wykonaj zadanie w czacie, dopracuj podejście (krytyka→poprawa, `docs/04` wzorzec A3).
2. Napisz: **„turn this into a Skill”**.
3. Work wygeneruje `SKILL.md` — przejrzyj: czy `description` to warunek „Use when...”, czy procedura kompletna, czy format wyjścia ma szablon.
4. Zapisz, przetestuj na innym przykładzie, iteruj.
5. Udostępnij workspace, jeśli ma z tego korzystać zespół.

Standard formatu i zasady: `docs/05`. Gotowe punkty wyjścia: `skills/` w tym repo.

## Projekt = kontekst, który trwa

Gdy pracujesz nad jednym klientem/produktem przez tygodnie:

1. `New Project` → nazwa + ikona.
2. W projekcie: ustaw **ton**, **instrukcje** (formatowanie, słownictwo domenowe), wgraj **pliki** (kontrakty, brand book, dane).
3. Przeciągaj czaty do projektu. Wszystko, co wiesz o kliencie, jest pod ręką w każdym czacie projektu.

(źródło: `docs.mistral.ai/vibe/work/projects`)

## Trudne zadanie → wybierz wzorzec (nie improwizuj)

| Zadanie | Wzorzec | Gdzie |
|---|---|---|
| Transformacja wieloetapowa (A→B→C) | Łańcuch szeregowy | `docs/04` wzorzec A, `workflows/` |
| Analiza „z 5 perspektyw” | Orkiestrator + workerzy + synteza | `docs/04` wzorzec B/G |
| Nie wiesz, którą ścieżkę wybrać | Router | `docs/04` wzorzec C, `prompts/tasks/classify-intent.md` |
| Ryzykowna/wieloobiektowa zmiana | Plan → walidacja → wykonanie | `docs/04` wzorzec D |
| Kod/dokument z błędami pierwszej wersji | Review-and-fix | `docs/04` wzorzec E |
| Dokument (PDF/obraz/CSV) | Ekstrakcja wg schematu + grounding | `docs/07` |
| Research z aktualnych danych | Wbudowany `deep-research` lub web search + cytowania | `docs/07` punkt 7 |
| Ocena jakości odpowiedzi | Sędzia / checklista / adwokat diabła | `docs/08` |
| Pisanie „po mistrzowsku” | Przewodnik stylu | `docs/06` |

## Pierwsza wiadomość w każdym nowym czacie — szablon

```text
[Opcjonalnie: @nazwa-skilla lub /nazwa-skilla]

Rola: You are a <rola>.
Kontekst: <kim jestem, w jakiej dziedzinie pracuję>.
Zadanie: <co dokładnie chcę>.
Dane: <wklejone / wgrane pliki / wzmianka o Library>
Format: <szablon lub „JSON z kluczami...”>
Ograniczenia: <czego nie rób, czego nie wymyślaj, język, długość>
Przykłady: <1-2 przykłady wejście→wyjście, gdy format jest niestandardowy>
```

Anatomia wzorowana na `docs/02` punkt 18.

## Czego unikać (skrót z dokumentacji oficjalnej)

- Niejasnych słów („ciekawe”, „lepsze”, „wiele”) → konkretne miary.
- Sprzecznych instrukcji → drzewka decyzyjne.
- „Policz słowa” → podaj liczby jako dane.
- Prośby o zbędne treści (płać za tokeny = marnuj czas i pieniądze).
- Skali „1–5” → skale opisowe.
- Ufania, że model „pamięta” — kontekst jest skończony; ważne fakty powtarzaj lub trzymaj w plikach/Library.
- Wrzucania sekretów do czatu (klucze, hasła, dane osobowe wrażliwe) — patrz polityka bezpieczeństwa poniżej.

## Bezpieczeństwo i prywatność (praktycznie)

- Work **pyta o zgodę przed wrażliwymi akcjami** (wysyłka maila, kasowanie, zmiany w narzędziach) — czytaj te prośby uważnie.
- Nie wgrywaj dokumentów z danymi osobowymi wrażliwymi, jeśli organizacja tego zabrania — sprawdź politykę.
- Custom instructions i skille są **trwałe** — nie wpisuj tam sekretów.
- Wersja darmowa vs płatna: część funkcji (Code Interpreter/sandbox, niektóre Workflows) wymaga planu płatnego — błąd „tier does not have access” jest normalny.

## Gdy coś nie działa

| Objaw | Przyczyna / rozwiązanie |
|---|---|
| Skill się nie aktywuje | `description` za ogólny — dopisz konkretne słowa-klucze i „Use when...” (`docs/05`) |
| Dwie odpowiedzi różnią się mimo tego samego promptu | Model losuje (temperatura) — poproś o wariant najbardziej konserwatywny lub włącz think; albo zapisz dobrą odpowiedź jako skill |
| Czat „zapomina” środek długiego dokumentu | Za dużo kontekstu — pracuj sekcjami (`docs/07` punkt 5) lub streszczaj pośrednie wyniki |
| Odpowiedź w złym języku | Wymuś język w pierwszej wiadomości lub w custom instructions („Always respond in French unless I ask otherwise”) |
| Work robi coś nie tak w trakcie zadania | Przycisk **stop**, korekta wiadomością, lub zacznij nowy czat z lepszym promptem startowym |
| „Workflow failed: tier does not have access” | Funkcja wymaga wyższego planu — użyj alternatywy ręcznej |
| Model „wymyśla” fakty | Wymuś grounding: „Answer only using the provided documents; if not found, say so” (`docs/07` punkt 2) |

## Skrót: hierarchia personalizacji (zapamiętaj raz)

**Custom instructions** (zawsze, wszystkie czaty) < **Project** (czaty w projekcie + pliki) < **Skill** (konkretne zadanie, najwyższy priorytet, gdy aktywny).

## Gdzie szukać dalej

- Problemy z produktem → [help.mistral.ai](https://help.mistral.ai)
- Aktualna dokumentacja → [docs.mistral.ai](https://docs.mistral.ai) (lub [llms.txt](https://docs.mistral.ai/llms.txt) jako jeden plik)
- Gotowe skille → `docs/09` (biblioteki) + `skills/` (to repo)
- Wzorce promptów → `docs/02`, `prompts/`
- Wzorce wieloetapowe → `docs/04`, `workflows/`
