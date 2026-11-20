# AI Development Team — checkpoint i wyjaśnienie postępu
**20 listopada 2026 · po merge PR #74**

> **Krótko:** stworzyliśmy bezpieczne fundamenty pod uruchamianie i obserwowanie agentów w Command Center. **Jeszcze nie uruchamiamy prawdziwych Claude Code / Codex przez nowy mechanizm.** Następny krok to połączenie testowego terminala z istniejącym systemem sesji (TC-4-SESSION S2).

## Po co to robimy — po ludzku?

Chcemy, żeby Command Center pozwalał zlecić zadanie agentowi, pokazywał przebieg jego pracy, umożliwiał anulowanie i zapisywał wiarygodny wynik. Samo uruchomienie programu z przycisku jest łatwe; trudniejsze jest pilnowanie **co dokładnie wolno mu uruchomić, czy można mu ufać, co zrobił i czy naprawdę zakończył pracę**.

Budowaliśmy więc elementy od dołu, zamiast od razu uruchamiać prawdziwe agenty:
1. **Osobna kontrola (Trust Broker).** Usługa Windows S5 v4 ma ograniczone uprawnienia i raportuje do interfejsu stan `Running — Custody-Only`. To strażnik/test fundamentu bezpieczeństwa, **nie** włączony mechanizm Full Launch. Odczyt statusu nie zmienia jego journalu.
2. **Sprawdzenie wykonania i wejścia (TC-0/1/2).** Kod sprawdza integralność i pochodzenie tego, co ma być uruchomione, kontroluje proces i interpretuje wyniki. 24 trudne przykłady danych testowych (fixtures) zapisano tekstowo jako base64 z kontrolą SHA-256; nie musieliśmy zmieniać wspólnych hooków Git.
3. **Terminal (TC-3-PREP).** ConPTY to windowsowy mechanizm interaktywnego terminala: wejście, wyjście, zakończenie i kontrola limitów. Sprawdziliśmy go **tylko z syntetycznym klientem testowym**, bez uruchamiania Claude/Codex. Naprawiono dwa niestabilne testy. Przy sprzątaniu hosta konsoli część ryzyk pozostaje jawnie odroczona.
4. **Jedna lista sesji w Command Center (TC-4-SESSION S1).** Zamiast tworzyć drugi system wykonywania, rozdzieliliśmy istniejący rejestr `execution_run` na wspólny `session_registry` i dodaliśmy kontrakt `CC-TERMINAL-SESSION/1`. Dotychczasowy `CC-EXECUTION-RUN/1` ma pozostać zgodny. **To jeszcze definicje i stany, nie podłączony terminal.**

**Przykład przyszłego działania (jeszcze niewdrożony):** użytkownik uruchamia zadanie w Command Center → system weryfikuje dopuszczony program i rezerwuje sesję → terminal przekazuje wyjście w kontrolowany sposób → UI pokazuje postęp → anulowanie zatrzymuje proces i zapisuje wynik. Dziś mamy podwaliny; połączenia i zgoda na prawdziwych agentów to kolejne etapy.

## Co faktycznie jest w Git

