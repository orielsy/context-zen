# Example: Research Handoff

This example is intentionally fictional. It demonstrates shape, not a real research result.

```markdown
---
context_zen:
  version: "0.1"
  continuation: true
  enabled: true
---

# Context Zen Handoff

## Objective
Determine whether a proposed browser primitive meaningfully reduces framework-specific runtime responsibility.

## Current State
The investigation has separated the problem into native capability, framework policy, and compiler/tooling concerns. Evidence supports the first distinction, but no performance claim has been established.

## Decisions in Force
- Treat the work as an architecture exploration, not a prediction.
- Prefer primary specifications over commentary.
- Separate implemented behavior from proposals.

## Constraints / Preferences
- Do not claim that the primitive replaces frameworks.
- Do not invent benchmark numbers.
- Keep unresolved standards questions explicit.

## Rejected / Superseded
- The original binary framing of "platform versus framework" was rejected.
- A maturity ranking was dropped because the proposals are not directly comparable.

## Active Artifacts
- Working notes: `notes/primitives.md`
- Source matrix: `notes/sources.md`

## Open Questions
- Which responsibilities are intentionally application policy rather than missing platform primitives?
- Which proposal states changed since the last source review?

## Immediate Next Step
Re-check the primary proposal statuses, then update only the claims whose source state changed.
```
