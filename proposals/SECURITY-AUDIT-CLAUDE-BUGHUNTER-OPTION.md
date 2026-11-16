# Claude-BugHunter — opcjonalne narzędzie Security Audit

- Data wpisu: 2026-11-16
- Źródło: https://github.com/elementalsouls/Claude-BugHunter
- Decyzja: **OPTIONAL / EVALUATE / ADAPT — NO MIGRATION**
- Status: **BACKLOG / NOT STARTED**. Brak zgody na instalację, uruchamianie skanerów, testy aktywne lub implementację.

## Cel
Rozważyć wykorzystanie wybranych umiejętności i metodologii Claude-BugHunter jako **opcjonalnego**, niezależnego etapu audytu bezpieczeństwa kodu tworzonego lub zmienianego przez agentów Claude Code / Codex. Nie zastępuje testów funkcjonalnych, review Codex/Claude, istniejących quality gates ani modelu zgód Command Center.

## Proponowane zastosowania
- Laravel/API: autoryzacja i izolacja tenantów (`company_id`), IDOR, SQL injection, uprawnienia.
- WordPress/WooCommerce: własne wtyczki, AJAX/REST, nonce, capability checks, XSS/CSRF.
- AI Development Team: audyty zmian dotyczących kont, tożsamości, wykonywania CLI, sandboxingu, uprawnień i integracji.

## Warunki przed ewentualnym użyciem
1. Zweryfikować upstream commit, licencję, zależności, skrypty instalacyjne i zakres dostępu.
2. Najpierw pilotaż pasywny, na kontrolowanym własnym repozytorium/fixture; bez skanowania produkcji i bez wysyłania sekretów.
3. Aktywne testy wyłącznie na jawnie autoryzowanym środowisku, w uzgodnionym zakresie.
4. Wyniki jako hipotezy do potwierdzenia: ścieżka dowodowa, severity, false-positive review i niezależna weryfikacja.
5. Zachować rozdział discovery/execution, consent per task, obecne blokery i niezależne bramki odbioru.
6. Raporty i artefakty lokalnego AI Development Team do `C:\\AI-Development-Team\\reports`.

## Sugerowane miejsce w workflow (opcjonalne)
Implementacja → testy funkcjonalne/kontraktowe → **Security Audit (opcjonalnie)** → niezależne review → decyzja PASS / CHANGES REQUIRED / BLOCKED.

## Decyzja wykonawcza
Wyłącznie wpis do katalogu narzędzi możliwych do użycia. Bez zmiany kolejności obecnych prac, bez automatycznego uruchamiania i bez zobowiązania do adopcji.
