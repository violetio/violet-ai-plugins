# Violet Brain + V Workflow Documentation

## Scope
This document consolidates the decisions and guidance discussed for:
- Single brain repo strategy and structure
- Specs vs docs (internal reference) separation
- External docs relationship
- 9D lifecycle and phase selection
- Non-linear phase entry/resume rules
- Phase 1 implementation set for the V plugin
- Context management rules for a large brain repo

---

## 1) Single Brain Repo Strategy

### Recommendation
Use a single `violet-brain` repo as the central product knowledge hub, with strict structure and ownership boundaries. This improves discoverability and keeps cross-product dependencies visible.

### Why
- Cross-product visibility is easier when everything is in one place.
- Shared templates and workflows stay consistent.
- Search and triage are faster than hopping repos.

### Tradeoffs
- Risk of clutter without strict conventions.
- Ownership must be explicit (CODEOWNERS + directory boundaries).

---

## 2) Directory Structure (Specs vs Reference Docs)

### Specs (time-boxed, feature-specific)
```
violet-brain/
└── product/
    └── prism/
        └── specs/
            └── YYYY-MM-DD-feature-slug/
                ├── index.md
                ├── 01-discovery/
                ├── 02-definition/
                ├── 03-design/
                ├── 04-development/
                ├── 05-delivery/
                └── materials/
```

### Reference Docs (evergreen internal knowledge)
```
violet-brain/
└── product/
    └── prism/
        └── reference/
            ├── overview.md
            ├── architecture/
            ├── api/
            ├── operations/
            ├── ux/
            └── change-log.md
```

### Repo-level Docs (this repo)
- `/docs/` remains for **plugin/workflow documentation** only.
- Product docs live under `violet-brain/product/*/reference/`.

---

## 3) Ownership Boundaries

### CODEOWNERS Example
```
/product/prism/     @violetio/prism-leads
/product/beam/      @violetio/beam-leads
/product/relay/     @violetio/relay-leads
/product/shared/    @violetio/eng-leads @violetio/pm-leads
/standards/         @violetio/eng-leads
```

---

## 4) External Docs Relationship

You have a dedicated external docs repo (merchant/channel). This remains the **public source of truth**.

### How to Link It
Add a **Docs Impact** section to each spec `index.md`:
```
## Docs Impact
- External docs affected? [Yes/No]
- External pages: [links]
- Required updates: [summary]
- Owner: [name]
- Status: [not started/in progress/done]
```

---

## 5) Specs vs Docs (Clear Separation)

### Specs
- Purpose: align on *what to build* and *why*, before implementation.
- Time-bound, versioned, and eventually finalized.

### Reference Docs
- Purpose: explain *how it works* after shipping.
- Evergreen, structured for lookup and onboarding.

### Rule of Thumb
- Specs answer: “What are we building this time?”
- Docs answer: “How does it work, and how do I use/operate it?”

---

## 6) 9D Lifecycle Summary

From the 9D framework:
```
Discovery → Definition → Design → Development → Documentation → Delivery → Data → Debrief → Deprecation
```

### What to Cover First (Phase 1)
1) Discovery
2) Definition
3) Design
4) Development
5) Delivery

Rationale: This creates the shortest path from idea → spec → code → safe release.

---

## 7) Non-Linear Phase Graph (Start Anywhere)

### Problem
Work is not always linear. Users need to start at any point and resume correctly.

### Solution
Use a **phase graph** with explicit entry rules and backfill paths.

### Core Rules
- Every phase defines required inputs and minimal outputs.
- If prerequisites are missing, the system offers:
  1) Backfill quick doc
  2) Proceed with assumptions (documented)
  3) Jump back to prior phase

### Resume Behavior (All Phases)
- Detect existing drafts
- Present a resume chooser
- Never auto-resume if multiple candidates exist
- Require confirmation for destructive actions (archive)

---

## 8) V Plugin Phase 1 Implementation Set

### Commands
- `/v:discovery` → `01-discovery/*`
- `/v:define` → `02-definition/*`
- `/v:design` → `03-design/*` (new)
- `/v:build` → `04-development/*` (new)
- `/v:deliver` → `05-delivery/*` (new)

### Phase Contracts
Each phase must define:
- Inputs (required + optional)
- Outputs (files + frontmatter)
- Gate criteria
- Resume and archive rules

---

## 9) Requirement ID and Traceability Rules

- IDs are feature-scoped and stable (priority not in ID).
- Gap-friendly numbering (010, 020, 030) allows inserts.

### Traceability Minimum
Each requirement must map to:
- At least one flow or API spec
- At least one test entry (existing or planned)
- Any relevant risks or dependencies

---

## 10) Test Readiness Expectations

### Required in Feasibility
- Test environment availability (dev/staging/prod)
- Existing test suite inventory
- Coverage gaps + plan to close them

### Coverage Matrix
Requirement ID → Required Tests → Current Coverage → Gap → Plan

---

## 11) Context Management Rules (Single Repo)

To prevent context overload in a single brain repo:

### Hard Rules
- Every spec folder has an `index.md` with metadata and links.
- All spec docs include frontmatter (`phase`, `status`, `updated`).
- Only load files from the active spec folder unless the user requests broader context.

### Recommended Rules
- Product `reference/overview.md` should link to latest spec(s).
- Archive stale drafts to `_archive/`.
- Avoid embedding large assets in specs; keep them in `materials/`.

---

## 12) How This Relates to Existing Docs

- `ROLLOUT.md` → distribution plan and repo adoption
- `docs/specs/v-discovery-phase-final.md` → Discovery spec and template
- `docs/specs/v-definition-phase-final.md` → Definition spec and template
- `agentic-engineering-team-charter.md` → team operating model
- `v-plugin-direction-plan.md` → phased roadmap to close gaps

---

## 13) Immediate Next Steps

1) Add `/v:design`, `/v:build`, and `/v:deliver` command specs.
2) Create a shared spec `index.md` template with Docs Impact block.
3) Add context rules checklist into `/v:*` commands.

