# AI Development Team — S4-B1 MERGED / bramki S4-B2
**20 listopada 2026 — po scaleniu PR #78**

## Zrealizowano

- S1: wspólny rejestr terminal/run — [PR #74](https://github.com/radekgosciniak/AI-Development-Team/pull/74), MERGED.
- S2: syntetyczny adapter ConPTY — [PR #75](https://github.com/radekgosciniak/AI-Development-Team/pull/75), MERGED.
- S3: filtrowane i heurystycznie redagowane ramki VT/Unicode — [PR #76](https://github.com/radekgosciniak/AI-Development-Team/pull/76), MERGED. Redakcja nie gwarantuje usunięcia wszystkich sekretów.
- S4-A: backend journal/recovery, owner i bezpieczeństwo instancji — [PR #77](https://github.com/radekgosciniak/AI-Development-Team/pull/77), MERGED `fe16a1507e94b3615d53ed9c5c0725ea8eb6a7df`.
- **S4-B1: kontrolowane Tauri IPC dla klienta syntetycznego** — [PR #78](https://github.com/radekgosciniak/AI-Development-Team/pull/78), MERGED `53623b9e8ad646651c4ea8bd26e9ff875ad75a70`. HEAD PR `eb21e7670264520189b2a6f68195fbcdd1e1e6c8`.

### Co wnosi S4-B1

- Jedna komenda `terminal_session` wyłącznie pod `tc4-synthetic`; zwykły build release jej nie zawiera.
- Natywny `Caller` z cyklu życia okna, `owner_lost`, limit ośmiu operacji przyjmowany **przed** kolejką `spawn_blocking`.
- `install_recovery` przed oknem, wyłączny właściciel lokalnego dziennika na NTFS, zabezpieczenie przed TOCTOU za pomocą relatywnych uchwytów `NtCreateFile` / `OBJ_DONT_REPARSE`, `journalFull` bez automatycznego usuwania unresolved.
- Syntetyczna binarka tylko o przypiętym SHA-256: `VERIFY → HOLD → CLAIM → LAUNCH`, ten sam `Held` przekazywany do TC-0. Zła/missing binarka: `syntheticUnavailable` i sesja pozostaje `CREATED`.
- Recovery w IPC jest diagnostyczne, bez PID, ścieżek, automatycznej zgody operatora, zabijania procesów czy przejmowania osieroconych sesji.

### Dowody i ograniczenia

Według raportu implementatora FIX-02: FAST 116 PASS, INTEGRATION 33 PASS x2, kompatybilność PASS, trusted_cli 125 PASS / 1 IGNORED z testowym PKI, clippy bez uwag w zmienionych plikach, build release bez flagi OK, FULL NOT RUN. Niezależny Codex: **READY FOR MERGE WITH S4-B2 GATES**, FAST 18 PASS, INTEGRATION 10 PASS; test błędu ACL powtórzony poza sandboxem, bazowy FAIL z logów autora, PASS niezależnie powtórzony. Natywny smoke aplikacji Tauri **NIE WYKONANY**.

## S4-B2 — trzy jawne bramki przed UI

1. **ACL/AppManifest** — osobna decyzja o ograniczeniu komend aplikacji (~30) w Tauri 2.11.6. Puste `permissions: []` nie ogranicza automatycznie lokalnych komend aplikacji bez manifestu; Codex sklasyfikował to jako przekrojowe ryzyko **C**. Sprawdzić zgodność i wpływ na dotychczasowy frontend, bez szerokiego niekontrolowanego refaktoru.
2. **Packaging** — zbudować i dostarczyć `cc-tc4-synthetic.exe` obok aplikacji oraz powiązać build-time SHA-256 z dokładnie dostarczonym plikiem. Brak prawidłowego pinu ma skutkować odmową `syntheticUnavailable`.
3. **Natywny smoke Tauri** — z rzeczywistym oknem Windows, wyłącznie `tc4-synthetic`, przetestować create/start/read/write/finish, lifecycle, zamknięcie okna, błędy recovery i brak komendy w standardowym release. Nie deklarować PASS na podstawie samego buildu ani testów bez okna.

Dopiero po zamknięciu tych bramek implementować S4-B2 React UI. Podczas planowania i implementacji obowiązkowo sprawdzić właściwe repozytoria i zapisane ustalenia z [EXTERNAL-REPOSITORIES-ADOPTION-MAP.md](EXTERNAL-REPOSITORIES-ADOPTION-MAP.md) (ADOPT / ADAPT / REJECT, bez kopiowania kodu).

## Niezmienione blokady

- **G-ENV OPEN** — brak jawnego oczyszczonego środowiska dla niesyntetycznych procesów.
- **G1 BLOCKED / D-3 Full Launch DEFERRED**.
- **D-FR-1/2/3 OPEN/DEFERRED**.
- Trust Broker pozostaje **Custody-Only**.
- Brak autoryzacji uruchamiania prawdziwych Claude/Codex.
- Pełny test **FULL NOT RUN**.
- Retencja journala nadal bez automatycznego kasowania niepewnych rekordów; limit 4096 może zatrzymać admission.

Historyczny plan [przed S4-B1](CURRENT-STATUS-2026-11-20-S4B.md) pozostaje jako zapis wcześniejszego stanu.
