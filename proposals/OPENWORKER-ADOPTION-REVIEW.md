# OPENWORKER-01 — opcjonalny audyt i adaptacja wzorców

**Data:** 2026-11-16  
**Źródło:** https://github.com/andrewyng/openworker  
**Decyzja:** **ADAPT / NO MIGRATION**  
**Status:** **BACKLOG / DESIGN REVIEW ONLY** — bez instalacji, uruchamiania agentów, zmian kodu lub autoryzacji implementacji.

## Cel
Porównać rozwiązania OpenWorker z istniejącym AI Development Team Command Center (Tauri 2 + React/TS, Claude Code i Codex CLI), aby wybrać wyłącznie konkretne, uzasadnione wzorce architektoniczne. Nie zastępować obecnego Command Center, warstwy wykonywania, modelu kont ani PILR.

## Kandydaci do oceny i przypisanie do etapów

| Kandydat | Powiązanie z roadmapą | Warunek adaptacji |
|---|---|---|
| Harness SDK: sesje, tool calls, streaming i lifecycle | E7 / Runs / Interactive Agent Chat | Porównanie API i zachowania z natywnymi CLI; nie obchodzić kontroli tożsamości i zgód |
| Approval policy, human-in-the-loop, historia decyzji | Consent per task / Trusted CLI / E4 | Zgodność z zasadą explicit approval, fail-closed i audytowalnym receipt |
| Izolacja Windows / sandbox i ochrona narzędzi | Trusted CLI / ACC-ID-01 / security gates | Niezależny threat model, kontrola ACL/tokenów/TOCTOU i testy na izolowanych fixtures |
| Harmonogram, kolejki i unattended tasks | P7 / Scheduler / PILR | Limity zasobów, stop/cancel, autoryzacja per task i brak automatycznych zmian roadmapy |
| UX sesji i transkrypcji | E7 / UX / Interactive Agent Chat | Adaptacja wzorców UI bez migracji do innego frameworka |

## Bramki audytu OW-01…OW-06
1. **OW-01** — przypiąć konkretny upstream commit, zweryfikować licencję, zależności i provenance.
2. **OW-02** — przeanalizować rzeczywistą implementację Harness SDK i integrację z Claude/Codex (nie polegać tylko na README).
3. **OW-03** — porównać approval/consent, logowanie i zachowanie przy odmowie, timeoutach oraz restartach.
4. **OW-04** — ocenić sandbox Windows i model zagrożeń; bez uruchamiania na realnych kontach/profilach.
5. **OW-05** — porównać harmonogram, sesje, streaming i UX z E7, P7 oraz PILR.
6. **OW-06** — sporządzić macierz ADOPT / ADAPT / REJECT z dowodami, kosztami i ryzykiem; każda implementacja wymaga osobnej zgody.

## Zasady wykonawcze
- Najpierw bieżąca stabilizacja i zatwierdzone quality gates. Ten wpis **nie zmienia priorytetów**.
- Zachować rozdzielenie discovery/execution, izolację profili, Trusted CLI, consent per task i niezależne review.
- Nie zakładać, że funkcje deklarowane przez upstream są już zweryfikowane w kodzie.
- Raporty z ewentualnego pilotażu w `C:\\AI-Development-Team\\reports`.
- **Brak zgody na instalację, uruchomienie, migrację i automatyczną adaptację.**
