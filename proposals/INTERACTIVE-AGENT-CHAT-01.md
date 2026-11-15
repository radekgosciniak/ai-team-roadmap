# INTERACTIVE-AGENT-CHAT-01 — Direct Claude / Codex Chat in Command Center

**Status:** Roadmap backlog / concept approved; **implementation and agent execution NOT authorized**  
**Added:** 2026-11-15  
**Priority:** After stabilization of Trusted CLI / Launch Integrity and core Execution E7; coordinate UX work with Claude Design  
**Reference:** observed Orca Claude chat UI (screenshot supplied by operator, 2026-11-15) — **UX inspiration only; no assertion about Orca internals or security**

## Product intent

Provide a native, interactive conversation experience with a selected Claude Code or Codex agent **inside AI Development Team Command Center**, alongside the existing supervised task/run workflow. An interactive conversation is **not** equivalent to the autonomous Task → Agent → Account → Execution flow and must not replace it.

### Two complementary experiences

| Experience | User intent | System integration |
| --- | --- | --- |
| Interactive Agent Chat | Ask questions, inspect code, interpret output, propose changes, approve individual actions | Reuse provider accounts, Agent Catalog, process/session controls, run/event UI and existing authorization boundaries |
| Governed Agent Task | Execute a defined task, retain history/evidence and coordinate handoffs/reviews | Existing Projects → Tasks → assigned Agent → Account → Execution → Runs/Sessions, plus optional PILR Roadmap Mode |

**PILR compatibility:** Interactive chat can be a project-linked direct task or a standalone conversation. A roadmap is not mandatory. A chat may suggest changes to a Living Roadmap, but must not silently approve scope changes or mark milestones accepted.

## UX scope to prototype

1. Chat panel embedded in existing Command Center experience (avoid unnecessary new global navigation or duplicate session store); history and resume of interactive sessions where supported.
2. Select Claude Code or Codex, managed account/profile and assigned agent, showing selected identity/verification and a clear blocked/ready state. Never infer authorization from account label alone.
3. Select working directory/project, Git branch and optionally existing project context; visible read/write boundary.
4. Provider-supported model selection and reasoning/effort level; list capabilities from the actual provider/CLI contract rather than hardcoding screenshot options.
5. Display streamed assistant messages, command/tool requests, approval requests, tool results, cancellation, errors, timestamps and an understandable session state.
6. Permission controls inspired by Orca screenshot: **Ask for approval** as default; more permissive modes may be offered only when available and explicitly authorized. **Full access must never be the default** or silently inherited. Distinguish informational permission labels from enforceable backend constraints.
7. Clear action approval, denials, audit evidence and recovery after cancel/restart; no silent acceptance of edits/commands. Native approval when required by existing Execution policies.
8. Visually lightweight, compact interaction; progressive disclosure for CLI/probe/debug details; Polish and English; keyboard and screen-reader support.
9. Link interactive chats to task/run records and project history without storing credentials; adopt existing privacy limits and retention policy or seek a separate approved transcript policy before persisting conversation text.

## Engineering constraints and sequencing

- **Do not embed/clone Orca** or assume its UI proves implementation technique, isolation or permission enforcement; verify official Claude/Codex CLI/API streaming/session/approval contracts separately.
- Reuse Command Center's existing Windows launcher, Job cancellation, identity binding, trusted installation/Launch Integrity, agent accounts and Runs/Sessions. No second broker, separate scheduler or independent account registry.
- Keep EXPECTED / DECLARED / VERIFIED semantics, freshness and explicit operator authorization. Interactive chat must not bypass execution gates or CLI restrictions.
- Do not activate unsupported tool permissions by changing presentation only. Any filesystem mutation, shell execution, network access, subagent delegation and sandbox policy must be checked by real backend/provider controls.
- Dependency: stabilize **Trusted CLI → execution readiness/E7** first. Interactive chat design/prototype can be prepared without executing agents; production implementation requires a separate decision and independent native acceptance.
- Coordinate with `UI-ACCOUNTS-REDESIGN-01`, `UX-EXECUTION-SIMPLIFICATION-01`, `EXECUTION-IDENTITY-60S-UX-01`, approved Command Center UX rules and future Claude Design implementation.

## Acceptance criteria (future)

- Operator can start an explicitly authorized interactive session with selected provider/account, observe streaming responses, cancel and resume if supported.
- Requested edits/commands cannot execute without permission, including after UI restarts; default remains least privilege.
- Account/CLI update or verification failure blocks unsafe actions, with clear diagnostics and no fake VERIFIED.
- Different permission modes have verified behavior in native tests, including negative cases for tool-use and delegation.
- Sessions/events are traceable in existing Runs/Sessions; no token leakage or accidental storage of private transcripts.
- Standalone Direct Task chat works without PILR; project-linked chat may propose roadmap revisions only for subsequent approval.

## Related references

- [PILR — Project Intelligence & Living Roadmap](PILR-project-intelligence-living-roadmap.md)
- [External repositories adoption map](../EXTERNAL-REPOSITORIES-ADOPTION-MAP.md) — Orca/Open Duet/AionUi are **ADAPT / NO MIGRATION** references.
- [Current operational status (2026-11-14)](../CURRENT-STATUS-2026-11-14.md) — historical checkpoint; subsequent Trusted CLI work must follow actual acceptance status.

**No code change, external dependency installation, new permissions, live agent session or implementation authorization follows from this roadmap entry.**
