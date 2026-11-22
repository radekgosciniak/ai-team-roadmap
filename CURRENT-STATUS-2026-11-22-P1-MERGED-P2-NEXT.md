# AI Development Team — P1 MERGED, S4-B2-P2 NEXT

**22 listopada 2026 · Trusted CLI / TC-4-SESSION**

## Stan zatwierdzony

- **S4-B1 / PR #78:** MERGED `53623b9e8ad646651c4ea8bd26e9ff875ad75a70`, syntetyczne IPC i natywny Caller.
- **ACL-1 / PR #79:** MERGED `1dc231d1094de671ab2048f2fa4c389f26e9da30`, 29 komend bazowych dla webview `main`, `terminal_session` wyłącznie w Windows + `tc4-synthetic`.
- **P1 / [PR #80](https://github.com/radekgosciniak/AI-Development-Team/pull/80): MERGED**, head `b72c459d34fbd5d611211d6135cf24dfcef9d90b`, merge commit **`1da392e56c6d6d3709fd7116983834cd497cd9eb`**.
- **P1 independent acceptance:** Codex **READY FOR MERGE WITH P2 GATE**; J1 short write completeness **PASS**; J2 indeterminate fsync and publication barrier **PASS**; targeted Rust std/tc4 69/69 each. FULL **NOT RUN**; native smoke of packaged Tauri EXE **NOT RUN**.

## P1 — dostarczone

Pakowanie `cc-tc4-synthetic.exe` wraz z `command-center.exe` przy build-time `CC_TC4_SYNTHETIC_SHA256`, kontrola pliku PE/manifestu i bramki TC-0; pasywny `verify` bez wykonywania EXE z pakietów operatora; osobne debug/release; niezmienne katalogi wersji, profilowy lock Windows, `HISTORY.jsonl` jako dziennik commitów, `PENDING` do rozstrzygania niepewnych operacji oraz `ACTIVE` jako wskaźnik. FIX-01/02/03 usunęły wcześniejsze blokery niezależnych przeglądów; każdy etap miał własne dowody i raporty lokalne w `C:\AI-Development-Team\reports\TRUSTED-CLI\`.

## NEXT — S4-B2-P2 (obowiązkowa bramka przed React UI)

1. Zbudować / odczytać pakiet z P1 i sprawdzić `verify` (nie uruchamiając zawartości pakietu podczas verify). Do natywnego smoke uruchomić **dokładnie** `command-center.exe` z tego pakietu z `cc-tc4-synthetic.exe` obok niego.
2. W prawdziwym **Windows Tauri WebView2** zapewnić kontrolowany, autoryzowany sposób wywołania istniejącego `terminal_session` z webview `main`. `devtools: false` — rozstrzygnąć sposób testowego wywołania bez produkcyjnego bypassu ani rozszerzenia ACL. Weryfikować status każdej operacji.
3. Natywnie sprawdzić create/start/status/read/write_input/finish, stany końcowe, filtrowanie wyjścia, operator/lifetime window-close i `owner_lost`, recovery oraz odmowy dla złego caller, braku/zmiany pinu, nieistniejącego synthetic, nieautoryzowanego webview.
4. Sprawdzić standardowy release bez `tc4-synthetic`: brak `terminal_session` i jej grantów, zgodność istniejących ekranów aplikacji.
5. Zapisane dowody + niezależny odbiór Codex; dopiero po PASS rozpocząć React UI terminala syntetycznego.

**Nie instalować zewnętrznych frameworków agentów ani nie uruchamiać prawdziwych Claude Code / Codex** w P2. Przed przyszłym UI wykorzystać celowane analizy AionUi, Orca, OpenWorker, a ECC (nowo zarejestrowane w [mapie adopcji](EXTERNAL-REPOSITORIES-ADOPTION-MAP.md)) przy przyszłej orkiestracji / kontroli jakości. Obowiązuje analiza przypiętego kodu, licencji i ADOPT / ADAPT / REJECT.

## Otwarte i odroczone

- **G-ENV OPEN, G1 BLOCKED, D-3 Full Launch DEFERRED**; Trust Broker = Custody-Only; brak autoryzacji realnych agentów.
- **D-P1-01 toolchain pin DEFERRED**; **D-P1-02 build-path removal DEFERRED**.
- Trwałość metadanych katalogu Windows/Node przy zaniku zasilania nie została udowodniona; wystąpienie `journalCorrupt` pozostaje fail-closed; bez automatycznego rollbacku i czyszczenia starych wersji.
- `UV_FS_O_EXLOCK` sprawdzony na Windows 11 / Node v24.20.0; testy blokady powtórzyć po zmianie Node.
- ACL-2 (cztery `projects_*`), ACL-3 (trzy legacy `tasks_*`), ACL-4/5 — osobne przyrosty; nie mieszać z P2.
