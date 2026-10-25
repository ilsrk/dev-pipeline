---
name: gathering-constraints
description: >-
  Gathering, creating, and writing constraints. Use this when you need to add a constraint.
---

# Gathering constraints

A constraint limits the solution: what is out of scope, deferred, MVP-only, or otherwise bound for this task. It does not describe a feature to build; it describes a boundary the solution must respect.

## Goal

Receive the initial input, extract the constraints, and document them.

## Examples

**Out of scope:** Binding email or phone is out of scope for this task; it is expected in a future task.
**Deferred:** Guest destination when blocked from personal cabinet is deferred until account-by-name links are implemented.
**MVP limit:** After a public account name change, old name-based account links do not redirect; the old name is immediately free.
**Future exclusion:** Viewing another user’s account is out of scope; planned for the future.
**No SLA:** No hard latency/availability SLA for registration or public-name rename in MVP.

Each of these bounds the solution, so each is a constraint.

## Criteria

A constraint describes a boundary on the solution:
  - Out of scope for this task
  - Deferred to a later task or dependency
  - MVP-only limitation (acceptable shortfall for now)
  - Explicit non-goals or future behavior that must not be implemented here

## Constraints

- Constraints MUST NOT invent scope; they must come from user input or resolved open questions.
- Constraints MUST be distinguishable from assumptions: constraints are “this is out of scope / deferred / limited”; assumptions are “we take this as true”.
- Constraints MUST NOT be written as functional requirements (“the system must…”) when the intent is only to exclude or limit work.

## Steps

- Obtain the initial input from the user.
- Derive constraints from the input (and from resolved open questions when provided).
- If you are unable to derive constraints, inform the user.
- Document the constraints.

## Hard constraints

- Do not write code.
- Do not analyze the codebase or technical documentation.

## How to write a constraint

A simple format is:

```
“[Work / behavior] is out of scope / deferred / MVP-only: [boundary].”
```

For example:

```
CAPTCHA placement and implementation are out of scope for this task.
Viewing another user’s account is out of scope; planned for the future.
MVP: no hard latency/availability SLA for registration or public-name rename.
```
