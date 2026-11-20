# AI Development Team — checkpoint TC-4-SESSION S4-A
**20 listopada 2026 · po scaleniu PR #76**

## Po ludzku: gdzie jesteśmy?

Budujemy Command Center, w którym można będzie uruchamiać, obserwować i zatrzymywać sesje agentów w kontrolowany sposób. Nie zaczynamy od uruchamiania rzeczywistych Claude/Codex, bo bezpieczeństwo i diagnostyka muszą działać wcześniej.

- **S1 — jeden rejestr sesji:** aplikacja ma wspólny mechanizm zarządzania dotychczasowymi runami i nowymi sesjami terminalowymi, zamiast dwóch konkurujących systemów. [PR #74](https://github.com/radekgosciniak/AI-Development-Team/pull/74), MERGED `71c449b`.
- **S2 — kontrolowany terminal:** połączono ConPTY z rejestrem S1 i dotychczasowym launcherem TC-0. Sprawdzane są uruchamianie, anulowanie, limity i sytuacje, gdy proces nie został wiarygodnie zamknięty. [PR #75](https://github.com/radekgosciniak/AI-Development-Team/pull/75), MERGED `6766ff592215e4d379b467ef457b06f9090e1960`. Raport: 36 FAST, 13 INTEGRATION PASS.
- **S3 — bezpieczna treść terminala:** dodano filtr VT/Unicode i heurystyczne maskowanie potencjalnych sekretów przed wydaniem kontrolowanych ramek. Wzorce z [Orca](https://github.com/stablyai/orca), [OpenWorker](https://github.com/andrewyng/openworker) i [Claude-BugHunter](https://github.com/elementalsouls/Claude-BugHunter) **zaadaptowano jako idee, bez kopiowania kodu i instalowania tych narzędzi**. [PR #76](https://github.com/radekgosciniak/AI-Development-Team/pull/76), MERGED `04f050746bf084def3ab5b1c5b1466b8b237aeee`. Raport końcowy: 71 FAST i 17 INTEGRATION PASS, FULL NOT RUN. Pierwotna reprodukcja FAIL→PASS błędu FIX-04 nie została uzyskana; zamiast niej zastosowano analizę ścieżek emisji i testy niezmienników. Odbiór nie był niezależny od implementatora.
- **Lokalny main** i `origin/main` aplikacji potwierdzone przez operatora na `04f0507`.

Praktycznie: Command Center ma już fundament do obsługi testowego terminala i odfiltrowanego odczytu. **To nadal nie oznacza, że nowy terminal jest dostępny w UI albo wolno uruchamiać rzeczywiste konta agentów.**

## Następny checkpoint — S4-A

**S4-A: bezpieczeństwo i niezawodność przed otwarciem IPC.** Nowy izolowany worktree od `04f0507`; Claude implementuje, a odbiór koncentruje się na konkretnych regresjach:

1. **MINOR-1:** wyeliminować sytuację, w której wejście zaakceptowane w RUNNING dociera do klienta po konsolowym EOF, jeśli równolegle nastąpi finish. Bez drugiej kolejki, z jednoznaczną kolejnością i testami wyścigów.
2. **MINOR-2:** nie zostawiać sesji w STARTING bez przyczyny, gdy wątek właściciela spanikuje przed pętlą. Nie wolno błędnie raportować sukcesu ani potwierdzać cleanup bez dowodów.
3. **Własność:** powiązanie operacji ze zweryfikowanym wywołującym; nigdy nie ufać właścicielowi przekazanemu z niezaufanego frontendu.
4. **Restart:** tylko zgodnie z zakresem TC-4-DESIGN, metadane bez wejścia/wyjścia/sekretów, brak automatycznego wznowienia. PID nie jest wystarczającą tożsamością; niepewne procesy i cleanup fail-closed. Żadnego automatycznego zabijania po samym PID.
5. **Zachować:** jeden rejestr, jeden launcher TC-0, 16 runów, 4 terminale, 1 aktywny terminal, limity ramek, buforów i sanitizację.

Po FAST/INTEGRATION i odbiorze S4-A osobna decyzja o **S4-B**, czyli kontrolowanym IPC wyłącznie dla syntetycznych sesji. S4-B i UI nie zostały rozpoczęte.

## Otwarte bramki i ryzyka

| Element | Stan |
| --- | --- |
| G-ENV | **OPEN/BLOCKER** przed każdym niesyntetycznym Launch: TC-0 używa `CreateProcessW` z env=NULL i dziedziczy środowisko rodzica |
| G1 | **BLOCKED** |
| D-3 Full Launch | **DEFERRED** |
| D-FR-1/2/3 | Otwarte/odroczone; scenariusze child wyłączone |
| MINOR-1 / MINOR-2 | **OPEN**, zadania S4-A |
| F-2 i inne ograniczenia redakcji | Udokumentowane; heurystyka nie daje pełnej gwarancji sekretów |
| FULL | **NOT RUN**, nie deklarujemy PASS |
| Trust Broker | S5 v4 **Custody-Only**, bez zgody na prawdziwych agentów |

Niniejszy checkpoint aktualizuje [stan z wcześniejszej części 20.11.2026](CURRENT-STATUS-2026-11-20.md), który zostaje jako zapis historyczny. Źródłem statusów Git są PR w GitHub; wyniki testów i szczegóły runtime bazują na raportach wykonawców oraz przekazanych logach operatora.
