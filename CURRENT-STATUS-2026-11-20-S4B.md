# AI Development Team — checkpoint po S4-A, przed S4-B
**20 listopada 2026 — PR #77 MERGED**

## Stan faktyczny

Aplikacja: [AI-Development-Team](https://github.com/radekgosciniak/AI-Development-Team).

- **S1**: wspólny rejestr sesji, PR #74 MERGED.
- **S2**: syntetyczny adapter ConPTY, PR #75 MERGED.
- **S3**: streaming VT/Unicode + heurystyczna redakcja, PR #76 MERGED. Nie daje gwarancji wykrycia dowolnego sekretu.
- **S4-A**: twardnienie backendu, [PR #77 MERGED](https://github.com/radekgosciniak/AI-Development-Team/pull/77), merge commit `fe16a1507e94b3615d53ed9c5c0725ea8eb6a7df`. Naprawy MINOR-1 (EOF vs write_input) i MINOR-2 (panika właściciela), dziennik metadanych CC-TERMINAL-JOURNAL/1, restart sweep, kontrola spójności historii i wykrywanie duplikatów kluczy JSON przed deserializacją, natywna wyłączna własność `owner.lock` na Windows, jeden journaling/recovery gate; dostęp do sesji według natywnego `Caller`.

**Dowody:** według raportu implementatora S4-A FIX-03: 97 FAST PASS, 23 INTEGRATION PASS x2, regresja trusted_cli 125 PASS / 1 IGNORED, clippy bez uwag w zmienionych plikach, build release OK. Niezależny Codex potwierdził **READY FOR MERGE**, 21 FAST PASS + 1 test między procesami PASS. Test bazowego FAIL autor potwierdził wcześniej, Codex powtórzył stronę PASS. **FULL NOT RUN.**

To jest backend, nie nowy ekran terminala. Nowy IPC ani rzeczywiste uruchamianie Claude/Codex **nie zostały odblokowane**.

## Co daje S4-A?

- Zapis po zamknięciu wejścia nie dociera do syntetycznego klienta i zwraca `inputClosed`.
- Nieoczekiwana panika nie powoduje fałszywego FINISHED ani zwolnienia niepotwierdzonych zasobów.
- Po restarcie niepewność procesu oznacza `Unverifiable` albo `Unresolved`, nie automatyczny kill lub cleanup.
- Dziennik nie uznaje sprzecznych, uszkodzonych albo niejednoznacznych rekordów za potwierdzony koniec.
- Druga instancja nie może równocześnie przejąć tego samego lokalnego dziennika; po awarii Windows zwalnia blokadę, ale nierozwiązane sesje pozostają.

## NEXT: TC-4-SESSION S4-B — kontrolowany IPC (synthetic only)

Kryteria otwarcia IPC:

1. **Recovery przed obsługą pierwszego create** — `install_recovery` przy starcie aplikacji, bez możliwości obejścia `create_recovered` ani `start_recorded`.
2. **Caller na całe życie natywnego okna** — nie z danych requestu, nie tworzony od nowa przy każdej komendzie; obsługa `owner_lost` z zachowaniem unresolved.
3. **Operator i procesy niepewne** — `Unresolved`, `OrphanSuspected`, `Unverifiable` utrzymują blokady; żadnego działania na procesie bez natywnej zgody i ponownej weryfikacji tożsamości. Nie wolno automatycznie zabijać osieroconych procesów.
4. **Journal/instance** — lokalny katalog danych (nie niezweryfikowane SMB), fail-closed przy `journalOwned` i `journalLockUnavailable`; operator widzi status. Nie zastępować systemowego uchwytu zegarem ani PID.
5. **Retencja** — limit 4096 plików może bezpiecznie zablokować create; potrzebna weryfikowalna, niekasująca informacji o unresolved strategia rotacji/archiwizacji przed pełną produkcyjną ekspozycją.
6. **Kontrolowany kontrakt IPC** — walidacja owner, cursor, generation, timeoutów, rozmiarów wejścia i limitów; żadnego niezaufanego rozszerzenia uprawnień okna.
7. **Testy** — dwa procesy, restart, uszkodzone rekordy, szybkie zamknięcie okna, wyścigi, testy zgodności S1–S4-A, brak rzeczywistych CLI.

Przed implementacją sprawdzić odpowiednie repozytoria w [EXTERNAL-REPOSITORIES-ADOPTION-MAP](EXTERNAL-REPOSITORIES-ADOPTION-MAP.md) i konkretne mechanizmy OpenWorker, Orca, AionCore, Open Duet tylko gdy potwierdzone źródło. Decyzje ADOPT / ADAPT / REJECT, bez kopiowania obcego kodu.

## Nadal zablokowane

| Zagadnienie | Status |
| --- | --- |
| G-ENV — jawne, oczyszczone środowisko TC-0 | **OPEN / BLOCKER** przed realnymi CLI |
| G1, D-3 Full Launch | **BLOCKED / DEFERRED** |
| D-FR-1/2/3, w tym scenariusze procesów potomnych | **OPEN / DEFERRED** |
| Trust Broker | **Custody-Only**, bez uruchamiania rzeczywistych agentów |
| Redakcja sekretów | Heurystyczna, z opisanymi ograniczeniami |
| FULL | **NOT RUN** |

Wcześniejszy [checkpoint S4-A](CURRENT-STATUS-2026-11-20-S4A.md) pozostaje historycznym zapisem stanu sprzed merge #77. Status PR i hash scalenia zweryfikowano na GitHub; wyniki testów pochodzą z raportów wykonawców i niezależnego odbioru Codex.
