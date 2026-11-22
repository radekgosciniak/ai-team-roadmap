# AI Development Team — ACL-1 MERGED / kolejny etap S4-B2
**22 listopada 2026 · po scaleniu PR #79**

## Stan główny

Aplikacja: [AI-Development-Team](https://github.com/radekgosciniak/AI-Development-Team).

- S1 / PR #74: wspólny rejestr sesji — MERGED.
- S2 / PR #75: syntetyczny ConPTY — MERGED.
- S3 / PR #76: sanitizacja VT/Unicode i heurystyczna redakcja — MERGED.
- S4-A / PR #77: dziennik, recovery, własność i NTFS lock — MERGED `fe16a1507e94b3615d53ed9c5c0725ea8eb6a7df`.
- S4-B1 / PR #78: Tauri IPC wyłącznie synthetic, Caller na instancję natywnego okna, VERIFY→HOLD→CLAIM→LAUNCH — MERGED `53623b9e8ad646651c4ea8bd26e9ff875ad75a70`.
- **ACL-1 / [PR #79](https://github.com/radekgosciniak/AI-Development-Team/pull/79)**: jawny AppManifest i capabilities dla webview `main` — **MERGED** `1dc231d1094de671ab2048f2fa4c389f26e9da30`. Head PR `4a56a228616142ab2fd0d5bceeffd525be25fb62`.

## ACL-1 — co ukończono

- Główne lokalne webview `main` otrzymuje jawny dostęp do 29 komend bazowych. Z funkcją Windows + `tc4-synthetic` dochodzi osobna komenda `terminal_session` (razem 30). Grant nie jest powiązany z całym oknem ani innymi webview.
- Inne webview oraz zdalne pochodzenia nie dostają uprawnień; nowa komenda bez grantu jest domyślnie odrzucana.
- Dodatkowe capability są w `capabilities-acl/main-webview.json` i `capabilities-tc4/terminal-session.json`. Dotychczasowe `capabilities/main-window.json` i `tauri.conf.json` pozostają nienaruszone (**D-ACL-09 zaakceptowana**).
- Uprawnienia zdefiniowano ręcznie bez globalnych `deny-*`, wildcardów i `core:*`.
- Testowy `MockRuntime` nie został dodany przez niezgodność przypiętych wersji `tauri` i `tauri-utils` (**D-ACL-10 odroczona**, bez aktualizacji zależności).
- Komendy legacy `tasks_*` pozostają dla zachowania parytetu do ACL-3; cztery komendy `projects_*` bez własnej kontroli natywnego wywołującego — do ACL-2.

### Dowody i ograniczenia

Implementator: 19/19 testów ACL+nawigacji w obu wariantach, 29/29 regresji S4-B1, 14/14 testów terminal_session, 5/5 UX flow, frontend 618 PASS / 9 wcześniej pomijanych; debug i release z i bez TC4 PASS, `Cargo.lock` bez zmian. Niezależny Codex: **READY FOR MERGE WITH FOLLOW-UP CONDITIONS**. Prawdziwy WebView2: `main` 29/29 (standard) i 30/30 (TC4) pozytywnych wywołań, `secondary` 34/34 odmów w obu wariantach; dodatkowe webview i remote bez grantu. Cztery pliki ACL wywołują przebudowę; błędna referencja kończy build błędem. Natywny smoke standardowego release wyświetlił Projects i pobrał listę poprzez IPC; pełny interaktywny smoke terminala NIE WYKONANY. **FULL NOT RUN**.

36 znanych porażek `provider_accounts::recovery` reprodukuje się identycznie na bazowym main i head ACL-1; wymagają osobnej poprawnej fikstury, nie są regresją ACL-1.

## NEXT — S4-B2

1. **Packaging synthetic binary:** zbudować `cc-tc4-synthetic.exe`, dostarczyć obok aplikacji testowej, wyliczyć i osadzić właściwy build-time SHA-256, wykazać `syntheticUnavailable` przy braku lub błędnym pinie. Nie instalować realnych CLI jako substytutu.
2. **Native Tauri smoke:** na Windows z fizycznym webview i `tc4-synthetic`, testować create/start/status/read/write_input/finish, owner_lost po zamknięciu, recovery, denied cases, bez uruchamiania rzeczywistych agentów. Sprawdzić też standardowy release bez komendy/grantu.
3. **React UI terminala:** dopiero po zamknięciu wcześniejszych dwóch bramek; wyłącznie syntetyczne sesje.

Osobne przyrosty bezpieczeństwa: **ACL-2** — natywna kontrola wywołującego dla czterech komend `projects_*`; **ACL-3** — ograniczenie trzech legacy `tasks_*`; **ACL-4** — polityka przyszłych okien; **ACL-5** — bramka CI dla nowo dodanych komend. Nie łączyć automatycznie tych prac z packagingiem.

Zgodnie z polityką projektu, każde kolejne zlecenie implementacyjne zawiera celowany przegląd właściwych repozytoriów i materiałów z [EXTERNAL-REPOSITORIES-ADOPTION-MAP.md](EXTERNAL-REPOSITORIES-ADOPTION-MAP.md), decyzje ADOPT / ADAPT / REJECT oraz brak kopiowania kodu.

## Otwarte granice

- **G-ENV OPEN** — brak oczyszczonego środowiska uruchamiania niesyntetycznych procesów.
- **G1 BLOCKED / D-3 Full Launch DEFERRED**.
- **D-FR-1/2/3 OPEN / DEFERRED**.
- Trust Broker pozostaje **Custody-Only**.
- Nie ma zgody na rzeczywiste uruchamianie Claude Code / Codex przez nowy terminal.
- Retencja dziennika nie usuwa automatycznie nierozwiązanych wpisów; limit 4096 pozostaje bramką fail-closed.
