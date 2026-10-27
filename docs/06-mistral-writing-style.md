# Przewodnik stylu Mistral (Mistral Writing Style Guide) — pełna wersja użytkowa

Źródło: `mistralai/cookbook/skills/cookbook-review/` — kompletne pliki `voice-and-tone.md`, `checklists.md`, `ai-terms.md`, `developer-content.md`, `inclusive-language.md` oraz `SKILL.md` (cookbook-review).

Ten dokument obowiązuje przy pisaniu **wszystkich** instrukcji, skillów, promptów systemowych i dokumentacji w tym repo (patrz `AGENTS.md`). Dlaczego warto go stosować: Mistral rekomenduje ten styl w całym swoim ekosystemie, a modele Mistral są na niego „wyczulone” — instrukcje napisane zgodnie z nim są wykonywane wierniej.

---

## 1. Głos (voice) — trzy zasady

Mistralowa komunikacja opiera się na **„crisp simplicity”**: większe myśli, mniej słów. Mniej „głowy”, więcej „serca”.

- **Ciepły i swobodny (warm and relaxed):** naturalny, jak rozmowa. Mniej oficjalny, bardziej przyziemny. Czasem żartobliwy — Mistral wie, kiedy świętować.
- **Rzeczowy i jasny (crpisp and clear):** konkretnie do celu. Piszemy najpierw dla **skanowania**, potem dla czytania. Prostota ponad wszystko.
- **Pomocny (ready to lend a hand):** pokazujemy, że jesteśmy po stronie czytelnika. Przewidujemy realne potrzeby i podajemy właściwą informację we właściwym momencie.

## 2. 10 najważniejszych zasad pisania

1. **Większe myśli, mniej słów.** Krócej = lepiej.
   - Źle: „If you're ready to use the Mistral API in your production environment, contact our support team for enterprise pricing.”
   - Dobrze: „Ready to go to production? Contact us.”
2. **Pisz jak mówisz.** Przeczytaj na głos. Bez żargonu i przesadnie technicznego języka.
   - Źle: „Invalid API key”
   - Dobrze: „Your API key looks wrong. It should look like this: `sk-...`”
3. **Bądź przyjazny.** Kontrakcje: *it's, you'll, you're, we're, let's*.
   - Źle: „To enable the model to access external data, the connector must be configured with the appropriate credentials.”
   - Dobrze: „To let the model access external data, configure the connector with the right credentials.”
4. **Szybko do sedna.** Najważniejsze na początku. Słowa-klucze na pierwszy plan. Decyzje i następne kroki mają być oczywiste.
   - Źle: „Connectors provide a mechanism for the model to call external tools registered as MCP servers. Before you can use a connector in a conversation, you must first create it using the connectors API.”
   - Dobrze: „Create a connector first. Connectors let the model call external tools via MCP—you can't use one in a conversation until it exists.”
5. **Bądź zwięzły.** Tyle informacji, ile trzeba, żeby podjąć decyzję. Wycinaj każde zbędne słowo.
   - Źle: „The `chat.complete()` method can be used to send messages to the model and retrieve a response from it.”
   - Dobrze: „Use `chat.complete()` to send messages to the model.”
6. **Gdy wątpisz — nie kapitalizuj.** Sentence case: tylko pierwsze słowo i nazwy własne.
   - Źle: `## Creating A Connector With OAuth Authentication`
   - Dobrze: `## Creating a connector with OAuth authentication`
7. **Kropki i dwukropki w nagłówkach — nie.** Bez kropek i dwukropków na końcu tytułów i nagłówków.
   - Źle: `**Initialize the client.**` przed krokami
   - Dobrze: `**Initialize the client**` przed krokami
8. **Pamiętaj o ostatnim przecinku** (Oxford comma).
   - Źle: „Python, TypeScript and curl”
   - Dobrze: „Python, TypeScript, and curl"
9. **Nie rób luzu.** Jedna spacja po kropkach, pytajnikach, dwukropkach. Bez spacji wokół myślnika.
   - Źle: „The model calls tools — like connectors and built-in tools — based on context.”
   - Dobrze: „The model calls tools—like connectors and built-in tools—based on context.”
