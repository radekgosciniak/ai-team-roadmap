# AI Development Team — checkpoint i roadmapa na 14 listopada 2026

**Status dnia:** SESJA ZAKOŃCZONA. Nie uruchomiono kolejnego etapu ani pracy w tle.  
**Repo aplikacji:** [radekgosciniak/AI-Development-Team](https://github.com/radekgosciniak/AI-Development-Team)  
**Potwierdzony merge HEAD:** `922aeb19653f0ac3cf2ec8367d5ac6265260762a` ([PR #68](https://github.com/radekgosciniak/AI-Development-Team/pull/68)).  
**Lokalny stan:** według końcowego raportu Claude Code `main == origin/main == 922aeb1`; główny checkout pozostawiono na osobnej gałęzi z 22 lokalnymi zmianami; worktree, profile i raporty zachowano. To potwierdzenie pochodzi z raportu lokalnego, nie z bezpośredniego dostępu do dysku.

> Ten dokument jest najnowszym statusem operacyjnym. Starsze pliki `CURRENT-STATUS-2026-11-13.md` i `EXECUTION-F9-STATUS-2026-11-12.md` pozostają migawkami historycznymi; ich dawne statusy NOT STARTED/OPEN nie opisują już obecnego stanu.

## 1. Zakończone etapy — 14.11

| Etap | Stan | Dowody / merge |
| --- | --- | --- |
| UX-FLOW-01: UXF-1/2/3, UXF-4A | MERGED | [#60](https://github.com/radekgosciniak/AI-Development-Team/pull/60), [#61](https://github.com/radekgosciniak/AI-Development-Team/pull/61), [#62](https://github.com/radekgosciniak/AI-Development-Team/pull/62), [#65](https://github.com/radekgosciniak/AI-Development-Team/pull/65) |
| UXF-4B — odbiór natywny Windows 11 | ACCEPTED WITH CONDITIONS | 17 PASS / 1 FAIL P3 / 1 NATIVE PENDING; późniejsze poprawki P3 w E6 |
| Execution F9 E4 — ograniczone read-only | MERGED / ACCEPTED WITH CONDITIONS | [#59](https://github.com/radekgosciniak/AI-Development-Team/pull/59); wyłącznie FictionalFixture, nie FULL |
| Execution F9 E5 — trwała historia runów | MERGED | [#64](https://github.com/radekgosciniak/AI-Development-Team/pull/64), `234d342ef6a993add2135c2c8d853f3c0b91e125` |
| Execution F9 E6 — Live Execution, wariant A | MERGED / ACCEPTED WITH CONDITIONS | [#66](https://github.com/radekgosciniak/AI-Development-Team/pull/66), `1946f31baac01290d72931e9a9b57f1978835b7f` |
| E6 Native Acceptance | ACCEPTED WITH CONDITIONS | 19 PASS / 1 FAIL P3 / 3 NATIVE PENDING; syntetyczna historia E5 usunięta po odbiorze |
| E6-NATIVE-FIX-01 — E6-N1…N5 | MERGED / ACCEPTED | [#67](https://github.com/radekgosciniak/AI-Development-Team/pull/67), `38f4f264f4bbd69de5f6d94e9920d697e6f082bf` |
| E-1 v4.1 S0 + F-9 + S1 oraz FIX-01 | MERGED / ACCEPTED (częściowe E-1) | [#68](https://github.com/radekgosciniak/AI-Development-Team/pull/68), `922aeb19653f0ac3cf2ec8367d5ac6265260762a` |

**E6:** historia E5 do 100 rekordów, filtry, szczegóły metadanych, czteroetapowy panel nowego runu, integracja z zadaniem, UI PL/EN i responsywność. Brak zapisu promptów/transkryptów/odpowiedzi agentów w historii. E6-NATIVE-FIX-01 zamyka E6-N1…N5, w szczególności nie pokazuje wygasłej autoryzacji jako gotowej.

**E-1 S0/F-9/S1:** zmieniona binarka daje `binaryChanged`; preflight sprawdza przejściową, zamrożoną kwalifikację E4 i scope fixture, odmawia fałszywego READY; `binaryStatus` hashuje plik bez uruchamiania CLI; naprawiono spójność `root_digest` (w tym junction), limit wejścia IPC 8192 B, parser `unpinned`, etykietę wersji. PR #68 nie włącza jeszcze Trusted CLI ani automatycznej kwalifikacji nowych wersji.

**Wyniki ostatniego odbioru PR #68:** Node 608 PASS / 0 FAIL / 10 SKIP; Rust E1 18/18 PASS; pełny Rust 583 PASS / 36 znanych `provider_accounts::recovery` FAIL / 6 ignored. Codex delta R2: MERGE WITH CONDITIONS, warunek spełniony; natywne scenariusze możliwe bez uruchomienia CLI PASS. Nie twierdzić, że pełny Rust suite jest zielony.

## 2. Nadrzędna decyzja produktowa — Trusted CLI (14.11)

**Normalny tryb:** Command Center ma używać aktualnych, oficjalnych, wiarygodnie rozpoznanych instalacji Claude Code i Codex CLI bez ręcznej, wieloetapowej kwalifikacji i nowego wydania aplikacji po każdej zwykłej aktualizacji.

- Automatycznie wykrywaj aktualizację i testuj obsługiwany kontrakt CLI; sama zmiana wersji/SHA nie powinna trwale blokować codziennej pracy.
- Sprawdzaj rzeczywiste pochodzenie i integralność pliku oraz to, że uruchamiany jest sprawdzony obraz. Sama ścieżka npm lub numer wersji nie dowodzi pochodzenia.
- Nadal obowiązują przypisanie/tożsamość konta, zakres projektu, autoryzacja zadania, ograniczenia uprawnień oraz odmowa przy wykryciu podmiany lub niezgodnego kontraktu.
- Rozbudowana kwalifikacja wersji E4/Q pozostaje **opcjonalnym trybem podwyższonego bezpieczeństwa**; nie jest domyślną bramką każdej aktualizacji oficjalnego CLI.
- Dla niezweryfikowanego źródła, niezgodnej binarki lub niewystarczającej kontroli izolacji: jasna diagnostyka i odmowa odpowiedniej operacji; nie utożsamiać „CLI działa” z „CLI jest bezpieczne”.
- To **zatwierdzona polityka docelowa, NIE funkcja już wdrożona**. Zamrożona tabela E4 w PR #68 jest rozwiązaniem przejściowym. Obecne Claude Code 2.1.325 nadal napotyka `binaryChanged`.

**Dawne decyzje wymagające dopisywania każdego SHA/wersji do kodu i wydania Command Center są zastąpione tą polityką docelową.** Nie zmieniać istniejących blokad E4 do czasu odrębnie zatwierdzonej i przetestowanej implementacji Trusted CLI.

## 3. Kolejność wznowienia — nic nie rozpoczęto

1. **TRUSTED-CLI-01 — projekt trybu domyślnego (NEXT / NOT STARTED):** wiarygodne źródło oficjalnej instalacji, kompatybilność kontraktu, reakcja na aktualizacje bez wydania aplikacji, komunikaty i sposób przełączania trybu. Zatwierdzić projekt przed implementacją. Przypadek odniesienia: Claude 2.1.323 → 2.1.325, Codex 0.167.2 niezmieniony.
2. **E4-LAUNCH-INTEGRITY-01 (LI) — REQUIRED / NOT STARTED:** luka startu procesu po ścieżce mimo uprzedniej weryfikacji pliku; wdrożyć spójne powiązanie obrazu zweryfikowanego z uruchamianym, z niezależnym review. **Wymagane przed pierwszym startem niezweryfikowanego kandydata i przed dopuszczeniem E7**. Kolejność prac nad projektem LI vs Trusted CLI może być równoległa, natomiast uruchomienie kandydatów wymaga naprawy LI.
3. **TRUSTED-CLI implementacja i odbiór (NOT STARTED):** po zatwierdzonym projekcie i rozstrzygnięciu LI; odblokowanie bez niejawnego dopuszczenia nieznanych plików.
4. **E7 — native end-to-end (NOT STARTED):** pierwszy świadomie autoryzowany, kontrolowany run; finalne scenariusze `NATIVE PENDING` z E6, UXF-4B, prawdziwe READY/wygaśnięcie i historia; żadnych nieautoryzowanych startów.
5. **EXECUTION-IDENTITY-60S-UX-01 (BACKLOG):** raz ręczna weryfikacja w sesji, a w przyszłości automatyczne świeże identity probe tuż przed świadomym startem, bez ręcznego przechodzenia między ekranami; zachować ważność VERIFIED ≤60 s i osobną autoryzację każdego runu.
6. **Opcjonalne Q1–Q4/S4 (BACKLOG):** rozbudowana kwalifikacja/sejf binarek dla podwyższonego bezpieczeństwa, nie warunek wszystkich zwykłych aktualizacji.

## 4. Otwarte ryzyka i ograniczenia

| ID | Status / kolejna decyzja |
| --- | --- |
| E-1 / Claude 2.1.325 | BLOCKED w istniejącym E4 po aktualizacji globalnego npm 13.11; nie przywracano 2.1.323 i nie kwalifikowano 2.1.325 |
| U-1 / LI (P1) | Sprawdzony SHA może nie odpowiadać faktycznie uruchomionemu obrazowi przy podmianie katalogu; obowiązkowy blocker startów kandydatów |
| U-2 (P2) | `pinBinary` dostępne z renderera bez natywnej zgody; rozstrzygnąć w projekcie Trusted CLI |
| U-3 (P2) | Logowanie wybiera wykryty .exe zamiast przypiętego obrazu; rozstrzygnąć przed bezpiecznym workflow |
| U-4 (P2) | Niezmierzona rzeczywista delegacja/sub-agenci Codex; nie uważać flag za dowód pełnej izolacji |
| U-5 (P2) | CLI Claude w dotychczasowym argv nie raportuje kompletnej powierzchni narzędzi; projekt `stream-json` przygotowany, nie wdrożony |
| F-9 | CLOSED w PR #68 |
| E6 NATIVE PENDING | Scenariusze zależne od rzeczywistego autoryzowanego startu odłożone do E7; nie są PASS |
| UI-PRIVACY-EMAIL-01 | BACKLOG; maskowanie pełnych adresów e-mail w UI kont |
| Rust Recovery 36 | Znany bazowy problem testów, nie naprawiony przez PR #68 |

Ważne dla bezpieczeństwa: MAC i testy prowadzone przez proces pod tym samym kontem Windows nie chronią przed złośliwą binarką z tymi samymi uprawnieniami. W trybie Trusted CLI wyraźnie komunikować model zaufania i jego granice.

## 5. Backlog UX i funkcje po MVP

- **UX-ACCOUNT-SIMPLIFICATION-01 / UI-ACCOUNTS-REDESIGN-01:** prostsze konta Claude/Codex (Add account → Login → Done), mniej kart diagnostycznych, szczegóły zaawansowane zwinięte; planowany jeden spójny projekt Claude Design z `ACCOUNT-USAGE-01`.
- **ACCOUNT-USAGE-01 A1–A6:** adaptacja pomysłów [Orca](https://github.com/stablyai/orca) — limity per konto, ręczny reset limitu Codex tylko za zgodą, zwarty panel kont, lista/wybór kontekstu podglądu, wskaźniki statusbar, pełny widok konta. ADAPT / NO MIGRATION. Nie przełączać po cichu rzeczywistego konta agenta.
- **UX-EXECUTION-SIMPLIFICATION-01:** dalsze uproszczenie Run/Preflight i obsługi sesji po E7, w powiązaniu z `EXECUTION-IDENTITY-60S-UX-01`.
- **AGENT-COLLAB-REVIEW-01:** Claude Code → Codex → niezależny review → raport/naprawy ze śledzeniem kont, procesów i historii; po E7.
- Pozostałe historyczne workstreamy (P6-A, W3.3, ACC-ID-01 itd.) zachowują własne statusy i nie są automatycznie uznawane za zamknięte.

## 6. Raporty lokalne / checkpoint

- `C:\AI-Development-Team\reports\SESSION-CHECKPOINT-2026-11-14.md` — checkpoint sesji (raport Claude).
- `C:\AI-Development-Team\reports\EXECUTION-F9\EXECUTION-F9-BACKLOG.md` — szczegółowy backlog.
- `C:\AI-Development-Team\reports\EXECUTION-F9-E6\` — odbiór natywny i FIX-01.
- `C:\AI-Development-Team\reports\E-1-CLI-UPDATE\` — S0, F-9, S1, FIX-01, projekt v4.1 oraz dowody review.

Roadmapa publiczna zawiera tylko opis stanów i decyzji; nie zawiera lokalnych dowodów, profili, danych kont ani sekretów.
