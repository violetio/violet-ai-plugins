# Context Router (for /v:* commands)

Use this prompt at the start of any `/v:*` command to enforce correct repo context and reduce drift.

---

## Context Check

1) Determine current working repo.
2) Ask user to confirm the **active spec path** in `violet-brain`.
3) If implementation is requested, ask for the **active code repo**.

---

## Prompt

```
I need to confirm context before we proceed.

Active spec (in violet-brain): [path]
Active code repo (if implementation): [repo or N/A]

Which should I focus on right now?
1) Spec work in violet-brain
2) Implementation work in code repo
3) Cross-repo coordination (spec + code)
```

---

## Routing Rules

- **If Spec Work**:
  - Stay in `violet-brain`.
  - Only read/write within the active spec folder unless asked.

- **If Implementation**:
  - Confirm spec path exists.
  - Switch to code repo for edits and tests.
  - Reference spec IDs in commit/PR notes.

- **If Cross-Repo**:
  - Use `violet-brain` as hub.
  - Explicitly declare the active repo when switching.
  - Never edit code in `violet-brain` or specs in code repos.

---

## Fallbacks

- If spec path is unknown, offer to create or locate it.
- If user declines to provide a spec, proceed with assumptions and log them.

