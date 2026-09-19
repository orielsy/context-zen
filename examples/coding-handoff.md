# Example: Coding Handoff

This example is intentionally fictional. It demonstrates shape, not a real project.

```markdown
---
context_zen:
  version: "0.1"
  continuation: true
  enabled: true
---

# Context Zen Handoff

## Objective
Finish migrating the dashboard data layer to the new typed API client without changing visible behavior.

## Current State
The client wrapper exists and the Accounts page is migrated. Billing still uses the legacy fetch helper.

## Decisions in Force
- Keep the public component props unchanged during this migration.
- Use the generated API types as the source of truth.
- Migrate one feature boundary at a time.

## Constraints / Preferences
- No unrelated UI redesign.
- Preserve existing loading and error states.
- Run the focused tests before the full suite.

## Rejected / Superseded
- Rewriting all data access at once was rejected because it makes regressions harder to isolate.
- A custom handwritten response type was superseded by generated types.

## Active Artifacts
- Branch: `api-client-migration`
- Migrated: `src/features/accounts/`
- Next target: `src/features/billing/`
- Legacy helper: `src/lib/fetch.ts`

## Open Questions
- Confirm whether the Billing export endpoint is represented in the generated client.

## Immediate Next Step
Inspect the generated Billing endpoints, then migrate the Billing query hook without changing its public return shape.
```
