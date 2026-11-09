# External repositories — adoption map for AI Development Team

**Status:** architecture reference / planned evaluation, **NOT IMPLEMENTATION AUTHORIZATION**  
**Last updated:** 2026-11-09  
**Owner:** AI Development Team architecture roadmap  
**Canonical public roadmap:** [AI Team Roadmap](https://github.com/radekgosciniak/ai-team-roadmap)  
**Project implementation:** [AI Development Team](https://github.com/radekgosciniak/AI-Development-Team)

This document is a durable record of which externally reviewed projects may be useful, **when** to revisit them and **what must be verified** before implementation. It is a decision register, not a dependency list or permission to copy code.

## Reviewed source snapshots

| Source | Pinned version considered | Role |
|---|---|---|
| [AionCore](https://github.com/iOfficeAI/AionCore/tree/503a87199eb978790ca77675b39c84b241535bf3) | `503a87199eb978790ca77675b39c84b241535bf3` | Multi-agent orchestration backend reference |
| [AionUi](https://github.com/iOfficeAI/AionUi/tree/e108b6b56b1a6eb99a00904878d0582f657b2908) | `e108b6b56b1a6eb99a00904878d0582f657b2908` | Desktop UI / sessions / team activity reference |
| [Rustbox](https://rustbox.sh/blog/rustbox-vs-judge0-vs-e2b-vs-piston-code-execution-engine) | Article reviewed separately; no pinned source audit in this register | Possible future isolated code-execution research, **not chosen** |

Original local study: `C:\AI-Development-Team\reports\AION-ANALYSIS\AION-ARCHITECTURE-SECURITY-ASSESSMENT-2026-11-08.md`. Detailed evidence remains local; do not publish private environment artifacts.

## Adoption by stage

| Stage | Our need | Reference pattern | Decision | Preconditions / completion evidence |
|---|---|---|---|---|
| **AGCAT-1/2** | Trusted catalog, native user approval and agent membership | No Aion integration | **KEEP OWN IMPLEMENTATION** | AGCAT-1 accepted with conditions; AGCAT-2 PR #51 Draft, Windows native acceptance still pending |
| **AGCAT-3** | Select verified agent for Tasks without creating an execution path | Catalog identity/revision / atomic membership check; Aion is conceptual input only | **OWN CONTRACT, NO IMPORT** | Hold catalog lock through Tasks commit; verify identity and revision; fail closed on missing membership; do not turn on `execution_ready` |
| **Stage 1A.2** | Real provider CLI integration and process lifecycle | AionCore ACP/process supervision concepts, session model | **ADAPT AFTER REVIEW** | Independent Windows process tree/Job Object review; explicit operator permissions; CLI capability and cancellation tests; recovery semantics; avoid blind auto-approval |
| **P7 — Real Agent Broker** | Reliable agent-to-agent handoffs and work allocation | AionCore Mailbox, TaskBoard, Team Scheduler, team coordination | **PRIMARY ADAPT CANDIDATE** | First independently review isolated Mailbox PoC (54/54 tests reported, delta review pending). Specify durable state, lease/ack, idempotency, deadlines, crash/restart and sender authentication; implement within our trust model |
| **P7 — Scheduled tasks** | Repeatable schedules without duplicate work | AionCore scheduler/cron and task deduplication | **ADAPT CONCEPTS** | Timezone/DST rules, persistence, missed-run policy, concurrency/idempotency and operator-controlled scheduling; no unattended command execution by default |
| **P8 — Live Execution / team activity** | Visibility of agent runs, handoffs, logs, approvals | AionUi team/session UI; AionCore status/activity events | **INSPIRE / ADAPT** | Real backend events first; avoid mock statuses, preserve existing approved Command Center layout and UX decisions; operator consent and accessible event history |
| **Possible future sandbox execution** | Isolate untrusted code if requirements justify it | Rustbox and comparison with Judge0/Piston/E2B | **RESEARCH ONLY** | Define threat model and OS support first; independent code/security audit, limits and isolation evidence; not a prerequisite for AGCAT or P7 |

## Explicit boundaries

1. **No migration to AionCore/AionUi** and no source-code import without a separate license review, security review and operator decision.
2. **No implicit approval** of ACP/MCP tool calls, agent launches, file edits or Git operations. Aion's approval defaults must not override our native operator gates.
3. **Do not equate file ownership or same-SID attribution with security against a hostile same-SID process.** Windows NTFS, ACL, races, process containment, Job Objects and recovery must be assessed within the relevant stage.
4. Keep **TaskBoard** (work lifecycle) separate from **Mailbox** (messages / handoffs); document at-least-once delivery and make handlers idempotent. Exactly-once must not be claimed without proof.
5. Before starting each stage, re-examine pinned external commits and current upstream changes; record reviewed version, accepted patterns and rejected patterns.
6. This register does not change state of P3-1A1 Recovery, P6-A, W3.3 or AGCAT-2 native acceptance, and grants **no authorization** for PR merge or agent execution.

## Next concrete decision points

- **Now:** finish AGCAT-2 native smoke qualification (W-1 V3 preflight controller list-construction blocker), keep [PR #51](https://github.com/radekgosciniak/AI-Development-Team/pull/51) as Draft until native acceptance decision.
- **After AGCAT-2:** plan AGCAT-3 Tasks membership, using our approved catalog contract.
- **Before P7 implementation:** ask an independent reviewer to perform **Mailbox PoC P2 delta review**; then draft a narrow P7 architecture decision comparing AionCore Mailbox/TaskBoard/Scheduler to our requirements, including Windows and security gaps.
- **Before P8 implementation:** decide an event schema for session/run/agent status and evaluate AionUi's interface patterns against our approved Command Center UX.

**Authority:** the public roadmap summarizes status; this document records external-reference usage. Neither one supersedes project security gates or explicit operator approval.
