# AI Development Team — aktualny status (13 listopada 2026)

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
| **UXF-2 / S2–S3** | **NOT STARTED — NEXT** | Automatyczne aliasy/profile_id, karta „Agenci projektu”, dodawanie agenta i przypisanie konta; **najpierw poprawki Claude Design D-1…D-8 i zgoda właściciela** |
| **UXF-3 / S4–S5** | **NOT STARTED** | Domyślny agent w formularzu zadania, „Zmień agenta”, zakładka „Uruchomienie”, przejście do panelu E4 |
| **UXF-4 / S6** | **NOT STARTED** | Integracja, regresja, natywny smoke Claude/Codex; osobny mały fix poziomego przewijania przy 480 px |

**Przegląd projektu:** READY WITH CHANGES. Rozpoznane korekty: nie sugerować READY bez świeżej weryfikacji i autoryzacji (`identityNotVerified`, `identityStale`, `notAuthorized`, `notChecked`); poprawić tabelę błędów i `baselineNotClean`; generator aliasów musi uwzględniać aliasy historyczne zapisane w agentach i nie dopuścić do przypadkowego nadania dostępu do nowego projektu; uwzględnić `projectUnbound`, pusty katalog i usunięty projekt. Wariant A zostaje. Konto agenta wynika z jego wiązania, nie z osobnego domyślnego konta projektu.

**UXF-1 review:** dwa uruchomienia `codex exec -s read-only` koordynowane przez Claude Code, uwagi P2/P3 naprawione, review delta READY. Użyto domyślnego profilu Windows (konto niepotwierdzone) i `reasoning effort: none`; ograniczenie dowodowe jawne. Testy wg raportu: Rust module 22 PASS, Node 507 PASS / 0 FAIL / 5 SKIP, TS/Vite PASS; pełny Rust na pierwszym commicie 545 PASS / 36 znanych Recovery FAIL / 6 SKIP. Merge `1c24518`.

## Następna kolejność

1. **UXF-2**, po dopracowaniu specyfikacji i UX przez Claude Design (D-1…D-8), zatwierdzeniu i implementacji w osobnym PR.
2. UXF-3, następnie UXF-4 — dopiero po odbiorze kompletnego UX-FLOW-01 warto prezentować użytkownikowi całość nowego przepływu.
3. **Execution F9 E5** — trwałe rekordy uruchomień; **E6** — prawdziwa lista i szczegóły Runs/Live Execution; **E7** — dalsza integracja end-to-end. Te etapy są **NOT STARTED**.
4. Po E7 pełne odświeżenie Claude Design: `UX-ACCOUNT-SIMPLIFICATION-01`, `UX-EXECUTION-SIMPLIFICATION-01`, uproszczone dodawanie kont „Add account → Login → Done”, a także zarządzanie zmianami wersji CLI bez modyfikacji kodu przy każdym wydaniu.
5. **AGENT-COLLAB-REVIEW-01 (backlog, niezaimplementowane):** Claude Code → Codex CLI → niezależny review → raport → poprawki; docelowo przez Command Center z jawnym wyborem kont/profili, kontrolą procesów oraz historią promptu, diffu, modelu, poziomu rozumowania i werdyktów. Planowanie po E7, zależność od E5/E6.

## Otwarte zadania i ograniczenia

- `UI-LIVE-EXECUTION-PLACEHOLDER-01`: główny placeholder „Live Execution is not available” nie jest usterką E4, lecz brakiem docelowego źródła historii. Właściwa naprawa w E5/E6, bez fikcyjnych danych.
- `CODEX-PROBE-DEFAULT-FEATURES-01`: identity probe Codex działał z domyślnymi funkcjami narzędzi; udokumentować i ograniczyć w osobnym zadaniu, bez ponownego otwierania zaakceptowanego E4.
- Aktualizacja CLI: oddzielić sprawdzanie zgodności adaptera E1 od binarnego pinning oraz kwalifikacji E4; unikać twardej listy wersji wymagającej zmiany kodu.
- 36 znanych niepowodzeń `provider_accounts::recovery` w pełnej regresji Rust; nie raportować całego suite jako zielonego.
- Lokalny backlog UX i raporty w `C:\AI-Development-Team\reports\ui-ux\`; wcześniejsze materiały Claude Design nie są automatycznie publikowane w tej roadmapie.
- Inne starsze workstreamy (P6-A, W3.3, UI-REFRESH) zachowują własne statusy historyczne; **nie** są automatycznie uznawane za ukończone.

