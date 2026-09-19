# Context Health

Context Zen v0.1 uses a qualitative heuristic rather than a universal numerical score.

The goal is not to prove that a conversation is "bad." The goal is to decide whether **continuing from the full accumulated history is still a better working environment than continuing from a distilled state**.

## Two classes of signal

### Hard signals

These are measurable when a host exposes them:

- estimated or exact token/context load
- turn count
- number of attached files or active artifacts
- number of tool interactions
- conversation age
- distance in turns/tokens from important decisions

Hard signals are useful, but none automatically forces a handoff.

A very long conversation can remain coherent.

### Semantic signals

These require interpretation:

- **superseded-state density** — how much visible context describes plans or decisions that are no longer active?
- **instruction collision** — are old and new constraints simultaneously present in ways that can be confused?
- **branch accumulation** — how many abandoned explorations remain mixed into the active thread?
- **rediscovery** — is the agent repeatedly re-deriving or asking for facts already established?
- **state burial** — are important decisions difficult to distinguish from surrounding history?
- **obsolete-state behavior** — is the agent acting on a prior decision after the user changed it?
- **correction pressure** — is the user increasingly correcting memory/context drift?
- **relevance density** — how much of the retained conversation still materially affects the next action?
- **handoff advantage** — would a compact continuation package likely present the active state more clearly than the full transcript?

## States

### HEALTHY

Retained history is still mostly relevant and coherent.

Common characteristics:
- active decisions are clear
- few meaningful contradictions
- rejected branches are not influencing current work
- the agent can retrieve prior constraints reliably
- the current task can be understood without reconstructing the whole conversation

### WATCH

There are early signs of accumulation, but continuity still benefits from the current context.

Common characteristics:
- several old branches are visible
- active state requires some reconstruction
- conversation length is substantial
- important decisions are far back
- the project has changed direction multiple times

WATCH is intentionally quiet. It is a reason to reassess later, not a reason to nag the user.

### HANDOFF

A distilled state is likely to be a cleaner continuation substrate than the full history.

No single fixed token threshold defines this state.

Strong evidence can include:
- multiple semantic degradation signals converging
- repeated use of superseded state
- repeated user corrections caused by lost/buried decisions
- unresolved contradictions that can be cleanly resolved in a canonical handoff
- active working state representing only a small fraction of a very large conversation
- a natural project boundary where the next phase no longer benefits from most prior exploration

## Decision test

A useful final test is:

> If a fresh capable agent received a faithful, compact handoff of the current state, would it probably have an easier time continuing correctly than this agent has reasoning over the entire accumulated conversation?

If the answer is clearly yes, Context Zen should hand off.

## No fake precision

A future implementation may test weighted scoring, host telemetry, retrieval measurements, or benchmarked thresholds.

Until those exist, Context Zen should expose its reasoning qualitatively rather than presenting a number such as `63/100` as though it were scientifically calibrated.

## Calibration direction

Potential future evaluation could compare:

1. continuation accuracy from full long-context history
2. continuation accuracy from Context Zen distillation
3. loss of active constraints
4. revival of rejected decisions
5. user correction frequency
6. token cost and latency
7. task-specific success

That would turn the heuristic into something testable instead of merely intuitive.
