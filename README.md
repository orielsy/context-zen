# Context Zen

**Context Zen** is an experimental context-engineering skill for long-running AI sessions.

Its premise is simple:

> **Context capacity is not context quality.**

A conversation can still fit inside a model's context window while becoming harder to reason over cleanly. Old branches, superseded decisions, buried constraints, repeated explanations, and accumulated conversational history can eventually become more distracting than useful.

Context Zen is designed to notice that transition and create a clean continuation handoff before accumulated history becomes a liability.

## What it does

Context Zen has two core behaviors:

1. **Observe** — after the user explicitly enables it, the skill watches the active conversation for signs of context degradation during normal turns.
2. **Handoff** — when degradation crosses the skill's threshold, or when the user asks for one manually, Context Zen distills the current working state into a continuation package.

The handoff preserves what the next context needs and intentionally drops conversational entropy.

When the host exposes clipboard access, Context Zen copies the generated handoff automatically. When clipboard access is unavailable, it returns one clean, self-contained block for the user to copy.

## Activation

Context Zen is **off by default**. It never silently opts a user into monitoring.

Canonical interactions:

```text
context zen on
context zen status
context zen handoff
context zen off
```

Activation follows the **continuation lineage**, not one chat window. If Context Zen is active when it creates a handoff, the handoff carries that state forward so the receiving context can continue monitoring without making the user re-enable it after every successful handoff.

A manual handoff does not implicitly enable monitoring. If Context Zen was off before a one-time handoff, it remains off.

## Why distillation instead of summarization?

A chronological summary often preserves too much history:

```text
The user suggested A.
The agent proposed B.
The user rejected B.
The agent modified B into C.
The user changed C to D.
```

A continuation handoff should instead reconstruct the authoritative state:

```text
Current decision: D
Rejected: B and C
Relevant constraint from A: ...
Next action: ...
```

Context Zen calls this **context distillation**: reconstructing the smallest reliable working state needed to continue.

## Context health

v0.1 intentionally uses a transparent heuristic rather than pretending there is a universal numerical score.

It considers signals such as:

- accumulated conversation/context load
- stale or superseded branches
- conflicting instructions or decisions
- repeated rediscovery of facts already established
- important constraints buried far from the active work
- divergence between conversation history and current working state
- signs that the agent is following obsolete state or asking for known information
- whether a compact handoff would likely be easier to reason over than the full history

See [docs/context-health.md](docs/context-health.md).

## Handoff shape

A Context Zen handoff prioritizes:

- objective
- authoritative current state
- decisions made
- user constraints and preferences relevant to the work
- rejected approaches that must not be accidentally revived
- active artifacts, files, branches, commits, or resources
- unresolved questions
- immediate next action
- only the historical context that materially affects continuation

See [docs/handoff-format.md](docs/handoff-format.md).

## Portability

Context Zen is written as a host-agnostic skill. Some hosts can expose token counts, persistent state, shell access, or a clipboard API; others cannot.

The skill treats those capabilities as optional:

- use exact context/token information when the host exposes it
- otherwise reason from observable conversation structure
- use clipboard access when available
- otherwise return a single copyable handoff
- never assume it can start a new conversation automatically
- never claim to monitor while the agent is not executing

"Automatic" in Context Zen means the skill reassesses health during the normal turn loop once enabled. It is not a background daemon unless a particular host explicitly provides that capability.

## Status

**v0.1 — Experimental**

The core behavior is defined, but the context-health heuristic is deliberately provisional. The project is meant to make that heuristic inspectable and improvable rather than hide it behind a false precision score.

## Repository structure

```text
.
├── README.md
├── SKILL.md
├── docs/
│   ├── context-health.md
│   ├── handoff-format.md
│   └── lifecycle.md
└── examples/
    ├── coding-handoff.md
    └── research-handoff.md
```

## Research

The companion research note lives on [orielsy.com](https://orielsy.com/research/context-zen-when-more-context-stops-helping/).

---

Created by [Orielsy Diaz](https://orielsy.com/).
