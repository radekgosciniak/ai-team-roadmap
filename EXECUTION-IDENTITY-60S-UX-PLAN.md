# EXECUTION-IDENTITY-60S-UX-01 — sesyjna wygoda, świeża weryfikacja przed startem

**Stan:** BACKLOG / decyzja koncepcyjna zatwierdzona do planowania, niezaimplementowane (2026-11-14).  
**Powiązania:** UX-EXECUTION-SIMPLIFICATION-01, Execution E4/E6/E7, problem E-1 (zmiana binarki CLI).

## Cel

Obecnie po autoryzacji trzeba przejść do AI Provider Accounts, ręcznie wykonać Check identity, wrócić do Execution i uruchomić zadanie w ciągu 60 sekund. Uprościć to bez osłabienia bramek E4.

## Proponowany UX — raz ręcznie na sesję, odświeżenie automatyczne przed startem

1. Po otwarciu aplikacji użytkownik ręcznie weryfikuje konto przy pierwszym użyciu. UI może pokazywać **„konto zweryfikowane w tej sesji”** (stan informacyjny), dopóki nie zmieni się aktywne konto, profil, wersja/SHA CLI, EXPECTED albo generacja procesu. Nie nazywać tego świeżym VERIFIED dla nowych uruchomień.
2. Gdy użytkownik świadomie inicjuje kolejne uruchomienie, aplikacja **automatycznie wykonuje identity probe dla przypisanego managed profile** w ramach przepływu startu, zamiast wymagać ponownego kliknięcia Check identity i ręcznej nawigacji do kont.
3. Bezpośrednio po probe istniejący preflight/admission musi nadal egzekwować **VERIFIED <=60 s i tę samą sesję/generację**, aktualne EXPECTED, pin/kwalifikację binarki, agenta, konto, zadanie, repozytorium oraz ważną jednorazową autoryzację. Nie przedłużamy okna 60 s i nie nadajemy trwałego uprawnienia sesyjnego.
4. **Autoryzacja startu i natywne zatwierdzenie** pozostają osobnymi, związanymi z danym zadaniem/zakresem, wymaganymi krokami. „Jedno zatwierdzenie na sesję” nie zastępuje jednorazowej autoryzacji E4. Osobna analiza bezpieczeństwa byłaby konieczna, by zmienić jej model.
5. Nie robić samoczynnych probe przy wejściu na ekran, co minutę w tle, ani w celu obejścia błędu. Wynik identityStale, identityNotVerified, binaryChanged, mismatch i inne odmowy ma być czytelny. Brak niejawnego wyboru innego konta.
6. Jeśli autoryzacja wymaga sekwencji „autoryzacja → probe → preflight → start”, UI steruje tą kolejnością i minimalizuje liczbę przejść; żadne automatyczne działania nie zatwierdzają okien Windows za użytkownika.

## Warunki i kryteria odbioru

- Aktualizacja CLI lub zmiana SHA, konta, profilu, EXPECTED, procesu unieważnia sesyjny wskaźnik i blokuje start do ponownej kwalifikacji, zgodnie z E4; rozwiązać E-1 bez automatycznego uznawania nieznanych binarek.
- Testy: pierwszy start; drugi start w tej samej sesji po >60 sekundach; restart aplikacji; podmiana konta; update CLI; identity mismatch; Cancel; timeout; brak dostępu CLI; równoczesne inicjacje; niezdolność do uzyskania autoryzacji.
- Żadnych zmian w trwałym modelu uprawnień bez niezależnego review. Osobne worktree, bez zmian E4 podczas prac projektowych.
- **Priorytet:** projekt / analiza kontraktów podczas E6; implementacja po osobnej zgodzie i kwalifikacji, najpóźniej przed uznaniem docelowego UX uruchomień za zakończony. Nie blokuje samego historycznego E5 ani zamkniętego UXF-4B.

## Powiązane rzeczy

- E-1: zmieniony SHA Claude CLI blokuje identity probe — osobny blocker przed E7.
- UI-ACCOUNTS-REDESIGN-01 i ACCOUNT-USAGE-01: odrębny backlog, nie implementować w tym zadaniu.
