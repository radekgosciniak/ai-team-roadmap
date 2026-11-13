# AI Development Team — aktualny status (13 listopada 2026)

## Aktualizacja końca dnia — 13.11.2026, po PR #63

**Aktualny stan GitHub (potwierdzony):**
- **UXF-1:** MERGED — [PR #60](https://github.com/radekgosciniak/AI-Development-Team/pull/60), merge `1c24518`.
- **UXF-2 / S2–S3:** MERGED — [PR #61](https://github.com/radekgosciniak/AI-Development-Team/pull/61), merge `f86db18`; Codex R5 READY, przegląd zamknięty. Natywne zapisy pozostają do UXF-4.
- **UXF-3 / S4–S5:** MERGED — [PR #62](https://github.com/radekgosciniak/AI-Development-Team/pull/62), merge `bfdec8aa9743d40a0664c7bbe3a42e1902b13ee5`; formularz Task, agent, zakładka „Uruchomienie”, przejście do panelu; Codex READY; Node 564 PASS / 0 FAIL / 5 SKIP.
- **UI-POLISH-PREFLIGHT-01:** MERGED — [PR #63](https://github.com/radekgosciniak/AI-Development-Team/pull/63), merge `e81f9f907f835d25d99e243eb28f595f63ed66c6`, HEAD PR `a09c092`; CSS Project/Task selects i panelu, Codex R2 READY; Node 567 PASS / 0 FAIL / 5 SKIP, TypeScript/build PASS. Bez zmian logiki/IPC/Rust.
- **Execution F9 E1–E4:** MERGED, E4 nadal ACCEPTED WITH CONDITIONS (wyłącznie FictionalFixture). **E5–E7 NOT STARTED**.

**Stan następny:** **UXF-4 / S6 — NOT STARTED / NEXT**, pełny natywny odbiór projektu → agenta → konta → zadania → Preflight/Execution i ścieżek zapisu za zgodą operatora na izolowanych danych. Docelowy redesign i rzeczywisty Live Execution po E5/E6. `ACCOUNT-USAGE-01` (limity, reset Codex, zwarty wybór kont Orca) i `AGENT-COLLAB-REVIEW-01` to backlog, nie wymagania UXF-4.

**UWAGA o testach:** pełny Rust UXF-3 został zatrzymany po zawieszeniu testu `execution_executor` (233/589, 0 fail do chwili zatrzymania); **nie jest pełnym PASS**. W UXF-3 nie było zmian Rust. Znany osobno baseline: 36 FAIL w `provider_accounts::recovery`. Późniejsza diagnostyka; bez automatycznego twierdzenia o braku nowej regresji.

**Zamknięcie dnia:** wymagane lokalne `main == origin/main == e81f9f9` po bezpiecznym fetch/fast-forward, z zachowaniem głównego checkout, wszystkich worktree, profili i lokalnych dowodów. Nie jest jeszcze potwierdzone. Trwały checkpoint stanu, raportów i decyzji w lokalnym repo; brak rozpoczęcia UXF-4 i E5–E7 dziś.

**Pierwszeństwo:** ta aktualizacja zastępuje nieaktualne statusy UXF-2/UXF-3 w historycznych sekcjach poniżej; starsze dane pozostawiono jako zapis wcześniejszej migawki.

---


> **Bieżący status nadrzędny** względem historycznego raportu E1–E3 z 12 listopada. Fakty o PR potwierdzone na GitHub; wyniki testów i lokalny stan main na podstawie raportów odbiorowych Claude Code.

## Wydane do main

| Etap | Status | Źródło |
| --- | --- | --- |
| ACC-ID-01, ACC-LOGIN-01, ACCOUNT-BIND-01 | MERGED | [PR #53](https://github.com/radekgosciniak/AI-Development-Team/pull/53), [#54](https://github.com/radekgosciniak/AI-Development-Team/pull/54), [#55](https://github.com/radekgosciniak/AI-Development-Team/pull/55) |
| Execution F9 E1–E2 — identity probe i weryfikacja UI | MERGED, FUNCTIONAL PASS | [PR #56](https://github.com/radekgosciniak/AI-Development-Team/pull/56) |
| Execution F9 E3 — preflight | MERGED, FUNCTIONAL PASS | [PR #57](https://github.com/radekgosciniak/AI-Development-Team/pull/57) |
| UI-BUG-OVERFLOW-01 — korekta powłoki / preflight | MERGED | [PR #58](https://github.com/radekgosciniak/AI-Development-Team/pull/58) |
| **Execution F9 E4-A…E4-D** — read-only execution | **MERGED, ACCEPTED WITH CONDITIONS** | [PR #59](https://github.com/radekgosciniak/AI-Development-Team/pull/59), merge `4855e4c` |
| **UX-FLOW-01 UXF-1 / S1** — magazyn domyślnego agenta projektu | **MERGED** | [PR #60](https://github.com/radekgosciniak/AI-Development-Team/pull/60), merge `1c245187a6aca72057b20a7b2135712ea750d3c9` |

**Zgłoszona synchronizacja lokalna:** `main == origin/main == 1c24518`; osobne worktree, lokalne raporty, profile i dowody zachowane.

## Execution E4 — rzeczywisty wynik

- **Claude Code 2.1.323: FUNCTIONAL PASS** na FictionalFixture. Testy normalnego odczytu i zabronionych operacji bez modyfikacji fixture; obsługa odmowy, wygasłej tożsamości, Cancel i zwolnienia blokad.
- **Codex 0.167.2: FUNCTIONAL PASS warunkowo** po poprawce Safe Read-only. Dwa natywne runy COMPLETED pod MXC, brak przestrzeni konektorów widocznych modelowi, fixture bez zmian, procesy i blokady zwolnione.
- **Nie przyznano kwalifikacji FULL.** E4 uprawnia wyłącznie do ograniczonych testów fikcyjnych fixture. Codex może czytać poza workspace; narzędzia sub-agentów pozostają znanym ograniczeniem. `CODEX-PROBE-DEFAULT-FEATURES-01` pozostaje w backlogu.
- Wynik regresji E4 wg raportu: Rust 524 PASS / 36 znanych Recovery FAIL / 6 SKIP; Node 502 PASS / 0 FAIL / 5 SKIP, TS/Vite PASS. Brak CI raportowanego na PR.
- Nie utożsamiać ukończonego E4 z docelowym wykonywaniem dowolnych zadań na rzeczywistych projektach.

## UX-FLOW-01 — zatwierdzony wariant A „Projekt jako centrum”

**Cel:** Projekt → dodanie agenta → wybór konta → utworzenie zadania → przejście do Execution, bez ręcznych aliasów, `profile_id` i rewizji.

| Pakiet | Status | Zakres / warunek |
| --- | --- | --- |
| **UXF-1 / S1** | **MERGED — PR #60** | `CC-PROJECT-DEFAULTS/1`, osobna rewizja projektu, get/set/clear; domyślny agent tylko wstępnie wypełnia formularz; bez zmian istniejących zadań i Execution |
| **UXF-2 / S2–S3** | **PR #61 OPEN — READY WITH CONDITION, NOT MERGED** | Implementacja S2/S3, automatyczne aliasy i karta „Agenci projektu”; 4 rundy Codex, 9 znalezisk P2 poprawiono; runda 5 (ostatnia delta) jeszcze niewykonana; natywne ścieżki zapisu do odbioru w UXF-4 |
| **UXF-3 / S4–S5** | **NOT STARTED** | Domyślny agent w formularzu zadania, „Zmień agenta”, zakładka „Uruchomienie”, przejście do panelu E4 |
| **UXF-4 / S6** | **NOT STARTED** | Integracja, regresja, natywny smoke Claude/Codex; osobny mały fix poziomego przewijania przy 480 px |

**Przegląd projektu:** DESIGN READY FOR IMPLEMENTATION (FIX-01 v1.2, D-1…D-8 poprawione; teksty zakładki „Uruchomienie” zatwierdzone). Rozpoznane korekty: nie sugerować READY bez świeżej weryfikacji i autoryzacji (`identityNotVerified`, `identityStale`, `notAuthorized`, `notChecked`); poprawić tabelę błędów i `baselineNotClean`; generator aliasów musi uwzględniać aliasy historyczne zapisane w agentach i nie dopuścić do przypadkowego nadania dostępu do nowego projektu; uwzględnić `projectUnbound`, pusty katalog i usunięty projekt. Wariant A zostaje. Konto agenta wynika z jego wiązania, nie z osobnego domyślnego konta projektu.

**UXF-1 review:** dwa uruchomienia `codex exec -s read-only` koordynowane przez Claude Code, uwagi P2/P3 naprawione, review delta READY. Użyto domyślnego profilu Windows (konto niepotwierdzone) i `reasoning effort: none`; ograniczenie dowodowe jawne. Testy wg raportu: Rust module 22 PASS, Node 507 PASS / 0 FAIL / 5 SKIP, TS/Vite PASS; pełny Rust na pierwszym commicie 545 PASS / 36 znanych Recovery FAIL / 6 SKIP. Merge `1c24518`.

## Następna kolejność

1. **UXF-2 / PR #61** — implementacja gotowa warunkowo, **bez merge**. Oczekuje na niezależną rundę 5 Codex dla delty `bb46354..c4a296d`; potem decyzja właściciela o merge. Użytkownik potwierdził ręczny reset limitu Codex 13.11.2026; sam reset nie uruchamia review.
2. UXF-3, następnie UXF-4 — dopiero po odbiorze kompletnego UX-FLOW-01 warto prezentować użytkownikowi całość nowego przepływu.
3. **Execution F9 E5** — trwałe rekordy uruchomień; **E6** — prawdziwa lista i szczegóły Runs/Live Execution; **E7** — dalsza integracja end-to-end. Te etapy są **NOT STARTED**.
4. Po E7 pełne odświeżenie Claude Design: `UX-ACCOUNT-SIMPLIFICATION-01`, `UX-EXECUTION-SIMPLIFICATION-01`, uproszczone dodawanie kont „Add account → Login → Done”, a także zarządzanie zmianami wersji CLI bez modyfikacji kodu przy każdym wydaniu.
5. **AGENT-COLLAB-REVIEW-01 (backlog, niezaimplementowane):** Claude Code → Codex CLI → niezależny review → raport → poprawki; docelowo przez Command Center z jawnym wyborem kont/profili, kontrolą procesów oraz historią promptu, diffu, modelu, poziomu rozumowania i werdyktów. Planowanie po E7, zależność od E5/E6.

## ACCOUNT-USAGE-01 — limity i ręczny reset Codex (BACKLOG)

Wzorzec: [Orca](https://github.com/stablyai/orca) — widok limitów osobno dla sesji/tygodnia, odnowienie, aktywne konto/profil, dostępne kredyty resetu, okno potwierdzenia. **ADAPT / NO MIGRATION**, bez kopiowania kodu bez weryfikacji licencji i API.

- **A1 — odczyt:** limity per managed account i provider (Codex; Claude tylko po weryfikacji obsługi), zużycie i czas odnowienia; UNKNOWN/UNSUPPORTED gdy brak wiarygodnych danych, bez wyprowadzania stanu z etykiety profilu.
- **A2 — reset Codex:** wyłącznie manualny, tylko dla aktywnego i jawnie wybranego konta; sprawdzić dostępność kredytów, wyświetlić koszt (1 kredyt), zakres i termin ważności, potwierdzić natywnie, wykonać operację idempotentnie i ponownie odczytać stan. Nigdy automatycznie przez agenta.
- **A3 — kompaktowy panel kont i limitów (wzorzec Orca):** rozwijany popover z podziałem Claude/Codex, przełącznik Compact/Detailed, limity sesyjne i tygodniowe (plus dodatkowe pule wyłącznie jeśli provider je udostępnia), procent zużycia, czas odnowienia oraz znacznik czasu ostatniego wiarygodnego odczytu. Przy braku potwierdzonych danych pokazuj UNKNOWN/UNSUPPORTED, nie fałszywe 0%.
- **A4 — lista i szybki wybór kontekstu konta:** lista profili/kont z zaznaczonym aktualnie przeglądanym kontem, dostawcą, typem profilu (managed / system default), stanem rzeczywistej weryfikacji i informacją „odświeży się przy następnym uruchomieniu”, jeśli to prawda. Wybór w panelu zmienia **wyłącznie kontekst podglądu / propozycję dla przyszłych nowych operacji** — nie przełącza po cichu tożsamości w działającym CLI, nie zmienia przypisania żadnego agenta, nie obchodzi EXPECTED/VERIFIED. Akcja „Zmień konto agenta” pozostaje osobnym jawnym przepływem z istniejącymi kontrolami rewizji i unieważnienia autoryzacji.
- **A5 — skrócone wskaźniki w pasku statusu:** Claude i Codex — najważniejszy dostępny limit, procent, czas odnowienia, opcjonalny stan wybranego konta; kliknięcie otwiera A3. Pasek nie powinien pokazywać wartości konta innego niż jawnie wybrane ani utożsamiać „wybrane” z „zweryfikowane”.
- **A6 — pełny widok konta:** z jednego miejsca: provider → konto/profil → limity → przypisani agenci → status EXPECTED/VERIFIED/stale/unknown, z odnośnikami do istniejących ekranów i osobnych akcji; brak ujawniania tokenów/sekretów. Dostępność i stan „Active” z Orca nie są dowodem aktywnej tożsamości w Command Center.
- **Warunki UX/odbioru A3–A6:** stany ładowania, brak danych i błąd, wiele kont tego samego dostawcy, różne okna limitów, limity wyczerpane, odnowienie, konto niezweryfikowane, przełączenie samego podglądu bez wpływu na runy i konta przypisane agentom, responsywność i klawiatura. Najpierw adaptacja wzorca UX, potem weryfikacja rzeczywistych danych i testy; bez migracji do Orca.
- **Przed implementacją:** zweryfikować dostępny protokół/wersję Codex i zgodność RPC (np. `account/rateLimits/read` oraz ewentualny reset), izolację profili, semantykę błędów i zasady autoryzacji. Obecność tych metod nie jest jeszcze niezależnie zweryfikowana w tej roadmapie.
- **Termin:** po UXF-4 lub równolegle z E5, bez blokowania PR #61 i UXF-3.
- **Fakt operacyjny:** użytkownik potwierdził, że 13.11.2026 samodzielnie użył dostępnego kredytu resetu limitu Codex w Orca. Nie oznacza to automatycznej kwalifikacji konta ani uruchomienia zaległego review UXF-2, którego wynik nadal nie został przekazany.

## Otwarte zadania i ograniczenia

- `UI-LIVE-EXECUTION-PLACEHOLDER-01`: główny placeholder „Live Execution is not available” nie jest usterką E4, lecz brakiem docelowego źródła historii. Właściwa naprawa w E5/E6, bez fikcyjnych danych.
- `CODEX-PROBE-DEFAULT-FEATURES-01`: identity probe Codex działał z domyślnymi funkcjami narzędzi; udokumentować i ograniczyć w osobnym zadaniu, bez ponownego otwierania zaakceptowanego E4.
- Aktualizacja CLI: oddzielić sprawdzanie zgodności adaptera E1 od binarnego pinning oraz kwalifikacji E4; unikać twardej listy wersji wymagającej zmiany kodu.
- 36 znanych niepowodzeń `provider_accounts::recovery` w pełnej regresji Rust; nie raportować całego suite jako zielonego.
- Lokalny backlog UX i raporty w `C:\AI-Development-Team\reports\ui-ux\`; wcześniejsze materiały Claude Design nie są automatycznie publikowane w tej roadmapie.
- Inne starsze workstreamy (P6-A, W3.3, UI-REFRESH) zachowują własne statusy historyczne; **nie** są automatycznie uznawane za ukończone.

