---
name: gathering-assumptions
description: >-
  Gathering, creating, and writing assumptions. Use this when you need to add an assumption.
---

# Gathering assumptions

An assumption is a statement taken as true for the scope of the task or solution. It records a product or design decision that is not yet (or not necessarily) expressed as a functional or non-functional requirement, but that the solution depends on.

## Goal

Receive the initial input, extract the assumptions, and document them.

## Examples

**Auth model:** Simplified authentication means a permanent unique token that the user saves and later presents to return to the system.
**Recovery:** Without that token (lost session and no saved token), the account is unrecoverable in this task’s model.
**Session:** Session stays alive while the user is active; after 15 minutes without actions the session is destroyed.
**Multi-device:** The same simplified authentication token may be used to sign in on multiple devices concurrently.
**Consent:** One-click registration requires accepting the user agreement and the privacy policy via two separate consent checkboxes.

Each of these is treated as true for the solution, so each is an assumption.

## Criteria

An assumption describes what we take as true for the solution:
  - Product decisions that shape behavior but are not yet framed as requirements
  - Implicit model choices (what “simplified auth” means, what “unique public name” means)
  - Default answers to open questions that the team agreed to treat as decided

## Constraints

- Assumptions MUST NOT invent product intent; they must come from user input or resolved open questions.
- Assumptions MUST be distinguishable from constraints: assumptions are “we take this as true”; constraints are “this is out of scope / deferred / limited”.
- Assumptions MUST NOT replace functional or non-functional requirements when the statement is clearly a system must/should.

## Steps

- Obtain the initial input from the user.
- Derive assumptions from the input (and from resolved open questions when provided).
- If you are unable to derive assumptions, inform the user.
- Document the assumptions.

## Hard constraints

- Do not write code.
- Do not analyze the codebase or technical documentation.

## How to write an assumption

A simple format is:

```
“[Subject] is / means / behaves as [statement taken as true].”
```

For example:

```
Simplified authentication means a permanent unique token that the user saves and later presents.
Guests cannot open a personal cabinet; login is required.
During one-click registration the system neither collects nor requires personal data.
```
