# AI Development Team — Trusted CLI / CUSTODY-01 / CC-TB-01
**Checkpoint: 19 listopada 2026**  
**Status źródeł:** PR #69 zweryfikowany w GitHub; stan lokalnej usługi, worktree i testów CC-TB-01 pochodzi z raportu operatora/Claude Code, nie z bezpośredniego dostępu do hosta.

## Stan Git i granice akceptacji

| Etap | Stan na 19.11.2026 | Dowód / uwaga |
| --- | --- | --- |
| Trust Broker Custody-Only MVP, host custody PoC | **MERGED do main** | [AI Development Team PR #69](https://github.com/radekgosciniak/AI-Development-Team/pull/69), merge `d1571c75b5f5126d6019689f4efe9b3513406c67`; commit źródłowy `bfce0d33f7939a42928292269b3e1fcb08928081`; 33 pliki |
| Broker S5 v3 — natywna usługa Windows | **Funkcjonalny test natywny PASS** (raport lokalny) | `CCTrustBrokerS5Test`, `NT SERVICE\CCTrustBrokerS5Test`, restricted service SID, sesja 0, SCM PID = pipe PID `13744` podczas testu; bez dodatkowych LSA grants |
| IPC authenticate i ograniczenie Launch | **PASS w scenariuszu klienta operatora** (raport lokalny) | `request_run -> Ok("refused:custodyOnly:launchDisabled")`; wcześniejszy `OpenProcess error 5` rozwiązany w S5 v3 |
| CC-TB-01 — status w Tauri/React | **IMPLEMENTED LOCALLY; UNCOMMITTED** | Branch `feat/cc-tb-01`, 11 plików tekstowych; Rust 16/16, UI 161/161, tsc/build PASS; natywny odczyt `Running — capability not verified`. Brak potwierdzonego GUI desktopowego, commita, PR lub merge |
| `broker_info` read-only | **IMPLEMENTATION AUTHORIZED / deployment not authorized** | Wymagane rzeczywiste capability z brokera; odpowiedź niesekretna, bez zmiany journal, replay window, vault, registry i admission; nie używać `request_run` jako sondy |
| S5 v4 | **PROPOSED; NOT DEPLOYED** | Wymaga osobnej zgody i testów przed zatrzymaniem/wymianą działającej S5 v3 |
| TC-0/1/2 | **IMPLEMENTED LOCALLY / NOT COMMITTED** | `trusted_cli/*.rs`, `lib.rs`, 24 binarne fixtures (10 tar, 11 gz, 3 bin) lokalnie; wymagają rozwiązania weryfikowalnego checkpointu |
| Git hook fixtures policy | **BLOCKED — MAJOR-2** | Proponowana modyfikacja instalacji współdzielonych hooków nie przeszła niezależnego review Codex (FIX-02); brak instalacji nowego hooka |
| G1 i Full Launch / TC-3 | **G1 BLOCKED / D-3 DEFERRED / TC-3 NOT DONE** | Brak ogólnego odbioru Trusted CLI i brak autoryzacji rzeczywistych uruchomień CLI |

## Dowody testowe (według raportów lokalnych)

- Trust Broker S5 v3: FAST **44/44**; INTEGRATION **49/49**; release build PASS. Brak FULL regresji.
- Natywny test S5 v3: odrębna tożsamość usługi, ograniczone przywileje `SeImpersonatePrivilege` i `SeChangeNotifyPrivilege`; właściciel named pipe = service SID. Zakaz Launch potwierdzony dla jednego autoryzowanego klienta. Nie stanowi pełnego dowodu odporności na atak.
- CC-TB-01: Rust **16/16** oraz UI **161/161**, build TS/React PASS; operator raportuje natywny smoke do usługi, nie potwierdza jeszcze wizualnego testu okna Tauri.
- Przechowywane lokalnie raporty: `C:\AI-Development-Team\reports\TRUSTED-CLI\TRUSTED-CLI-FIRST-PUSH-PR.md`, `CC-TB-01-IMPLEMENTATION.md`, `CUSTODY-01-S5-IPC-AUTH-REDEPLOY-V3.md`. Nie publikować ich automatycznie ani nie traktować jako dowodu po stronie GitHub.

## Zależności i następne kroki

1. **CC-TB-01 GUI + Git:** wykonać wizualny test w aplikacji; zachować sprawdzanie SCM PID / pipe PID / pipe owner; commit 11 tekstowych plików i Draft PR, ale nie oznaczać jako MERGED do potwierdzenia.
2. **`broker_info`:** zaimplementować minimalną, tylko do odczytu operację IPC, bez uruchamiania zadań i bez rozszerzania DACL usługi; przetestować bezpieczeństwo i kompatybilność. S5 v4 wymaga osobnego zatwierdzenia wdrożenia.
3. **Custody security evidence:** testy odporności na manipulację protected registry, vault i identity przez proces operatora; testy restart/crash, negatywne IPC, bez nieautoryzowanego uruchamiania agentów.
4. **TC-0/1/2 checkpoint:** nie blokować niezależnych commitów przez nieukończony instalator hooka; zachować integralność 24 fixtures, normalne hooki i review wymaganych zmian.
5. **G1 / D-3 / TC-3:** pozostać zablokowane lub odroczone do osobnych decyzji, w tym architektury pełnego Launch i ConPTY.

## Reguła aktualizacji Living Roadmap

Po istotnym PR odnotować osobno: `IMPLEMENTED LOCALLY` → `COMMITTED` → `PR OPEN/DRAFT` → `MERGED` oraz `TESTED` → `ACCEPTED`. Nie przenosić statusów automatycznie pomiędzy tymi osiami. Odwoływać się do trwałego PR/SHA lub raportu z oznaczonym źródłem. Roadmap Mode steruje pracą zgodnie z [PILR](proposals/PILR-project-intelligence-living-roadmap.md); Direct Task nadal nie wymaga roadmapy.

**Ten checkpoint aktualizuje stan z [14.11](CURRENT-STATUS-2026-11-14.md), nie przepisuje historycznych migawek.**
