# AI Development Team — Trusted CLI / CUSTODY-01 / CC-TB-01
**Checkpoint: 19 listopada 2026**  
**Status źródeł:** PR #69, #70 i #71 zweryfikowane w GitHub jako MERGED. Wdrożenie S5 v4 i testy natywne/GUI według raportu operatora i Claude Code; brak bezpośredniego dostępu do hosta.

## Stan Git i granice akceptacji

| Etap | Stan na 19.11.2026 | Dowód / uwaga |
| --- | --- | --- |
| Trust Broker Custody-Only MVP, host custody PoC | **MERGED do main** | [AI Development Team PR #69](https://github.com/radekgosciniak/AI-Development-Team/pull/69), merge `d1571c75b5f5126d6019689f4efe9b3513406c67`; commit źródłowy `bfce0d33f7939a42928292269b3e1fcb08928081`; 33 pliki |
| Broker S5 v3 — natywna usługa Windows | **Funkcjonalny test natywny PASS** (raport lokalny) | `CCTrustBrokerS5Test`, `NT SERVICE\CCTrustBrokerS5Test`, restricted service SID, sesja 0, SCM PID = pipe PID `13744` podczas testu; bez dodatkowych LSA grants |
| IPC authenticate i ograniczenie Launch | **PASS w scenariuszu klienta operatora** (raport lokalny) | `request_run -> Ok("refused:custodyOnly:launchDisabled")`; wcześniejszy `OpenProcess error 5` rozwiązany w S5 v3 |
| CC-TB-01 — status w Tauri/React | **MERGED / GUI PASS** | [PR #70](https://github.com/radekgosciniak/AI-Development-Team/pull/70), merge `fd95b1d4fbcb0b0d23fce50bee3e4355d831f840`; Rust 16/16, UI 161/161, produkcyjny frontend i natywne GUI PASS; status `Running — Custody-Only` z v4 |
| `broker_info` read-only | **MERGED / HOST + NATIVE PASS** | [PR #71](https://github.com/radekgosciniak/AI-Development-Team/pull/71), merge `f8571d34d8bb6e96deb9855f9d79b0edf7b12d00`; FAST 46/46, INTEGRATION 53/53; S5-5 T0–T7 PASS; bez zmian custody po odczytach |
| S5 v4 | **DEPLOYED / FUNCTIONAL PASS** | CP-1–CP-6, preflight coexist/install, native S5-5, smoke i GUI PASS; nowy PID 9508 podczas odbioru, przerwa poniżej 20 s; oba stagingi zachowane, rollback nie wykonany |
| TC-0/1/2 | **IMPLEMENTED LOCALLY / NOT COMMITTED** | `trusted_cli/*.rs`, `lib.rs`, 24 binarne fixtures (10 tar, 11 gz, 3 bin) lokalnie; wymagają rozwiązania weryfikowalnego checkpointu |
| Git hook fixtures policy | **BLOCKED — MAJOR-2** | Proponowana modyfikacja instalacji współdzielonych hooków nie przeszła niezależnego review Codex (FIX-02); brak instalacji nowego hooka |
| G1 i Full Launch / TC-3 | **G1 BLOCKED / D-3 DEFERRED / TC-3 NOT DONE** | Brak ogólnego odbioru Trusted CLI i brak autoryzacji rzeczywistych uruchomień CLI |

## Dowody testowe (według raportów lokalnych)

- Trust Broker S5 v3: FAST **44/44**; INTEGRATION **49/49**; release build PASS. Brak FULL regresji.
- Natywny test S5 v3: odrębna tożsamość usługi, ograniczone przywileje `SeImpersonatePrivilege` i `SeChangeNotifyPrivilege`; właściciel named pipe = service SID. Zakaz Launch potwierdzony dla jednego autoryzowanego klienta. Nie stanowi pełnego dowodu odporności na atak.
- CC-TB-01: Rust **16/16** oraz UI **161/161**, build TS/React PASS; natywny smoke i test okna Tauri przez Windows UI Automation **PASS** (operator). S5 v4: 77+ odczytów statusu bez skutków ubocznych; sesja C potwierdziła niezmieniony journal i konfigurację custody-only.
- Końcowy raport V4: `C:\AI-Development-Team\reports\TRUSTED-CLI\S5-V4-DEPLOYMENT-RESULT.md` (według operatora, SHA pliku zaczyna się od `614478a5`). 
- Przechowywane lokalnie raporty: `C:\AI-Development-Team\reports\TRUSTED-CLI\TRUSTED-CLI-FIRST-PUSH-PR.md`, `CC-TB-01-IMPLEMENTATION.md`, `CUSTODY-01-S5-IPC-AUTH-REDEPLOY-V3.md`. Nie publikować ich automatycznie ani nie traktować jako dowodu po stronie GitHub.

## Zależności i następne kroki

1. **CC-TB-01 GUI + Git:** ZAKOŃCZONE: PR #70 MERGED, GUI i kliknięcie odświeżenia PASS; sprawdzanie SCM PID / pipe PID / pipe owner zachowane.
2. **`broker_info` i S5 v4:** ZAKOŃCZONE funkcjonalnie: PR #71 MERGED, wdrożone V4, S5-5/natywny smoke/GUI PASS. Sesja C (22:06:04): SHA-256 journal `5be9a750737713fae371c49001703818c91e1f38592945599825174e3f073ba2`, 373 B, 1 rekord `brokerStarted`, katalog runs bez przebiegów, `tb-stub` nieuruchomiony. Po ponad 77 zapytaniach statusu stan custody pozostał identyczny.
3. **Custody security evidence:** testy odporności na manipulację protected registry, vault i identity przez proces operatora; testy restart/crash, negatywne IPC, bez nieautoryzowanego uruchamiania agentów.
4. **TC-0/1/2 checkpoint:** nie blokować niezależnych commitów przez nieukończony instalator hooka; zachować integralność 24 fixtures, normalne hooki i review wymaganych zmian.
5. **G1 / D-3 / TC-3:** pozostać zablokowane lub odroczone do osobnych decyzji, w tym architektury pełnego Launch i ConPTY.

## Reguła aktualizacji Living Roadmap

Po istotnym PR odnotować osobno: `IMPLEMENTED LOCALLY` → `COMMITTED` → `PR OPEN/DRAFT` → `MERGED` oraz `TESTED` → `ACCEPTED`. Nie przenosić statusów automatycznie pomiędzy tymi osiami. Odwoływać się do trwałego PR/SHA lub raportu z oznaczonym źródłem. Roadmap Mode steruje pracą zgodnie z [PILR](proposals/PILR-project-intelligence-living-roadmap.md); Direct Task nadal nie wymaga roadmapy.

**Ten checkpoint aktualizuje stan z [14.11](CURRENT-STATUS-2026-11-14.md), nie przepisuje historycznych migawek.**

## Aktualizacja po wdrożeniu S5 v4 — 19.11.2026

Wdrożenie i testy funkcjonalne S5 v4 zakończone; nie jest to zatwierdzenie G1 ani Full Launch. Działająca usługa testowa zachowuje `--custody-only`, restricted service SID oraz wyłącznie SeImpersonate / SeChangeNotify. Staging V3 pozostaje do rollbacku i nie wolno go usuwać bez osobnej decyzji. **Aktualny priorytet: TC-0/1/2 checkpoint + plan następnej integracji**; instalator hooków MAJOR-2 pozostaje osobnym problemem. 
