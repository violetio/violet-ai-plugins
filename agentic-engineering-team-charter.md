# Agentic Engineering Team Charter (Violet)

## Purpose
Establish a clear, low‑overhead operating model for an agentic engineering team that produces reliable, reviewable outputs while keeping human decisions explicit and traceable.

## Core Principles
- **Spec‑first**: Discovery → Definition → Build. No build without approved specs.
- **Human‑in‑the‑loop gates**: Explicit approvals at scope, UX, and feasibility.
- **Stable identifiers**: Requirements IDs are stable and referenced across docs.
- **Traceability**: Every requirement maps to design decisions, tests, and risks.
- **Safety by default**: Confirmation for destructive actions; sandboxed tools.
- **Durable memory**: Versioned documents; no silent overwrites.
- **Fast feedback**: Tight review loops with clear action items.

## Roles and Responsibilities

### 1) Discovery Lead (Agent + Human)
**Goal**: Convert a raw request into an approved Discovery Brief.

**Outputs**:
- Intake document (approved)
- Discovery Brief (approved)

**Gate**: Discovery Brief approved.

### 2) Requirements Analyst (Agent)
**Goal**: Produce a complete, testable requirements spec.

**Outputs**:
- Requirements document (approved)
- Stable requirement IDs
- Success criteria and acceptance criteria

**Gate**: Requirements approved.

### 3) Experience Designer (Agent)
**Goal**: Define user flows and interactions per surface.

**Outputs**:
- Persona × Surface matrix
- User journeys and flows
- Wireframes (GUI) and API specs (API surfaces)

**Gate**: Experience approved.

### 4) Feasibility Architect (Agent)
**Goal**: Validate technical approach, risks, and effort.

**Outputs**:
- Feasibility analysis
- Basis of estimate
- Test readiness + coverage matrix

**Gate**: Feasibility approved.

### 5) Delivery Lead (Human)
**Goal**: Decide go/no‑go and prioritize execution.

**Outputs**:
- Go/No‑go decision
- Scope adjustments (if needed)

## Artifacts and Source of Truth

**Primary source of truth**: Specs in the brain repo.

**Secondary tracking**: Linear tickets link back to spec IDs.

- Requirements IDs are feature‑scoped and stable.
- Use IDs in:
  - Experience flows
  - Feasibility risks
  - Test coverage matrix
  - Linear ticket titles or checklists

## Document Lifecycle

1. Draft v1 created by agent
2. Human review with inline feedback
3. Agent responds to feedback
4. Iterate until “final” (no feedback blocks)
5. Approved documents are immutable

## Traceability Map (Minimum)

Each requirement must map to:
- At least one user flow or API spec
- At least one test entry (planned or existing)
- Any known risk or dependency

## Tooling and Guardrails

- Sandbox by default
- Explicit confirmation for destructive actions
- Prefer read‑only exploration before edits
- Explicit tool usage logging in agent outputs

## Testing Readiness (Minimum Standard)

For every feature:
- Inventory current tests in the area
- Map each P0/P1 requirement to a test type
- Identify gaps and plan to close them
- Confirm environments (dev/staging/prod) and test data strategy

## Operating Cadence

- **Discovery review**: 30–60 minutes
- **Definition review**: 60–90 minutes
- **Feasibility review**: 30–60 minutes
- **Build kickoff**: only after all gates pass

## Success Metrics

- % of requirements with test coverage
- Review cycles to approval (goal: 1–2)
- Post‑implementation changes due to unclear scope (goal: low)
- Rework due to missing requirements (goal: near zero)

## Minimal Stack

- Docs: Markdown in brain repo
- Work tracking: Linear
- Diagrams: Mermaid + ASCII wireframes
- Tests: Unit + Integration + E2E (where feasible)

## Adoption Plan

1) Pilot 1–2 features end‑to‑end
2) Review gaps and update the charter
3) Roll out to broader team

