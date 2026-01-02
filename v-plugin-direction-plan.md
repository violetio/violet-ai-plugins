# V Plugin Direction Plan

## Purpose
Unify the V plugin roadmap by combining:
- Rollout strategy (`ROLLOUT.md`)
- Discovery and Definition specs (`docs/specs/v-discovery-phase-final.md`, `docs/specs/v-definition-phase-final.md`)
- Agentic Engineering Team Charter (`agentic-engineering-team-charter.md`)

This plan defines the next milestones, owners, and outputs to move from documentation to operational usage across Violet repos.

---

## Guiding Principles
- Spec-first workflow: Discovery → Definition → Design → Development → Review
- Human-in-the-loop gates at each phase
- Stable requirement IDs and traceability across docs, flows, and tests
- Durable memory (versioned docs, no silent overwrites)
- Safe tooling (explicit confirmations for destructive actions)

---

## Phase 1: Consolidate the Core Workflow

### Goals
- Ensure the unified `v` plugin is the single entry point for product lifecycle work.
- Lock the Discovery and Definition specs as source of truth.

### Deliverables
- `plugins/v/commands/discovery.md` aligned with `docs/specs/v-discovery-phase-final.md`
- `plugins/v/commands/define.md` aligned with `docs/specs/v-definition-phase-final.md`
- Skills updated to match spec templates and ID conventions

### Checks
- Discovery and Definition commands reference the latest templates
- Resume/archiving behavior is consistent across phases
- Updated frontmatter rules are enforced (created/updated fields)

---

## Phase 2: Close the Charter-to-Plugin Gap

### Goals
- Embed charter practices directly into plugin workflows.

### Deliverables
- Traceability enforcement in `/v:define` (IDs referenced in flows + test coverage matrix)
- Test readiness and coverage matrix required in feasibility outputs
- Charter published as a workflow/knowledge entry (optional)

### Checks
- Every requirement ID appears in at least one flow and one test entry
- Feasibility includes Basis of Estimate and Test Readiness sections

---

## Phase 3: Extend the Workflow Beyond Definition

### Goals
- Add Design, Development, and Review phases to complete the lifecycle.

### Deliverables
- `/v:design` command (technical design spec)
- `/v:build` or `/v:implement` command (implementation guidance + checklists)
- `/v:review` command (quality gates, test signoff)

### Checks
- Each phase has clear inputs/outputs, versioned docs, and gate criteria
- Commands follow the same review loop pattern

---

## Phase 4: Rollout and Adoption

### Goals
- Use `ROLLOUT.md` to deploy in priority repos.

### Deliverables
- Pilot repo configured (.claude/settings.json)
- Feedback loop from pilot use
- Updated rollout checklist with lessons learned

### Checks
- Plugins load without errors
- Teams can run /v:discovery and /v:define end-to-end
- Adoption metrics tracked (see ROLLOUT.md)

---

## Phase 5: Governance and Evolution

### Goals
- Ensure plugin and workflow changes are versioned, reviewed, and measurable.

### Deliverables
- Version bump process followed per CLAUDE.md
- Release notes per plugin update
- Quarterly review of workflow effectiveness

### Checks
- Version increments align with scope of changes
- Clear change log for spec or command updates

---

## Success Metrics
- % of features that complete Discovery + Definition with approved gates
- % of requirements mapped to flows and tests
- Time-to-definition approval (goal: 1–2 review loops)
- Reduction in post-implementation scope changes

---

## Open Questions
- Should the charter be formalized as a workflow plugin or remain a doc?
- Which repo should host Design/Development/Review specs (brain repo vs this repo)?
- What tooling should enforce traceability (manual checklist vs automated checks)?

---

## Immediate Next Steps (Suggested)
1. Align `/v:define` with latest Definition spec and confirm traceability rules are enforced.
2. Pilot in one repo (per ROLLOUT.md) and capture feedback.
3. Draft `/v:design` spec and command outline.

