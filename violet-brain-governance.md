# Violet Brain Governance

## Purpose
Define how Violet uses `violet-brain` as the central knowledge hub and how work moves between planning and implementation.

---

## Core Policy

1) **All non-coding work starts in `violet-brain`.**
   - Discovery, Definition, Design, Delivery planning, and internal reference docs live here.

2) **All product brains consolidate into `violet-brain`.**
   - No separate product-brain repos.
   - Product structure and ownership are enforced through directories and CODEOWNERS.

3) **All coding and implementation work happens in the applicable code repo.**
   - Specs are referenced from `violet-brain`, code is authored in the service/app repo.

---

## Repo Roles

### violet-brain
- **Primary source of truth** for specs and internal product reference.
- Houses:
  - Specs (time-boxed, versioned)
  - Reference docs (evergreen)
  - Standards and templates

### Code repos (e.g., BeamService, VioletDashboard)
- **Primary source of truth** for implementation.
- Houses:
  - Code
  - Tests
  - Build/deploy configuration

---

## Structure

### Specs
```
product/{product}/specs/YYYY-MM-DD-feature-slug/
  index.md
  01-discovery/
  02-definition/
  03-design/
  04-development/
  05-delivery/
```

### Reference Docs
```
product/{product}/reference/
  overview.md
  architecture/
  api/
  operations/
  ux/
```

---

## Context Routing Rules

### Default
- Start all planning conversations in `violet-brain`.
- Move to code repos only when implementation work begins.

### Cross-Repo Coordination
- `violet-brain` remains the hub.
- At any time, the active spec folder is the authoritative scope.
- When switching to a code repo, always link back to the relevant spec.

### Non-Negotiables
- Specs never live in code repos.
- Code changes never happen in `violet-brain`.

---

## Ownership and Review

- CODEOWNERS enforce product boundaries in `violet-brain`.
- Specs require product owner approval at phase gates.
- Code changes follow normal repo review practices.

---

## Docs Impact Policy

Every spec `index.md` must include:
```
## Docs Impact
- External docs affected? [Yes/No]
- External pages: [links]
- Required updates: [summary]
- Owner: [name]
- Status: [not started/in progress/done]
```

---

## Enforcement (Soft + Hard)

### Soft Enforcement
- Agents prompt for missing prerequisites.
- Resume chooser prevents wrong-file continuation.

### Hard Enforcement
- CODEOWNERS on `violet-brain` directories.
- Gate checks required to move forward.