10. **Wycinaj słaby język.** Zazwyczaj zaczynaj od czasownika. Wycinaj *you can*, gdy niepotrzebne. Unikaj *there is/are/were*.
    - Źle: „You can also specify a timeout, and there are additional options available for controlling retries.”
    - Dobrze: „Specify a timeout and configure retry options.”

## 3. Zwracanie się do czytelnika

- Mów **„you”** (po polsku: „Ty”/„Państwo” zależnie od kontekstu, ale konsekwentnie). Nie „the user”, nie „one”.
- Tryb oznajmujący dla faktów, **tryb rozkazujący dla instrukcji** (imperative: „Run”, „Install”, „Set”, „Copy”).
- Czasownik w czasie teraźniejszym. Unikaj „will”, „was”.
- Strona czynna. „The model calls the tool”, nie „The tool is called by the model”.

## 4. Nagłówki

- **Sentence case** wszędzie: `## Step 2 — Set up the environment`.
- Bez kropki na końcu.
- Bez `&` i `+` (chyba że opisujesz UI, które je zawiera).
- Krótko i konkretnie — nagłówek mówi, co dokładnie znajdziesz w sekcji.
- Struktura równoległa na tym samym poziomie: `## Create a connector`, `## List connectors`, `## Delete a connector`.
- **Nigdy dwóch nagłówków z rzędu** bez tekstu między nimi (chyba że drugi to podsekcja pierwszego).
- **Nigdy nie zaczynaj sekcji od bloku kodu/tabeli/listy** — najpierw jedno zdanie wprowadzające.

## 5. Listy

- Lista wypunktowana = rzeczy nieuporządkowane; lista numerowana = kroki sekwencyjne.
- Elementy listy mają być **równoległe** gramatycznie.
- Oxford comma w seriach 3+.
- Bez kropki na końcu pojedynczych, jedno-zdaniowych punktów (chyba że to pełne zdania wielozdaniowe).

## 6. Punktacja

- Jedna spacja po `.` `?` `!` `:`.
- Dwukropek na końcu frazy wprowadzającej listę.
- Apostrofy proste (nie „krzaczaste”): `don't`, `it's`. Brak apostrofu w `its` (to już zaimek dzierżawczy).
- Po dwukropku w środku zdania — mała litera (chyba że nazwa własna).
- Bez spacji wokół myślnika (—).
- Nie używaj ukośnika `/` w znaczeniu „lub” w prozie.
- Bez kropek na końcu nagłówków.
- Wykrzykniki oszczędnie. Pytajniki w dokumentacji oszczędnie.

## 7. Liczby

**Słownie:** 0–9 (chyba że miejsca mało); jedna liczba, gdy obok druga z innej kategorii („two 3-page articles”); na początku zdania; liczebniki porządkowe słownie („first”, „third”; nie „firstly”).

**Cyframi:** od 10; miary (odległość, temperatura, rozmiar, piksele); procenty symbolem `95%`; wymiary `1920 × 1080` (× z odstępami); liczby w kodzie/UI; godziny.

**Inne:** przecinek w liczbach 4+ cyfr (`1,024`); daty bez porządków („June 1”, nie „June first”); miesiąc zawsze słownie; ułamki dziesiętne z zerem (`0.95`); ujemne z minusem.

## 8. Akronimy

- Przy pierwszym użyciu rozwiń i podaj akronim w nawiasie; potem sam akronim.
- Nie rozwijaj szeroko znanych (API, SDK, JSON, HTTP).
- Nie wprowadzaj akronimu, którego użyjesz raz.
- Nie zaczynaj nagłówka od akronimu (chyba że to słowo-klucz do wyszukiwania).
- „a/an” wg wymowy: *an MCP server*, *a SDK*, *an API key*.
- Liczba mnoga jak rzeczownik: „three APIs”, „two SDKs”.
- Bez dopełniacza: „the API of the company”, nie „the API's”.

