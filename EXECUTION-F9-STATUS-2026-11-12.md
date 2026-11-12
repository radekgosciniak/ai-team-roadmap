# Execution F9 — stan po PR #57 (2026-11-12)

**Repozytorium aplikacji:** [AI-Development-Team](https://github.com/radekgosciniak/AI-Development-Team)  
**Account Assignment:** [PR #55](https://github.com/radekgosciniak/AI-Development-Team/pull/55) — MERGED; merge `70279425787e3d17386dcc29abf0bedd563b4817`; feature commit `03c400a05b39fd3f33e126a2f13b2bff9d4c3135`.  
**Identity Probe + UI:** [PR #56](https://github.com/radekgosciniak/AI-Development-Team/pull/56) — MERGED; merge `ad40f99b877c799c00571647706c1d07936cb07f`; feature commit `354c460d48d134c7a9eed70ffd95dde803d795bb`.  
**E3 Preflight Gate:** [PR #57](https://github.com/radekgosciniak/AI-Development-Team/pull/57) — MERGED; merge `e6d8ee55701a5e97983158eeb231ebd031362bac`; feature commit `7505584173fee9f573e0c0d609079d9bed90ccdd`. Local `main == origin/main == e6d8ee5` confirmed by fast-forward sync.  
**State of implementation:** E1, E2, E3 merged and functionally accepted in native tests. **Next: E4, blocked on resolving the admission/launch TOCTOU gap before any actual execution.**

## Delivered foundation

| Component | State | Description |
|---|---|---|
| ACC-ID-01, F-1/F-2, F-3/F-4 | MERGED (PR #53) | Discovery of default/managed Claude and Codex accounts; EXPECTED versus DECLARED; no implied active authentication |
| ACC-LOGIN-01 / EX-LAUNCH-0 | MERGED (PR #54) | Browser OAuth for isolated managed accounts; shared Windows Job process launcher, cancellation and lifetime control |
| ACCOUNT-BIND-01 | MERGED (PR #55), native 11/11 PASS | Durable agent → account binding using existing provider-accounts registry, Rust validation, Agent Catalog and Tasks presentation; P3-1 disabled Remove for assigned accounts |
| F9 E1 — production Identity Probe | MERGED (PR #56), native FUNCTIONAL PASS | CLI account check in managed profiles; production Ticket → Receipt → record/judge; bounded stdin/output, pinned executable, generation and freshness checks; Claude 2.1.322, Codex 0.167.2 |
| F9 E2 — verification UI | MERGED (PR #56), native FUNCTIONAL PASS | CLI binary pin, EXPECTED, explicit identity pin, Check identity/Cancel, fresh VERIFIED and mismatch/error states |
| F9 E3 — backend preflight + minimal UI | **MERGED (PR #57), native FUNCTIONAL PASS** | Backend READY/BLOCKED gate checks task → approved agent → managed account → pinned/EXPECTED fresh VERIFIED <=60s → clean Git repo → one-time read-only authorization v2; minimal Live Execution panel, no Start |
| E4 — read-only execution | **NEXT / BLOCKED (TOCTOU)** | Implement native non-IPC `admit()` (atomic recheck, consume, launch context; post-consume recheck), close final profile/binary TOCTOU at launcher, enforce true CLI read-only constraints, then controlled execution using EX-LAUNCH-0; no task has run |
| E5 — Runs records | PLANNED | Durable run result and metadata, no credential/transcript leakage |
| E6 — Runs controls | PLANNED | Start/Cancel user experience |
| E7 — native end-to-end | PLANNED | First controlled read-only run and review |

## E3 native acceptance and next blocker

- **PR #57 merged** to `main`; local `main` fast-forward synchronized to `e6d8ee55701a5e97983158eeb231ebd031362bac`; E3 worktree `feat/execution-f9-e3` remained clean and intact (HEAD `7505584`).
- **N3-1…N3-10 PASS:** both Claude and Codex reached backend preflight READY; negative tests returned appropriate BLOCKED stages/codes: stale or missing VERIFIED, dirty Git, changed task/catalog/binding/EXPECTED, expired/missing authorization and system account refusal. E3 does not launch CLI or an agent task.
- **Authorization v2** in the existing `execution_control` store replaces fixed Stage 1 catalog matching for F9; bound to task, current accepted Agent Catalog, agent, managed account, profile, binary, EXPECTED, scope and repository. Approval requires explicit operator confirmation. Consumption is internal, not IPC.
- Two defects fixed during native test: v2 responses wrongly tagged `/1`; TypeScript compared serialized JSON while Rust sorted object keys. Cross-language fixture regression tests added.
- E3 test baseline: **Rust 459 pass / 36 known Recovery fail / 6 ignored**, **Node 490 pass / 0 fail / 5 skip**, tsc/Vite/Cargo PASS; no evidence of new regression.
- **BLOCKER FOR E4 — TOCTOU:** E3 `check` and v2 `consume` are not an atomic admission gate. E4 must create an internal Rust `admit()` that checks current chain while holding appropriate task/admission synchronization, consumes the one-time authorization, rechecks after consumption and aborts fail-closed on any change (consumption remains consumed). The residual gap between admission and EX-LAUNCH-0 needs final pinned executable SHA and correct managed-profile validation in launcher. Read-only must be actually enforced, not just requested by prompt.
- Open UI backlog: `UI-LIVE-EXECUTION-01` / `UI-BUG-OVERFLOW-01`: E3 panel's “Prepare authorization” extends outside its container causing horizontal scrolling; the oversized “Live Execution is not available” placeholder and cramped status/info hierarchy need restructuring. Fix UI access/overflow when necessary; full Claude Design redesign after E7.
- No live agent execution authorized/performed yet. Test Codex EXPECTED and test Agent Catalog remain intentionally mutated in isolated **test** app data; do not move them into normal data or clean up without approval.

## E1/E2 evidence

- Native login identity check: **Claude Code VERIFIED** and **OpenAI Codex VERIFIED** on managed test accounts. Confirmation of EXPECTED and identity pin was operator-initiated.
- Codex 0.167.2: `account/read` needs stdin open until the response. Its `account/updated` notification may precede `account/read`; allow only when it contains expected limited fields, matches subsequent response, and no later disqualifying state change occurs. Other account state-change notifications remain UNKNOWN, not a bypass. Earlier PARTIAL PASS was fixed and retested FUNCTIONAL PASS.
- FIX-A: `attestDeclared` explicitly confirms the discovered account as EXPECTED after native re-read; no manual UUID needed in that workflow, and no automatic VERIFIED.
- FIX-B / D-FIX-B2: targeted Codex 0.167.2 notification handling.
- FIX-C: corrected stale texts, cancellation/status presentation, card spacing, accessibility.
- Rust `provider_identity`: **139/139 PASS**; full Rust **434 PASS / 36 known Recovery FAIL / 6 ignored**; Node **478 PASS / 0 FAIL / 5 SKIP**; TypeScript, Vite, Cargo builds and Git hooks PASS. Full Rust suite is **not green**.
- 28 files in PR #56. One native binary per provider available, so changing to a *different* binary was not tested natively. Tests used isolated managed-profile app data, no real agent task execution or OAuth relogin.

## Approved Execution MVP decisions (D-F9-1…D-F9-7)

1. Managed accounts only for MVP; default system profiles remain discovery-only for execution.
2. First agent execution must be enforced **read-only**, with no file modification, Git changes or applying patches; vendor limitations must not be papered over by a prompt.
3. Launch requires current EXPECTED match, pinned binary and identity and fresh VERIFIED from CLI probe, no older than 60 seconds; invalidation on configuration changes.
4. Codex 0.167.2 supported after protocol verification; constrained stdin through shared launcher.
5. Execution start authorization uses **approved Agent Catalog** and task/assignment revisions, replacing the older fixed Stage-1 catalog binding.
6. Run storage restricted to safe metadata, CLI session identifier and result up to 4 KB; no full transcript or secrets.
7. Native Windows confirmation dialog for launch, rather than webview-only approval.

**E3 task authorized:** sync local main with the merge of PR #56, create a fresh E3 worktree from the synced main, implement *only* Rust preflight READY/BLOCKED and synthetic tests. No real task runs, no new OAuth, no automatic merge. The user has not yet supplied an E3 implementation report; keep it as pending.

## UI backlog — after E7 (Claude Design)

- `UI-ACCOUNTS-REDESIGN-01`: clearer account cards, progressive disclosure, dark-themed account selector, readable status and responsive layout; preserve existing functionality, backend contracts and identity semantics. Claude Design may implement React/CSS after operator design approval.
- `UI-EXPECTED-01`: remove/replace old manual **Change expected account** form asking for Claude organization UUID / Codex workspace. Retain the explicit **Use detected account as EXPECTED** flow with native re-read; user should never need to type technical UUIDs.
- `UI-BUG-OVERFLOW-01` (P2): app shell scrolls into blank white area below UI; also nested scrollbars, horizontal overflow at narrow (~520 px) viewport, oversized cards.
- Functional blockers encountered in E3–E7 may be fixed when encountered; cosmetic redesign is intentionally deferred until after E7.

## Operational notes / caveats

- **VERIFIED is a time-bound observation, not permanent login status.** Execution E3 must re-evaluate freshness and bindings immediately before any future launch.
- System profiles are recognized as DECLARED but not eligible for the initial managed-only Execution MVP.
- Profile test app_data and older worktrees were deliberately preserved. No cleanup or credential deletion is implied.
- Local main sync after PR #55 was independently reported PASS at `70279425787e3d17386dcc29abf0bedd563b4817`; local sync after **PR #56 is not yet confirmed** by a report.
- 36 baseline Rust Recovery failures remain an unrelated open item. External repositories are inspiration only: **ADAPT / NO MIGRATION**; no ORCA account/session integration.
