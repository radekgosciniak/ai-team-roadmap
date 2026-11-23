# AI Development Team — P2 MERGED; UI-Design-01 NEXT

**2026-11-23** · S4-B2 / Trusted CLI / native WebView2

## Stan potwierdzony

- **P1 / PR #80:** MERGED `1da392e56c6d6d3709fd7116983834cd497cd9eb`; podpisany i pakowany testowy EXE, verify, kontrolowany journal i recovery.
- **P2 / [PR #81](https://github.com/radekgosciniak/AI-Development-Team/pull/81): MERGED**. Ostateczny reviewed head `35646cddb659867ec87035760e0df5b61b21f1ff`, merge `0fd36fd28c82deca3e395364d7222afe793ef0a7`. PR zmienił tylko driver `ui/scripts/tc4-p2-native-smoke.mjs` i jego testy; **bez zmian aplikacji, IPC, ACL, TC-0**.
- **Independent Codex verdict:** `READY FOR MERGE WITH SECURITY FOLLOW-UP`. Wcześniejsze blokery cleanup / CDP: **A2 PASS, B2 PASS, B3 PASS**. Prawdziwy zapakowany `command-center.exe` + WebView2 + `terminal_session` + ConPTY + `cc-tc4-synthetic.exe` osiągnęły `ENDED / exited / clean`.
- **Native smoke:** **73 PASS, 0 FAIL, 1 NOT VERIFIED**. `Origin: null` po trzech timeoutach **NOT VERIFIED**, nie wolno przedstawiać jako odmowy ACL. Testy drivera **45/45**; natywne sondy ACL std i tc4 po **1/1**; FULL **NOT RUN**. Potwierdzono zamknięcie własnych zasobów, brak cleanup obcych procesów i ścisłe związanie CDP z połączeniem testowym.

## Następny etap — UI-Design-01 (projekt przed kodem)

1. Zachować aktualny układ i zatwierdzone decyzje UX Command Center, bez globalnego redesignu.
2. Zaprojektować makiety: terminal syntetyczny, lista/karty sesji i ich stany, podgląd ramek/outputu i wejścia, zdarzenia/approval, powiązanie task → session; uwzględnić błędy, owner_lost, interrupted i recovery.
3. Rozdzielić prototyp / mock danych od obecnego backendu; nie sugerować obsługi realnych Claude Code / Codex, których uruchamianie jest nadal zablokowane.
4. Wykonać celowany przegląd AionUi, Orca, OpenWorker z mapy adopcji; ECC wyłącznie jako przyszły kandydat do orkiestracji, bez instalacji i importu.
5. Uzyskać zatwierdzenie projektów przed rozpoczęciem `S4-B2-UI` implementacji React. Utrzymać dwa tryby dla przyszłego Project Intelligence: roadmap-governed i direct-task.

## Otwarte bramki bezpieczeństwa

- **SEC-WEBVIEW2-01 / G-ENV OPEN:** produkcyjny WebView2 respektuje `WEBVIEW2_ADDITIONAL_BROWSER_ARGUMENTS`; nieautoryzowane CDP może wykonywać JavaScript z uprawnieniami autoryzowanego `main`. Bezpieczeństwo testowego drivera nie dowodzi bezpieczeństwa startu produkcyjnego release. Potrzebny odrębny hardening i odbiór przed realnymi agentami.
- **G1 BLOCKED, D-3 Full Launch DEFERRED:** nadal tylko proces syntetyczny; Trust Broker custody-only.
- **Origin: null NOT VERIFIED**, timeout 3 razy. Przechować jako osobny niezamknięty test/ograniczenie; nie zmieniać na PASS bez dowodu.
- **FULL NOT RUN**, brak claim o pełnym E2E realnych agentów. ACL-2..ACL-5, D-P1-01/02 pozostają osobnymi przyrostami.

## Raporty lokalne

- `C:\AI-Development-Team\reports\TRUSTED-CLI\S4-B2-P2-FIX-02-FINAL-ACCEPTANCE-CODEX.md`
- `C:\AI-Development-Team\reports\TRUSTED-CLI\evidence\s4-b2-p2-fix-02-codex\`
- `C:\AI-Development-Team\reports\TRUSTED-CLI\SEC-WEBVIEW2-01-FOLLOW-UP.md`

Do momentu świadomej decyzji brak autoryzacji do kolejnych prób realnego CLI. Zmiana dotyczy tylko checkpointu roadmapy.