| Element | Stan | Dowód |
| --- | --- | --- |
| Trust Broker MVP / Custody-Only | **MERGED**, usługa S5 v4 **DEPLOYED / FUNCTIONAL PASS** (dowody operatora) | [PR #69](https://github.com/radekgosciniak/AI-Development-Team/pull/69), merge `d1571c7` |
| Status Trust Brokera w UI | **MERGED**, GUI PASS (raport operatora) | [PR #70](https://github.com/radekgosciniak/AI-Development-Team/pull/70), `fd95b1d4` |
| `broker_info` bez skutków ubocznych | **MERGED**, testy natywne PASS (raport operatora) | [PR #71](https://github.com/radekgosciniak/AI-Development-Team/pull/71), `f8571d3` |
| TC-0/1/2 + 24 fixtures base64 | **MERGED** | [PR #72](https://github.com/radekgosciniak/AI-Development-Team/pull/72), `5a44966` |
| TC-3-PREP ConPTY i hardening | **MERGED**, host-test PASS (raport operatora: 125 PASS / 0 FAIL / 1 IGNORED, bez FULL) | [PR #73](https://github.com/radekgosciniak/AI-Development-Team/pull/73), `7dc797e` |
| TC-4-SESSION S1: kontrakt, stany i jeden rejestr | **MERGED**, S1 funkcjonalnie zaakceptowany | [PR #74](https://github.com/radekgosciniak/AI-Development-Team/pull/74), merge `71c449be45517c9ff2ab19e3542b38ac5f61f364` |
| Lokalny `main` i `origin/main` aplikacji | **SYNCHRONIZED** przez operatora na `71c449be...` | Wynik `git fetch origin main:main` i obu `rev-parse` |

**Uwaga o testach:** dla S1 nowe FAST 27/27 powtórzone trzy razy; powiązane INTEGRATION zostały wykonane przy implementacji. Końcowy przegląd nie wykonał ponownie pełnej regresji. **FULL = NOT RUN** — nie oznaczamy jej jako PASS. Niezmienione problemy bazowe (m.in. provider_accounts::recovery i clippy) nie zostały naprawione przez ten PR.

## Co robimy teraz

**Najbliższy checkpoint: TC-4-SESSION S2 — adapter sesji testowego terminala.** Nie tworzymy nowego launchera. Wykorzystujemy S1 i TC-3-PREP, zachowujemy dotychczasowy system runów i historyczne IPC.

S2 powinien sprawdzić:
- poprawną kolejność `reserve → start → finish/cancel`, zwłaszcza gdy anulowanie i zakończenie zdarzają się jednocześnie;
- zgodność kursora i limitów wyjścia z ConPTY, bez utraty lub powtórzeń danych;
- atomowość rejestru, budżety 16 runów / 4 terminali / 1 aktywnego terminala oraz brak wywołań zwrotnych rekurencyjnie blokujących ten rejestr;
- timeouty, cleanup i nierozwiązane pseudokonsole z wyraźnym raportowaniem, bez fałszywego statusu „clean”;
- testy z syntetycznym klientem; bez rzeczywistych CLI i bez podłączania produkcyjnego Full Launch.

**Dalsze etapy:** bezpieczne ramkowanie/oczyszczanie tekstu terminala i kontrolowana prezentacja w UI, dyspozytor operacji z kontrolą właściciela, a na końcu osobny proces decyzji o uruchamianiu realnych agentów. Zachowujemy zasadę, że roadmapa steruje pracą projektową, ale bezpośrednie pojedyncze zadania mogą działać jako Direct Task.

## Co nadal jest zablokowane albo otwarte

- **G1 = BLOCKED**, **D-3 Full Launch = DEFERRED**, **G3 / TC-4-RUN (TrustedSource + Claude) nieodblokowane**. Nazwa TC-4-SESSION dotyczy nowych sesji testowych i nie zastępuje TC-4-RUN z DESIGN-04.
- Trust Broker S5 v4 działa tylko w trybie `--custody-only`; nie podmieniamy usługi, nie uruchamiamy prawdziwych Claude/Codex. Staging V3 i V4 pozostają zachowane.
- **D-FR-1**: osobny supervisor conhost odroczony; **D-FR-2**: podpis katalogowy conhost odroczony; **D-FR-3**: możliwość wcześniejszego zwolnienia holdów w TC-0, P3 OPEN — nie rozszerzamy w ciemno scenariuszy potomnych.
- **Brak sekretów przez stdin** i brak surowych sekwencji VT w rendererze; filtr bezpieczeństwa musi obsługiwać sekwencje podzielone między fragmenty. UI nie dostaje uprawnień do dowolnego startowania procesów.
- **Instalator hooków MAJOR-2** jest osobnym, wstrzymanym tematem. Nie resetować ani nie czyścić starych worktree z niezacommitowanymi zmianami.
- Z S1 do S2 pozostają ryzyka dotyczące callbacków pod blokadą, kolejności start/finish oraz spójności offsetu; z przyszłych etapów S3/S4: filtrowanie znaków formatowania i kontrola właściciela przy każdej operacji.

## Skąd pochodzą te informacje

Statusy merge oraz commity PR wynikają z GitHub. Wyniki testów, szczegóły lokalnej usługi Windows i wyrównanie lokalnego main pochodzą z raportów Claude Code i logów operatora. Nie ma w tej aktualizacji nowego uruchomienia testów ani dostępu do lokalnego hosta.

Ten dokument jest **nowym checkpointem z 20.11.2026**. Poprzedni [checkpoint z 19.11](CURRENT-STATUS-2026-11-19.md) pozostaje historyczny i nie jest przepisany.