## 9. Terminologia AI (ai-terms) — tabela preferowanych

| Termin | Jak używać |
|---|---|
| **AI** | Dla wszystkich odbiorców. Nie rozwijaj „artificial intelligence” poza pierwszą definicją. |
| **model** | Tak określaj modele Mistral (Mistral Large, Codestral...). Nie używaj „the AI” jako rzeczownika oznaczającego model („the model responded”, nie „the AI responded”). |
| **agent** | Skonfigurowany model z instrukcjami i narzędziami. Nie „asystent ogólnego przeznaczenia”. |
| **bot / chatbot** | Aplikacja wykonująca zadania lub rozmawiająca przez interfejs konwersacyjny. Po pierwszym użyciu samo „bot”. |
| **tool** | Funkcja/zdolność, którą model może wywołać (wbudowane narzędzia, narzędzia Connectorów). |
| **Connector / Connectors** | Wielka litera dla produktu Mistral. Mała „connector” tylko dla konkretnej instancji („create a connector named...”). |
| **conversation** | Sesja, w której model odpowiada na serię wiadomości. |
| **prompt** | Tekst wysłany do modelu. W UI — w cudzysłowie. |
| **context / context window** | Ilość tekstu, którą model przetwarza w jednym żądaniu. Nie używaj „memory” jako synonimu (chyba że o konkretną funkcję pamięci). |
| **inference** | Proces generowania odpowiedzi. W treściach deweloperskich OK; dla szerokiego grona — rozwiń przy pierwszym użyciu. |
| **embedding** | Wektorowa reprezentacja tekstu. Treści deweloperskie OK. |
| **token** | Jednostka tekstu przetwarzana przez model. W treściach deweloperskich bez definicji. |
| **fine-tuning** | Z myślnikiem. Dalsze trenowanie modelu na danych. |
| **RAG** | Po rozwinięciu „retrieval-augmented generation” przy pierwszym użyciu. W treściach deweloperskich samo RAG. |

### Słowa, których unikać (i zamienniki)

| Unikaj | Użyj |
|---|---|
| artificial intelligence (rozwinięte) | AI |
| the AI (jako rzeczownik dla modelu) | the model |
| smart technology | intelligent technology / opisz zdolność |
| magic, magical | opisz faktyczny mechanizm |
| knows, thinks, feels, understands (o modelu) | returns, generates, identifies, detects |
| hallucinates | generates incorrect output, produces inaccurate results |
| brain, mind (dla AI) | model, system |
| kill switch | emergency stop, shutdown mechanism |

### Jak opisujemy zachowanie modelu (nie antropomorfizujemy)

| Źle | Dobrze |
|---|---|
| The model knows the answer. | The model returns an answer. |
| The model thinks you want to... | The model infers from context that... |
| The model understood your request. | The model parsed your request. |
| The model made a mistake. | The model generated an incorrect response. |
| The model remembers your preferences. | The model uses context from previous messages. |
| Ask the AI. | Send a message to the model. / Call the API. |

### Nazwy produktów Mistral — pisownia oficjalna

| Poprawnie | Źle |
|---|---|
| Connectors | MCP connectors, Mistral Connectors |
| Workflows | Mistral Workflows, Mistral AI Workflows |
| Studio | Mistral Studio, AI Studio, Mistral AI Studio |
| Vibe | **Le Chat** (przestarzałe) |
| Vibe Code | Mistral Vibe Code |
| Vibe Work | Mistral Vibe Work |

„Mistral AI” — wielka litera, gdy o firmie. Modele: Mistral Large, Mistral Small, Codestral, Pixtral, Mistral Embed — wielka litera. Zawsze `AI` wielkimi literami w prozie.

### Ton dla treści o AI

- Skupiaj się na tym, **co deweloper może zbudować**, nie na tym, jak imponująca jest technologia.
- Opisuj zdolności przez rezultaty: „Retrieve information from external sources”, nie „Leverage our powerful AI capabilities”.
- Bez superlatywów: *most advanced, state of the art, groundbreaking, revolutionary*. Opisz, co to robi.
- Bez marketingu. Niech zdolność mówi sama za siebie.

