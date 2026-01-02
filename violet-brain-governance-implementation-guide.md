# Violet Brain Governance Implementation Guide

## Purpose
Operationalize the governance policy for `violet-brain` and ensure workflows consistently route work between planning and implementation.

---

## Step 1: Consolidate Product Brains

1. Move all existing product-brain content into `violet-brain/product/{product}/`.
2. Preserve historical spec paths and add redirects in `reference/overview.md` if needed.
3. Create `product/shared/` for cross-product work.

---

## Step 2: Establish Directory Standards

Create or enforce:
```
product/{product}/specs/YYYY-MM-DD-feature-slug/
product/{product}/reference/
```

Add required templates:
- `index.md` for each spec
- Standard phase subfolders

---

## Step 3: Add CODEOWNERS

Create or update CODEOWNERS:
```
/product/prism/     @violetio/prism-leads
/product/beam/      @violetio/beam-leads
/product/relay/     @violetio/relay-leads
/product/shared/    @violetio/eng-leads @violetio/pm-leads
/standards/         @violetio/eng-leads
```

---

## Step 4: Update V Plugin Commands

Add a **context router** to `/v:*` commands:

- If in `violet-brain` → proceed with planning workflow.
- If in code repo → ask user to confirm spec path in `violet-brain`.
- If in parent directory → ask user to choose:
  - Active spec (violet-brain path)
  - Active code repo (for implementation)

---

## Step 5: Enforce Docs Impact Block

Add Docs Impact to spec `index.md` template and require it in gates.

---

## Step 6: Roll Out

1. Pilot in one product area (e.g., Prism).
2. Validate workflows end-to-end.
3. Expand to other products.

---

## Optional Automation

- Pre-commit checks for required frontmatter fields.
- Simple lint to ensure spec folders contain `index.md`.

