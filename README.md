# jira-comment

Skill dla Claude Code (działa też w Codex CLI), który pilnuje, żeby komentarze pisane do Jiry brzmiały jak napisane ręcznie przez człowieka — krótko, na temat, bez śladów AI i bez wewnętrznego żargonu.

## TL;DR

1. Skopiuj katalog `jira-comment/` do `~/.claude/skills/`.
2. Gdy prosisz Claude o odpowiedź w Jirze („odpisz PM-owi", „daj tekst do wklejenia", „odpowiedz na uwagi QA"), skill włącza się sam.
3. Skill pyta o tryb (techniczny / mocno ludzki), pokazuje pełny tekst komentarza i **czeka na Twoje „ok"** zanim cokolwiek trafi do Jiry.

## Problem, który rozwiązuje

Model lewą ręką pisze 230 słów z nagłówkami, boldem, „Cześć X, dzięki za…", trzema listami i zamknięciem „daj znać, jeśli…". Odbiorca czyta to dwa razy, a i tak widzi, że pisała maszyna. Do tego w komentarzu lądują rzeczy, które nic mu nie mówią: hashe commitów, numery MR-ów, nazwy funkcji, „z kodu wynika", „screenshot się nie załadował".

Skill wymusza odwrotność: jedna odpowiedź na każdy poruszony punkt, opcjonalnie jedna linia „co zobaczysz / co zrobić", opcjonalnie jedno pytanie. Nic więcej.

## Co robi krok po kroku

| Krok | Co się dzieje |
|---|---|
| 1. Tryb | Jeśli polecenie nie mówi, kto czyta, skill pyta: **Normalnie** (dev, QA) czy **Mocno ludzko i nietechnicznie** (PM, biznes, BOK, sprzedawca). Nie zgaduje z imienia ani roli. |
| 2. Kształt | Odpowiedź/decyzja per punkt autora + max jedna linia o skutku + max jedno pytanie. Bez wstępu, podziękowań, zamknięcia, nagłówków, bolda, powtarzania liczb. Odpowiedź na uwagę QA/CR to zwykle jedno słowo: „Naprawione.", „Tak, celowe." |
| 3. Warstwa ludzka | W trybie nietechnicznym: jedno zdanie na punkt, każde słowo techniczne zamienione na skutek widoczny w panelu albo wycięte (nie „job schedulera przelicza batchami co 12h", tylko „liczba dogania się sama w ciągu 12 godzin"). |
| 4. Gate | Pełny tekst w czacie, czekanie na jawne „ok". Bez „ok" żadne narzędzie piszące do Jiry nie jest wywoływane. Po poprawkach całość pokazana ponownie. |

Czego w komentarzu nie ma nigdy: informacji o modelu i jego narzędziach („nie mogę pobrać nagrania", „grep pokazuje"), identyfikatorów bez znaczenia dla czytającego (hash, MR, nazwa funkcji/pola/kolekcji), orzeczeń zależnych od czegoś, czego model nie widział (np. screen, który się nie załadował — o to pyta się autora jednym zdaniem, bez tłumaczenia dlaczego).

## Instalacja

Claude Code (globalnie, wszystkie projekty):

```bash
git clone git@github.com:MarekBartczak/jira-comment.git
cp -r jira-comment/jira-comment ~/.claude/skills/
```

Claude Code (jeden projekt): to samo do `<repo>/.claude/skills/jira-comment/`.

Codex CLI: `~/.codex/skills/jira-comment/` — symlink do kopii z Claude działa, jedno źródło.

Skill sam nie pisze do Jiry. Po „ok" Claude używa tego narzędzia do Jiry, które masz podpięte (np. MCP Atlassian `addCommentToJiraIssue`). Bez narzędzia — kopiujesz tekst ręcznie.

## Użycie

Wystarczy naturalne polecenie w rozmowie, np.:

- „odpisz QA w PROJ-1234, uwagi 1 i 3 naprawione, 2 celowe"
- „daj tekst do wklejenia dla PM-a, że różnica w liczbie ofert to te dodane ręcznie"
- wklejony komentarz z Jiry + „co mu odpowiedzieć?"

Albo wprost: `/jira-comment <ticket> <co odpowiedzieć> tryb: Ludzko`.

Tryb można podać od razu w poleceniu („odbiorca to PM, nie jest techniczny", „odpisz normalnie") — wtedy skill nie pyta.

## Dostosowanie do swojego zespołu

`SKILL.md` jest po polsku i zawiera przykłady z konkretnymi imionami i realiami (Allegro, „QA2", „Odśwież produkty"). Przed użyciem u siebie:

- podmień przykłady w sekcji **Przykłady** na własne — model uczy się z nich tonu,
- w **Krok 3** dopisz do listy zakazanych słów żargon swojej firmy,
- w `description` (frontmatter) zostaw frazy, na które skill ma się włączać — to po nich Claude go dobiera.

## Dlaczego tak

Komentarz czyta jedna osoba, często nietechniczna, bez kontekstu rozmowy z modelem. Ma wyglądać, jakby autor ticketu dostał odpowiedź od człowieka, który przeczytał jego punkty i na każdy odpowiedział. Wszystko o modelu, jego narzędziach i procesie zostaje w czacie z użytkownikiem. Sekcja **Racjonalizacje** i **Czerwone flagi** w `SKILL.md` wyliczają typowe wymówki modelu („użytkownik napisał «leć», nie ma czasu na pytanie o tryb") i momenty, w których ma się zatrzymać i przepisać.

## Licencja

MIT — patrz `LICENSE`.
