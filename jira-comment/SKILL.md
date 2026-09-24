---
name: jira-comment
description: Użyj przed napisaniem, poprawieniem lub wysłaniem jakiegokolwiek komentarza albo odpowiedzi w Jirze — gdy user prosi „odpisz", „odpowiedz PM-owi/QA", „daj tekst do wklejenia", wkleja komentarz z Jiry i chce odpowiedzi, albo gdy odpowiadasz na uwagi QA/CR w tickecie.
---

# Komentarz do Jiry

## Zasada

Komentarz czyta jedna osoba, często nietechniczna, w Jirze — bez Ciebie i bez kontekstu tej rozmowy. Ma wyglądać, jakby user napisał go ręcznie do tej osoby. Wszystko o Tobie, Twoich narzędziach i Twoim procesie zostaje w czacie z userem.

## Krok 1 — Tryb

Polecenie nie nazywa odbiorcy ani trybu → zapytaj, zanim napiszesz choć zdanie draftu. Narzędziem AskUserQuestion, jedno pytanie, dwie opcje:

- **Normalnie** — odbiorca techniczny: dev, QA, odpowiedź na komentarz pisany Claudem.
- **Mocno ludzko i nietechnicznie** — PM, biznes, BOK, sprzedawca.

Nie zgaduj z imienia, roli ani ze stylu komentarza. „Leć", „szybko", „mam call" nie zwalniają z pytania — pytanie to jedna linijka. Bez dostępu do narzędzia zadaj pytanie tekstem i zatrzymaj się.

Tryb podany wprost („odbiorca to PM, nie jest techniczny", „odpisz normalnie") → bez pytania.

## Krok 2 — Kształt komentarza (oba tryby)

Komentarz składa się wyłącznie z:

1. odpowiedzi albo decyzji — po jednej na każdy punkt, który poruszył autor,
2. opcjonalnie jednej linii o tym, co czytający zobaczy albo ma zrobić („wejdzie na QA z najbliższym wdrożeniem", „kliknij Odśwież produkty przy koncie"),
3. opcjonalnie jednego pytania, jasno postawionego.

Nic poza tym: bez wstępu i rozbiegu („tłumaczę dokładnie, skąd…"), bez podziękowań-wypełniaczy, bez zamknięcia („daj znać, jeśli…"), bez nagłówków i bolda do porządkowania, bez powtarzania liczb, które autor już napisał.

Odpowiedź na uwagę QA/CR to jedno słowo lub jedna linijka: „Naprawione.", „Dodane.", „Tak, celowe." Mechanizm wyjaśniasz tylko wtedy, gdy bez tego odbiorca nie wie, co ma dalej zrobić.

W komentarzu nie ma, niezależnie od trybu:

