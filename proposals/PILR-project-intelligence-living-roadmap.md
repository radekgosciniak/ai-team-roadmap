# PILR — Project Intelligence & Living Roadmap

**Status:** Concept approved; implementation not authorized  
**Decision:** PILR-ADR-001 — Optional Roadmap Governance  
**Date:** 2026-11-15  
**Target:** AI Development Team Command Center (Tauri 2 + React/TypeScript)

## Goal

Provide a persistent, versioned, project-level plan that guides AI agents through long-running software development without losing decisions, scope, dependencies, or verification evidence. Governance must remain optional for directly assigned tasks.

## Two execution modes

### Roadmap Mode (project-governed)
- For greenfield applications, multi-stage development and long-running work.
- Project Blueprint captures requirements, goals, constraints, architecture and acceptance criteria.
- Living Roadmap tracks milestones, work items, dependencies, owners, status and blockers.
- Agents receive the current approved plan and a scoped context snapshot before execution.
- Roadmap governs work order and scope; proposed deviations require an impact assessment and authorization when outside approved scope.
- An agent's claim of completion does not equal verified acceptance. Tests, independent reviews and quality gates determine acceptance.

### Direct Task Mode (ad hoc, no roadmap required)
- A user may assign an immediate, standalone task to an agent without creating a roadmap or milestone.
- Tasks may be project-linked or completely independent (e.g. a one-off repository audit).
- Direct tasks retain applicable permissions, safety restrictions and explicit approval gates; they do not bypass these controls.
- Results are recorded in task/run history. When linked to a project, material findings can propose roadmap changes, but cannot silently alter an approved plan.
- A completed direct task may later be associated with a roadmap work item.

## Proposed modules and integration

| Capability | Integration |
| --- | --- |
| Project Blueprint | Extend project configuration and requirements |
| Living Roadmap | Milestones, work items and dependencies within project detail |
| Project Pulse | Dashboard with progress, decisions, blockers and Git activity |
| Decision Registry | Versioned architecture decisions (ADR), approvals and rationale |
| Project Context Snapshot | Source-grounded context assembled for each agent session |
| Quality Gates | Evidence, tests, independent review and explicit acceptance |
| Change Control | Propose, assess, approve and version scope/plan changes |
| Project Drift Detection | Flag unauthorized scope changes, ADR conflicts, unverified completion and work out of sequence |

### Context and provenance

Keep separate records of (1) intended work, (2) agent-reported work and (3) independently verified outcomes. Context snapshots should reference durable source reports, commits and decisions rather than relying on entire chat transcripts. Prefer human-readable, version-controlled Markdown/JSON alongside any application database, with a defined synchronization strategy.

### Change governance

Agents in Roadmap Mode may flag new findings and propose plan updates, but cannot unilaterally change approved goals or scope. A proposed change records its reason, dependency impact, affected milestones and approval status. Direct Task Mode remains available at any time without silently changing the roadmap.

## Implementation sequencing

**Do not displace current AI Command Center stabilization and core-function priorities.** First specify data model, integration contracts, UX prototype and acceptance criteria. Implement PILR only after separate authorization and appropriate readiness gates. Reuse existing Projects, Tasks, Runs and Sessions instead of building a separate application.

## Suggested acceptance criteria (future implementation)

1. Create a project blueprint and versioned roadmap from a new-project specification.
2. Resume a new agent session using an accurate, provenance-linked context snapshot.
3. Enforce roadmap dependencies and approval rules in Roadmap Mode.
4. Execute Direct Task without any roadmap, both project-linked and standalone.
5. Record evidence and independent verification before marking work accepted.
6. Propose and approve roadmap changes with an audit trail.
7. Detect scope drift and distinguish warnings from policy-enforced blocks.
8. Preserve roadmap and decision history in portable, version-controlled files.

This document records an approved product/architecture direction, **not** authorization to modify the running application or execute agents.
