# Lifecycle

Context Zen is deliberately opt-in.

The user activates it once for a chain of continued work. Successful handoffs should not force the user to repeat activation.

## State model

```text
                    explicit user opt-in
                           │
                           ▼
                         ON
                           │
                normal turn-loop checks
                           │
              ┌────────────┴────────────┐
              │                         │
           HEALTHY                    WATCH
              │                         │
              └────────────┬────────────┘
                           │
                     threshold crossed
                           ▼
                        HANDOFF
                           │
                      DISTILL STATE
                           │
                  COPY / RETURN PACKAGE
                           │
                           ▼
                    NEW CONTEXT
                           │
                 inherited enabled=true
                           │
                           ▼
                         ON
                           │
                    ...until user says...
                           ▼
                          OFF
```

## Activation follows the work

The durable unit is the **continuation lineage**, not the chat window.

A lineage is the conceptual chain:

```text
Chat A -> Context Zen handoff -> Chat B -> Context Zen handoff -> Chat C
```

When monitoring is ON, a generated handoff carries `context_zen.enabled: true`.

When the user pastes that handoff into a fresh context, the receiving agent can continue monitoring without asking the user to opt in again.

## Why this matters

If activation were scoped only to one conversation, Context Zen would disable itself every time it successfully did its job:

```text
enable
...long session...
handoff
new chat
enable again
...long session...
handoff
new chat
enable again
```

That creates repetitive housekeeping.

The lineage model instead means:

```text
enable once
...handoff...
...handoff...
...handoff...
disable when desired
```

## One-time manual handoff

A user can request `context zen handoff` while monitoring is OFF.

That request authorizes one distillation/handoff operation. It does not imply continuing monitoring.

The outgoing marker must therefore preserve `enabled: false`.

## What activation authorizes

Turning Context Zen on authorizes the skill to:
- assess context health during normal execution
- generate a continuation handoff when the HANDOFF state is reached
- copy that handoff to the clipboard when an available host capability permits it
- propagate the enabled state through Context Zen continuation markers

It does not authorize the skill to:
- start or close conversations
- delete history
- write unrelated files
- change the user's task
- send data to third parties
- claim continuous/background execution that the host does not provide

## Host persistence

Persistent host state is optional.

If a host has durable skill/session state, it may store Context Zen activation there.

If it does not, the continuation marker in the handoff acts as the portable persistence mechanism.