- **Ciebie i narzędzi**: „screenshot się nie załadował", „nie mogę pobrać nagrania", „z kodu wynika", „grep pokazuje", „numeracja jak w Twoim komentarzu". Nie widzisz załącznika → powiedz to userowi w czacie i zapytaj, co na nim jest. W komentarzu odpowiadasz tylko na to, co wiesz, a o resztę pytasz autora jednym zdaniem („Której oferty dotyczy nagranie?", „Ile pokazuje panel, a ile marketplace?") — bez tłumaczenia, dlaczego pytasz. Nie zakładasz „typowego przypadku" i nie orzekasz („to nie błąd"), gdy rozstrzyga o tym właśnie to, czego nie widzisz.
- **Identyfikatorów, które nic nie zmieniają dla czytającego**: hashe commitów, numery MR-ów, nazwy funkcji, pól, kolekcji, handlerów.

## Krok 3 — Warstwa „mocno ludzko"

- Jedno zdanie na poruszony punkt, najwyżej trzy.
- Każde słowo techniczne zamień na skutek widoczny w panelu albo w sklepie — albo wytnij. Nie „job schedulera przelicza batchami co 12h", tylko „liczba dogania się sama w ciągu 12 godzin".
- Nie używasz: job, batch, endpoint, handler, backend, indeks, kolekcja, schemat, strategia, sync, MR, commit, repo, deploy (→ wdrożenie), staging (→ wersja testowa).
- Zero liczb i identyfikatorów, które nie zmieniają decyzji czytającego.
- Test przed pokazaniem: „Zrozumie bez dopytywania i bez słownika?" Jeśli chcesz dodać wyjaśnienie w nawiasie — przepisz zdanie.

## Krok 4 — Gate

Pokaż pełny tekst komentarza w czacie i czekaj na jawne „ok". Bez „ok" nie wywołujesz żadnego narzędzia piszącego do Jiry. Po poprawkach pokaż całość ponownie.

## Przykłady

PM pyta: „Przy koncie X panel pokazuje mniej ofert niż marketplace (screen). Skąd różnica? Trzeba coś klikać? I czy przy „Nie rób nic" oferty znikną z platformy?"

❌ 230 słów: „Cześć X, dzięki za sprawdzenie na stagingu, tłumaczę dokładnie skąd biorą się różnice i co warto wiedzieć: **1. Skąd różnica 1 842 vs 2 107 ofert?** Różnica ma dwa źródła: …" — trzy nagłówki, listy, te same liczby dwa razy, „Daj znać, jeśli coś jeszcze budzi wątpliwości."

✅ „Różnica to oferty dodane ręcznie w marketplace (tych panel nie liczy, bo nie pochodzą z pliku) plus oferty z ostatnich godzin, które jeszcze się nie przeliczyły. Te drugie dogonią się same w ciągu 12 godzin albo od razu po kliknięciu „Odśwież produkty" przy koncie. „Nie rób nic" niczego nie usuwa — istniejące oferty zostają, przestajemy tylko dodawać i zmieniać oferty z pliku."

QA po reteście, cztery punkty (1–2 OK, 3 sort skacze, 4 przycisk tylko dla admina — celowe?), tryb normalny:

❌ „Dzięki za retest! **Ad 3.** To pre-existing bug, nie z tego MR-a — sort szedł po `offerId`, które nie jest unikalne między integracjami, stąd niestabilna kolejność między stronami. Poprawka (sort po `_id`) jest już w kodzie: commit `e70dc494f36`, MR !27805. …"

✅ „1–2. OK. 3. Naprawione, wejdzie na QA z najbliższym wdrożeniem, dam znać do retestu. 4. Celowe — support dostanie tę akcję osobnym taskiem."

## Racjonalizacje

| Myśl | Rzeczywistość |
|---|---|
| „User napisał «leć», nie ma czasu na pytanie o tryb" | Pytanie to jedna linijka. Zły tryb = skasowany komentarz i „to jakaś kpina". |
| „PM narzekał, że odpowiedzi są niepełne, więc piszę wyczerpująco" | Niepełne ≠ krótkie. Niepełne = bez odpowiedzi na któryś punkt. Każdy punkt, jedno zdanie. |
| „Wyjaśnię, czemu nie odnoszę się do screena" | To zdanie o Tobie, nie o sprawie. Zapytaj usera w czacie, co jest na screenie. |
| „QA to technik, commit i nazwa pola mu pomogą" | Pomogą devowi w MR-ze. W tickecie QA chce wiedzieć: naprawione czy nie i gdzie retestować. |
| „Krótko wyjdzie niegrzecznie" | Jedno zdanie na temat jest grzeczniejsze niż ściana tekstu czytana dwa razy. |
| „Nagłówki i bold ułatwią czytanie" | Trzy zdania nie potrzebują nawigacji. Struktura zdradza maszynę. |
| „Mam call za 3 minuty, założę typowy przypadek i dopiszę userowi uwagę w czacie" | Uwaga w czacie nie dociera do czytającego. Do komentarza wchodzi to, co wiesz; o resztę pytasz autora jednym zdaniem. |

## Czerwone flagi — zatrzymaj się i przepisz

- W komentarzu jest „nie mogę", „nie udało się", „nie załadował".
- Więcej niż jeden akapit na jeden poruszony punkt.
- Hash, numer MR-a, nazwa funkcji lub pola w trybie ludzkim.
- Zaczynasz od „Cześć X, dzięki za…, poniżej…".
- W komentarzu jest orzeczenie („to nie błąd", „tak ma być"), które zależy od informacji, której nie masz.
- Wybrałeś tryb sam, bo „było oczywiste".
- Wywołujesz narzędzie piszące do Jiry bez „ok" w tej rozmowie.
