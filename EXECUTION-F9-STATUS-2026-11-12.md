# Execution F9 — stan po PR #56 (2026-11-12)

**Repozytorium aplikacji:** [AI-Development-Team](https://github.com/radekgosciniak/AI-Development-Team)  
**Account Assignment:** [PR #55](https://github.com/radekgosciniak/AI-Development-Team/pull/55) — MERGED; merge `70279425787e3d17386dcc29abf0bedd563b4817`; feature commit `03c400a05b39fd3f33e126a2f13b2bff9d4c3135`.  
**Identity Probe + UI:** [PR #56](https://github.com/radekgosciniak/AI-Development-Team/pull/56) — MERGED; merge `ad40f99b877c799c00571647706c1d07936cb07f`; feature commit `354c460d48d134c7a9eed70ffd95dde803d795bb`.  
**State of implementation:** E1/E2 merged and functionally accepted in native tests; E3 is **next authorized implementation task, not reported completed**.

## Delivered foundation

| Component | State | Description |
|---|---|---|
| ACC-ID-01, F-1/F-2, F-3/F-4 | MERGED (PR #53) | Discovery of default/managed Claude and Codex accounts; EXPECTED versus DECLARED; no implied active authentication |
| ACC-LOGIN-01 / EX-LAUNCH-0 | MERGED (PR #54) | Browser OAuth for isolated managed accounts; shared Windows Job process launcher, cancellation and lifetime control |
| ACCOUNT-BIND-01 | MERGED (PR #55), native 11/11 PASS | Durable agent → account binding using existing provider-accounts registry, Rust validation, Agent Catalog and Tasks presentation; P3-1 disabled Remove for assigned accounts |
| F9 E1 — production Identity Probe | MERGED (PR #56), native FUNCTIONAL PASS | CLI account check in managed profiles; production Ticket → Receipt → record/judge; bounded stdin/output, pinned executable, generation and freshness checks; Claude 2.1.322, Codex 0.167.2 |
| F9 E2 — verification UI | MERGED (PR #56), native FUNCTIONAL PASS | CLI binary pin, EXPECTED, explicit identity pin, Check identity/Cancel, fresh VERIFIED and mismatch/error states |
| F9 E3 — backend preflight | **NEXT / NOT CONFIRMED IMPLEMENTED** | Gate for task, approved agent, managed account, EXPECTED, pinned identity, fresh VERIFIED <=60s, unchanged binary and revisions, one-time approval, repository state; no launch |
| E4 — read-only execution | PLANNED | Actual agent CLI execution using existing EX-LAUNCH-0 after E3, not yet authorized in current task |
| E5 — Runs records | PLANNED | Durable run result and metadata, no credential/transcript leakage |
| E6 — Runs controls | PLANNED | Start/Cancel user experience |
| E7 — native end-to-end | PLANNED | First controlled read-only run and review |

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