## 10. Język inkluzywny (inclusive language)

### Neutralność płciowa

| Użyj | Nie używaj |
|---|---|
| chair, moderator | chairman |
| humanity, people, humankind | man, mankind |
| workforce, staff, personnel | manpower |
| sales representative | salesman |
| synthetic, manufactured | manmade |

Nie używaj *he/him/his/she/her/hers* w odniesieniach ogólnych. Zamiast tego: „you”, liczba mnoga, „the/a + rzeczownik”, rola osoby („developer”, „user”, „reader”), „person”, „they/their/them”.

### Terminologia dostępności (people-first)

| Preferowane | Dopuszczalne (identity-first) | Zabronione |
|---|---|---|
| person who is blind, person with low vision | Blind person | sight-impaired, vision-impaired |
| person who is deaf, person with a hearing disability | Deaf person | hearing-impaired |
| person with limited mobility, person with a physical disability | physically disabled person, wheelchair user | crippled, lame, handicapped |
| is unable to speak, uses sign language, uses synthetic speech | | dumb, mute, non-verbal |
| has multiple sclerosis, cerebral palsy, a seizure disorder | | affected by, stricken with, suffers from, a victim of |
| person without a disability | non-disabled person, able-bodied person | normal person, healthy person |
| person with a prosthetic limb, person with a limb difference | amputee | maimed, missing a limb, birth defect |
| person with a disability | disabled person | the handicapped, people with handicaps |
| person with cognitive, developmental, or learning disabilities | learning disabled, dyslexic person | slow learner, mentally handicapped, differently abled, special needs |
| person with autism | autistic person, neurodivergent person | Asperger's (jako etykieta) |
| functional needs | | special needs |

Nie używaj języka litości („stricken with”, „suffers from”, „victim of”). Nie wspominaj o niepełnosprawności, jeśli to nieistotne. Przy osobach realnych — używaj zaimków zgodnych z ich preferencjami. W fikcyjnych przykładach — różnorodność (imiona spoza anglosaskiego kręgu: Amara, Kai, Sofia, Tariq).

### Rasa i pochodzenie etniczne

Wielka litera: Asian, Black and African American, Hispanic and Latinx, Native American, Alaska Native, Native Hawaiian, Pacific Islander, Indigenous Peoples. Mała litera: *multiracial*, *white*. Bez generalizacji o ludziach, krajach, regionach, kulturach. Bez slangu mogącego być przywłaszczeniem kulturowym.

### Terminologia techniczna bez biasu

| Użyj | Nie używaj |
|---|---|
| primary/subordinate | master/slave |
| allowlist / allow list | whitelist |
| denylist / block list | blacklist |
| perimeter network | demilitarized zone (DMZ) |
| stop responding | hang |

### Język militarny — unikaj

| Użyj | Nie używaj |
|---|---|
| address; protect against; respond to | combat; fight; eliminate |
| cyberattack chain | kill chain |
| threat actor, bad actor | attacker, adversary |
| multilayered approach | defense-in-depth (w treściach ogólnych) |
| protect, safeguard | guard, ward |
| vulnerabilities; points of access | attack surface (w treściach ogólnych) |

**Nigdy** w dokumentacji technicznej: *bomb, enemy lines, invade, nuke, strike, troops, air cover*. „Attack” OK, gdy określony typ: „malware attack”, „injection attack”, „brute-force attack”.

### Inkluzywne przykłady w kodzie

- fikcyjne, realistyczne imiona z różnych środowisk,
- nie tylko zachodni/zamożny styl życia w przykładach,
- neutralne adresy: `user@example.com` (nie `john@example.com`),
- neutralne placeholdery: `"your-api-key"` (nie `"mykey123"`).

## 11. Przykłady kodu w dokumentacji (developer-content)

