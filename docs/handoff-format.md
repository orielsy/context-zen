# Handoff Format

A Context Zen handoff is a **continuation package**, not a conversation summary.

It should reconstruct the authoritative state required to continue the work with minimal conversational residue.

## Continuation marker

Place this marker at the top of a Context Zen-generated handoff:

```yaml
---
context_zen:
  version: "0.1"
  continuation: true
  enabled: true
---
```

Set `enabled: true` only when Context Zen monitoring was active before the handoff.

For a one-time manual handoff created while Context Zen was off:

```yaml
---
context_zen:
  version: "0.1"
  continuation: true
  enabled: false
---
```

The receiving agent may use `enabled: true` as inherited activation for that continuation lineage.

## Canonical structure

Use only sections that materially help continuation.

```markdown
# Context Zen Handoff

## Objective
The outcome the user is trying to achieve.

## Current State
The authoritative state right now.

## Decisions in Force
- Active decision
- Active decision

## Constraints / Preferences
- Constraint that changes implementation or reasoning
- Relevant user preference

## Rejected / Superseded
- Rejected approach — why it should not be revived
- Superseded decision — what replaced it

## Active Artifacts
- Repository / branch / commit
- Files
- Documents
- URLs
- Tool state
- Other identifiers needed to continue

## Open Questions
- Unresolved question or ambiguity

## Immediate Next Step
The most useful next action.

## Important Historical Context
Only history whose absence would likely cause a wrong decision.
```

## Rules

### Be authoritative

Prefer the latest explicit user decision over earlier brainstorming.

When tool/project state is available, prefer current source state over stale conversational descriptions.

### Be compact, not lossy

Do not optimize for shortest possible output.

Optimize for the smallest package that preserves correct continuation.

### Preserve negative knowledge

Rejected approaches matter because a fresh agent can otherwise independently propose them again.

Keep rejected items when:
- the user strongly ruled them out
- the rejection reveals an important constraint
- the alternative is likely to be rediscovered

### Preserve uncertainty

Do not resolve ambiguity by guessing.

Use explicit language such as:
- `Unconfirmed:`
- `Unknown:`
- `The conversation contains conflicting statements about...`

### Avoid transcript narration

Bad:

```text
The user first asked about X. Then we talked about Y. After that...
```

Better:

```text
Current implementation: Y.
X was rejected because ...
```

## Target-aware handoffs

A user may specify a destination.

Examples:

```text
context zen handoff for codex
context zen handoff for research
context zen handoff for another chatgpt session
```

Keep the same underlying truth, but emphasize the state most useful to the target.

### Coding target

Emphasize:
- repository and branch
- changed files
- current architecture
- commands/tests
- known failures
- next code change
- do-not-change constraints

### Research target

Emphasize:
- research question
- established findings
- source/evidence state
- assumptions
- unresolved questions
- next investigation

Target awareness may change emphasis, never facts.

## Clipboard

The clipboard payload must contain the complete handoff, including the continuation marker.

Do not copy only a shortened summary while displaying a fuller handoff.
