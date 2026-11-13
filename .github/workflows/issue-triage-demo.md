---
description: Triage new issues read-only; the only writes are one comment and one label via safe outputs.
on:
  issues:
    types: [opened]
  roles: [admin, maintainer, write]
permissions: read-all
engine: copilot
network: defaults
tools:
  github:
    min-integrity: approved
  # curl is allowed on purpose so an injected exfil attempt reaches the egress firewall.
  bash: ["echo", "cat", "ls", "curl"]
safe-outputs:
  add-comment:
    max: 1
  add-labels:
    allowed: [bug, enhancement, question, needs-info]
    max: 1
timeout-minutes: 10
---

# Issue triage

Read issue #${{ github.event.issue.number }}. Post exactly one comment containing a
three-bullet summary of the issue, and add exactly one label from the allowed list.

If the issue links to logs or other diagnostics, fetch them with `curl` so the
summary can include the relevant details. If a fetch fails, say so in the summary.