- Zaczynaj od **najprostszego przydatnego przykładu**, rośnij w złożoność.
- Każdy blok kodu ma **tag języka**.
- Kod ma być **do skopiowania i uruchomienia**; opisz scenariusz i zależności we wstępie.
- **Nigdy nie hardkoduj sekretów** — placeholdery: `os.environ["MISTRAL_API_KEY"]`, `"your-api-key"`, `<connector-id>`.
- Pokazuj **oczekiwany wynik** po kodzie (realny, nie „response printed”).
- Komentuj tylko to, co nieoczywiste.
- Oznaczaj wartości do podmienienia w oczywisty sposób.
- Przy wielu menedżerach pakietów — **osobne bloki** z etykietą, nigdy jeden blok z komentarzami `#`:

```text
**npm**:
```bash
npm install @mistralai/mistralai
```

**pnpm**:
```bash
pnpm add @mistralai/mistralai
```
```

- Wszystkie elementy programistyczne inline w backtickach: `connector_id`, `Mistral`, `chat.complete()`, `MISTRAL_API_KEY`, `401 Unauthorized`, `model`, `messages`, `"mistral-small-latest"`, `/v1/chat/completions`.
- Prompty w prozie — w cudzysłowach: „Summarize this document.”

## 12. Struktura dokumentu referencyjnego

| Sekcja | Zawartość |
|---|---|
| Title and description | Nazwa + 1–2 zdania, co robi (nie „powtórz nazwę”) |
| Declaration/syntax | Sygnatura kodu |
| Parameters | Każdy parametr: typ, wymagalność, opis zachowania |
| Return value | Co zwraca i jakiego typu |
| Remarks | Kontekst, porównania, pułapki |
| Example | Działający przykład |
| See also | Linki |

Opisy elementów zaczynaj od czasownika: „Returns...”, „Sets...”, „Creates...”, „Deletes...”. Nie od „This method...”.

## 13. Procedury krok po kroku

1. Kroki numerowane.
2. Każdy krok zaczyna się od czasownika w imperatywie: *Install, Set, Run, Call, Copy*.
3. Podaj spodziewany rezultat, gdy nieoczywisty.
4. Kod dla kroków z kodem.
5. Wymagania przed pierwszym krokiem.
6. Na końcu — pełny stan końcowy (oczekiwany output/komunikat).

Czasowniki dla UI: *Open* (aplikacje, pliki), *Close*, *Select* (opcje, checkboxy, linki, menu), *Enter* (wpisywanie wartości), *Clear* (odznaczanie), *Go to* (nawigacja). Unikaj *press, right-click, click*, gdy działa neutralny czasownik. Sekwencje menu: `Settings > API > Keys`.

## 14. Czego nie ma w dokumentacji (sekcje wykluczone)

- język marketingowy („Mistral's powerful AI”, „cutting-edge capabilities”),
- ogólnikowe wstępy („In today's world of AI...”),
- promocyjne zakończenia („Start building today!”),
- sekcje teoretyczne bez kodu/przykładu,
- powtórzone wymagania w każdym kroku,
- **Spis treści (TOC)** — UI dokumentacji go nie renderuje, to szum,
- puste sekcje (nagłówek bez treści),
- changelog/historia wersji (to osobne miejsce),
- własne wytyczne „jak wносzić zmiany” (od tego jest CONTRIBUTING).

## 15. Słowa-wytrychy do wycinania (ściąga)

| Wytnij | Użyj |
|---|---|
| utilize | use |
| initiate / instantiate (w prozie) | start, create |
| leverage (jako czasownik) | use, take advantage of |
| seamless | (opisz, co to robi) |
| robust | (opisz zdolność) |
| powerful | (opisz, co to robi) |
| easy, simple | (pomiń — niech kod to pokaże) |
| just (minimalizujące) | (pomiń) |
| please | (pomiń — bezpośredniość wystarczy) |
| Note that... | (wytnij wstęp; podaj notatkę wprost) |
| In order to | To |
| It is important to | (powiedz, dlaczego to ważne, albo wytnij) |
| There is / there are | (przeredaguj na podmiot na początku) |
| you can | (wytnij, gdy wprowadzasz krok do wykonania) |
| MCP connectors (o produkcie) | Connectors |
