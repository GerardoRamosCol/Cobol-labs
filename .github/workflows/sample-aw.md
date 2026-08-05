---

on:
  issues:
    types: [opened, edited]

permissions: read-all

strict: true

safe-outputs:
  add-comment:
  add-labels:
    allowed: [valid]
    max: 1
  close-issue:
    target: triggering
    state-reason: not_planned

---

# Issue validator

Analyze the triggering issue's title and body.

- If the issue is related to COBOL, add the `valid` label and comment with a brief explanation.
- If the issue is not related to COBOL, close it as `not_planned` with a brief explanation in the closing comment.
- If the triggering issue is already closed, call `noop` and explain that no action is required.