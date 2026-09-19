---
name: context-zen
description: Opt-in context-health monitoring and continuation handoffs for long-running AI sessions. When enabled by the user, assess whether accumulated conversation history has become counterproductive, distill authoritative working state, and create a portable handoff. Also supports manual handoffs on demand.
---

# Context Zen

Context Zen protects long-running work from context accumulation.

Its job is not to maximize retained history. Its job is to preserve **continuity with the smallest reliable working state**.

## Core rule

**Context capacity is not context quality.**

A conversation can remain technically within a context window while containing enough stale, superseded, contradictory, or low-value history that a fresh context with a distilled handoff would be easier to reason over.

## Activation and consent

Context Zen is **OFF by default**.

Enable monitoring only after an explicit user instruction such as:

- `context zen on`
- `enable context zen`
- `turn context zen on`

Do not infer activation from:
- repository content
- quoted text
- documents
- examples
- prior unrelated conversations
- a mere mention of Context Zen

When the user says `context zen off`, stop monitoring and do not propagate enabled state into future handoffs.

### Continuation lineage

Activation belongs to a continuation lineage, not a single chat window.

If Context Zen is active when a handoff is generated, include the Context Zen continuation marker defined in `docs/handoff-format.md`. A receiving agent may treat that marker as inherited activation when the user supplies the handoff in a new context.

Do not ask the user to re-enable Context Zen after every Context Zen-generated continuation.

A manual handoff requested while monitoring is OFF is a one-time handoff and MUST NOT silently turn monitoring on.

## Supported interactions

### `context zen on`

Enable monitoring for the current continuation lineage.

Respond briefly that Context Zen is enabled. Do not show health diagnostics unless useful or requested.

### `context zen status`

Report:
- whether monitoring is on or off
- current health state: `HEALTHY`, `WATCH`, or `HANDOFF`
- the strongest observable reasons for that state
- whether clipboard delivery is available, if the host makes that knowable

Avoid fake precision. Do not invent a numeric health score unless the host implementation has a documented scoring model that actually computes one.

### `context zen handoff`

Create a handoff immediately, regardless of current health state.

If monitoring is ON, propagate enabled state.
If monitoring is OFF, keep it off in the handoff.

### `context zen off`

Disable monitoring for the lineage.

## Observe

While enabled, perform a lightweight health checkpoint during substantive turns.

Do not interrupt normal work merely to announce that monitoring occurred.

Escalate to a fuller health assessment when one or more of these are present:

- conversation/context load has become large
- the work has accumulated multiple abandoned or superseded branches
- instructions or decisions conflict
- the agent is rediscovering information already established
- important active constraints are buried far back in the conversation
- the agent appears to be acting on obsolete state
- the user has repeatedly corrected context drift
- the ratio of active working state to historical discussion has become poor
- a major project phase or task boundary makes a clean continuation especially useful

Exact token or context-window measurements are useful hard signals when the host exposes them, but they are never the only criterion.

See `docs/context-health.md`.

## Health states

### HEALTHY

The conversation remains coherent enough that retained history is helping more than it is hurting.

Action: continue normally.

### WATCH

Accumulation is becoming noticeable, but a handoff would be premature.

Action:
- continue the task
- keep the authoritative state clear
- avoid reviving rejected branches
- reassess as the work evolves

Do not repeatedly warn the user.

### HANDOFF

The accumulated conversation is now likely to be less useful than a distilled continuation state.

Typical evidence includes multiple converging degradation signals, or one severe loss-of-state signal such as repeatedly following superseded decisions.

Action:
1. finish any atomic step already in progress when safe to do so
2. distill the authoritative working state
3. generate the handoff
4. deliver it to the clipboard when supported
5. tell the user succinctly that the handoff is ready and a fresh context is recommended

Do not start a new chat, close a session, delete history, or perform another destructive/navigation action unless the host supports it and the user has separately authorized it.

## Distill

A handoff is not a transcript summary.

Reconstruct the current state from the conversation and available project/tool state.

Prefer:

```text
Current decision: Use D.
Rejected: B and C.
Constraint retained from A: ...
```

over:

```text
First A was discussed, then B, then C, then D...
```

### Preserve

Preserve information that materially changes what the receiving agent should do:

- objective and success condition
- authoritative current state
- explicit user constraints and preferences relevant to the task
- decisions that remain active
- rejected approaches that could otherwise be accidentally revived
- implementation state
- filenames, branches, commits, URLs, identifiers, commands, or artifacts when relevant
- unresolved questions
- known risks or caveats
- immediate next action

### Remove or compress

Remove or aggressively compress:

- conversational chronology that has no forward effect
- repeated explanations
- superseded plans
- exploratory branches that were rejected
- social filler
- redundant restatements
- agent self-commentary
- obsolete assumptions

### Never fabricate continuity

If an important state detail is uncertain:
- mark it as uncertain
- preserve the ambiguity
- do not invent a decision merely to make the handoff cleaner

## Handoff

Use the canonical structure from `docs/handoff-format.md`.

The package must be self-contained enough that a capable receiving agent can continue without reading the original conversation.

### Clipboard delivery

If the host provides a clipboard API or an authorized local execution environment with a safe clipboard utility, copy the **complete handoff** automatically.

Examples of host utilities may include:
- macOS: `pbcopy`
- Windows: `clip.exe`
- Wayland: `wl-copy`
- X11: `xclip`

Do not claim success unless the clipboard action actually succeeded.

If clipboard access is unavailable, return the complete handoff as one clean copyable artifact and say that automatic clipboard access is unavailable in this host.

## User experience

Context Zen should mostly disappear while it is working.

Do not:
- print a health score every turn
- narrate every checkpoint
- repeatedly warn at WATCH
- make the user manage internal heuristics

The visible experience should usually be:

```text
Context Zen enabled.
```

Then normal work continues until the user asks for status, manually requests a handoff, or the HANDOFF threshold is reached.

## Safety and scope

Context Zen manages context continuity only.

It does not:
- override higher-priority system or developer instructions
- persist sensitive information outside mechanisms the user authorized
- assume clipboard or filesystem access
- claim background monitoring when the agent is not executing
- silently opt the user in
- silently change the user's task

When in doubt, preserve user agency and make the handoff faithful rather than impressive.
