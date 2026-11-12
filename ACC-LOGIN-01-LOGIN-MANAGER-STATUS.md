# ACC-LOGIN-01 — Account Login Manager / EX-LAUNCH-0 (2026-11-12)

> **Aktualizacja po PR #55 i #56:** ACCOUNT-BIND-01 oraz Execution F9 E1/E2 są już scalone. Poniższe priorytety były aktualne w chwili merge PR #54. Bieżący stan i kolejność E3–E7: [Execution F9 status](EXECUTION-F9-STATUS-2026-11-12.md). 


**Implementation:** [AI-Development-Team](https://github.com/radekgosciniak/AI-Development-Team)  
**Merged:** [PR #54](https://github.com/radekgosciniak/AI-Development-Team/pull/54), 12 November 2026  
**Feature commit:** `fb0054c`  
**Merge commit:** `480e269cc1e67651f5ab62f7878523779cad80f7`  
**Dependency:** [ACC-ID-01 / PR #53](ACC-ID-01-ACCOUNT-DISCOVERY-STATUS.md) (Claude and Codex system/managed Discovery).

## Completed and integrated

| Area | State | Scope |
|---|---|---|
| EX-LAUNCH-0 / process_launcher | MERGED / native functional PASS | One shared Windows native CLI launcher; constrained environment, process Job Object, timeout, cancellation, bounded and filtered output; reusable foundation for future Execution |
| provider_login | MERGED / native functional PASS | `start / status / cancel` for prepared managed profiles; uses shared launcher, never directly starts a second process implementation |
| Claude OAuth | FUNCTIONAL PASS | `claude auth login --claudeai` completed via browser from Command Center, no terminal or code paste; credentials isolated in managed profile; automatic Discovery shows DECLARED |
| Codex OAuth | FUNCTIONAL PASS | `codex login -c cli_auth_credentials_store="file"` completed via browser, `auth.json` in managed CODEX_HOME; automatic Discovery shows DECLARED |
| Cancel / process lifetime | FUNCTIONAL PASS | Both CLI processes cancelled, callback ports released, Command Center closing terminated its launched CLI |
| Credentials / default profiles | FUNCTIONAL PASS within tested scope | Default Claude and Codex credentials and ORCA profile credentials unaffected; no token copying or storing by Command Center |
| Login Manager UI (EN/PL) | MERGED | Sign in with browser, progress, cancellation, automatic Discovery refresh; obsolete “CLI login not collected” text removed; System default vs managed, DECLARED vs UNKNOWN explained |

Native acceptance used an isolated application identifier and test `app_data`, with owner completing browser OAuth. Four synthetic test profiles existed, two signed in; they were deliberately **not imported** into the normal app data. Subsequent standard-identity app check confirmed both system default accounts still DECLARED. The regular app had no managed profiles, so that specific native onboarding route was not retested there; it was covered in isolated end-to-end acceptance.

## Testing / caveats

- Rust process_launcher **15/15**, provider_login **11/11**, provider_identity **105/105** PASS.
- Full Rust **391 pass / 36 fail / 6 ignored**; 36 failures are known baseline `provider_accounts::recovery`, **not a green full suite**.
- Node **445 pass / 0 fail / 5 skip**; TypeScript, Vite and Cargo build PASS.
- Pre-commit and pre-push secret scanning hooks PASS; synthetic false positives were addressed by changing test source expressions, **not disabling rules or adding allowlists**.
- NATIVE-OAUTH-01: Claude and Codex real OAuth FUNCTIONAL PASS. No full Execution, no verified live account attestation.
- No guarantee of functional proxy support, Authenticode verification, or exhaustive native coverage of signing in again to an already signed-in managed profile. Do not treat DECLARED as VERIFIED.
- Separately documented security/high-assurance Recovery workstreams remain independent; EX-LAUNCH-0 login capability **does not authorize general agent execution**.

## Git and synchronization

PR #54 is **merged**. Owner reports Claude independently completed **SYNC PASS**: local `main` and `origin/main` both at `480e269cc1e67651f5ab62f7878523779cad80f7`. No `main` worktree was checked out among 37 worktrees. Fast-forward branch ref synchronization used `git fetch origin main:main` after ancestry validation; no other worktree was modified. ACC-LOGIN-01 and ACC-ID-01 worktrees remain clean and on their original branches. Other unrelated worktrees may have pre-existing changes. Avoid resets, blanket cleanup or worktree deletion.

## Next product priorities (NOT IMPLEMENTED)

1. **ACCOUNT-BIND-01 / Account Assignment:** choose an existing system-default or managed Claude/Codex profile for an agent and task, reconcile with Agent Catalog / Tasks, preserve EXPECTED vs DECLARED vs VERIFIED distinction, enforce ownership and mismatch behavior on backend. Prefer one narrow, functioning increment over a broad architecture rewrite.
2. **Execution integration / F-9:** reuse EX-LAUNCH-0 in the designated Execution implementation, implement explicit launch permission and `Ticket → Receipt → judge` verification. **EX-LAUNCH-0 being merged does not mean agent runs or VERIFIED are available.**
3. **UI-ACCOUNTS-REDESIGN-01:** later Claude Design pass for AI Provider Accounts: reduce technical text, distinguish system/managed accounts, progressive disclosure of details, readable account cards. Strictly preserve behavior, OAuth, statuses, IPC and backend; approve design before React/CSS implementation.
4. **UI-BUG-OVERFLOW-01 (P2):** fix app-shell vertical scrolling past content into blank white background; separate defect from accounts redesign; narrow width (~520 px) overflow remains a P3 follow-up.
5. Consider isolated test-profile cleanup **only after explicit owner decision**; do not delete credentials or remove worktrees automatically.

## Architectural commitments

- **ORCA is inspiration, not an integration.** No ORCA account/session scanning, copying or synchronization.
- System defaults are discovered without adding them or logging in again. Managed profiles are separate.
- Official CLI/browser sign-in; no custom OAuth, password collection or token logging.
- EXPECTED is operator-declared, DECLARED is local discovery, VERIFIED requires future Execution evidence; matching email is not a VERIFIED result.
- Core policy: **ADAPT / NO MIGRATION**, prioritize working functionality and protect existing worktrees / profiles.
