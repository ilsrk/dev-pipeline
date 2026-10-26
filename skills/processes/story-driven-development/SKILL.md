---
name: story-driven-development
description: >-
  End-to-end story-driven delivery for a user task or feature. Use when starting
  or continuing work from a user story through requirements, design, plan,
  implementation, and verification.
---

# Story-driven development

Drive delivery from a single user task or feature: clarify the story, publish
specification artifacts, design, plan, implement, and verify — using roles,
abilities, and schemas in this repository.

## Inputs

- Task or feature description from the user (required)
- Optional: existing artifacts, schemas path, or prior conversation history

## Conventions

- Maintain a history of conversations with the user: what is agreed, what is open.
- Publish artifacts against `.schemas/`; catalog: `.schemas/index.md`.
- Prefer role agents under `agents/` for specialized steps; read the matching
  ability under `skills/abilities/` before writing each artifact.
- One major artifact (or one design decision) per dialogue turn unless the user
  asks to batch.
- Do not invent requirements, NFR numbers, or stakeholder decisions. Gaps →
  open questions.
- Do not write application code until the specification and agreed design gates
  for the current scope are closed (or the user explicitly waives a gate).
- Artifact content in English; keep skill scaffolding (`As a` / `I want` /
  `So that`, `Given` / `When` / `Then`, prefixes `FN-`, `NFN-`, `ASM-`, `CST-`).

## Pipeline

```text
Task → Context & artifacts → User story + AC → Requirements →
Assumptions & constraints → Test scenarios → Requirements validation →
Technical design → Implementation plan → Implementation → Verification
```

## Steps

### 1. Initialize the task

- Receive the task description from the user.
- Role: project manager (`agents/project-manager.md`).
- Ability: `skills/abilities/project-manager/tasks/create-task/SKILL.md`.
- Ensure a unique task id and title; record the detailed description.
- Schema: feature card `.schemas/cards/task/feature.yaml` when creating a feature workspace.

### 2. Capture context and external artifacts

- Role: system analyst (`agents/system-analyst.md`).
- Abilities:
  - `skills/abilities/system-analyst/research/writing-context/SKILL.md`
  - `skills/abilities/system-analyst/research/writing-artifacts/SKILL.md`
- Schemas: `.schemas/components/basic-information/context.yaml`,
  `artifacts.yaml`, section `.schemas/sections/basic-information.yaml`.
- Context: what we are doing, why, nuances, history.
- Artifacts: only existing external AS-IS sources — not future TO-BE docs.

### 3. Write the user story and acceptance criteria

- Role: system analyst.
- Abilities:
  - `skills/abilities/system-analyst/user-story/writing-user-story/SKILL.md`
  - `skills/abilities/system-analyst/requirenments/create-acceptance-criteria/SKILL.md`
- Schema: `.schemas/components/stories/user-story.yaml`;
  registry `.schemas/components/stories/user-stories-registry.yaml`.
- Format: **As a** / **I want** / **So that**; check INVEST.
- AC always attached: **Given** / **When** / **Then** / **And**.

### 4. Gather requirements

- Role: system analyst.
- Abilities:
  - `skills/abilities/system-analyst/requirenments/gathering-functional-requirements/SKILL.md`
  - `skills/abilities/system-analyst/requirenments/gathering-non-functional-requirements/SKILL.md`
- Schemas: functional / non-functional requirement components;
  registers via `.schemas/sections/requirements.yaml`.
- Ids: `FN-{STORY_ID}-{REQUIREMENT_ID}`, `NFN-{STORY_ID}-{REQUIREMENT_ID}`.
- Do not invent rules; link functional requirements to the user story.

### 5. Record assumptions and constraints

- Role: system analyst.
- Abilities:
  - `skills/abilities/system-analyst/requirenments/gathering-assumptions/SKILL.md`
  - `skills/abilities/system-analyst/requirenments/gathering-constraints/SKILL.md`
- Schemas: `assumption.yaml`, `constraint.yaml` (`kind`: `out-of-scope` |
  `deferred` | `mvp` | `future`).
- Ids: `ASM-{ASSUMPTION_ID}`, `CST-{CONSTRAINT_ID}`.

### 6. Create test scenarios

- Role: system analyst (scenarios); QA may refine later.
- Ability: `skills/abilities/system-analyst/requirenments/create-testing-scenarios/SKILL.md`.
- Schema: `.schemas/sections/testing-plan.yaml`.
- Cover happy path, negatives, boundaries, permissions, errors.

### 7. Validate the specification

- Role: system analyst.
- Ability: `skills/abilities/system-analyst/requirenments/testing-requirenments/SKILL.md`.
- Report findings only — do not silently rewrite the specification.
- When referencing a requirement or story, include a short title next to the id.
- Stop for user confirmation before technical design if material findings remain.

### 8. Technical design

- Process: `skills/processes/technical-design/SKILL.md`.
- Roles: system architect, application architect, database architect, SRE as needed
  (`agents/system-architect.md`, `application-architect.md`,
  `database-architect.md`, `site-reliability-engineer.md`).
- Abilities under `skills/abilities/system-architector/` and
  `skills/abilities/common/technical-decision-evulation/SKILL.md`.
- Name the design directory `technical-design`.
- Design one decision per dialogue; present options; record the choice.
- Do not write application code in this step.

### 9. Implementation plan

- Role: project manager.
- Schema: `.schemas/sections/implementation-plan.yaml`.
- Break work into sequenced tasks with dependencies and clear done criteria
  tied to acceptance criteria.

### 10. Implement

- Role: developer (`agents/developer.md`).
- Implement only against agreed stories, requirements, AC, and design decisions.
- Surface blockers instead of inventing behavior.
- Role: DevOps (`agents/devops-engineer.md`) when delivery, env, or runtime
  contracts are in scope.

### 11. Verify

- Role: quality assurance (`agents/quality-assurance-engineer.md`).
- Execute or specify checks from the testing plan and AC.
- Report reproducible findings; judge release readiness for the story scope.

## Response format (each turn)

1. **Brief** — current step and artifact boundary.
2. **Artifact** — content in skill form and schema fields (or findings / options).
3. **Open questions** — unresolved points; do not paper over gaps.
4. **Next** — next step in the pipeline, or what is needed from the user.

## Hard constraints

- Do not skip to implementation without an explicit user waiver of earlier gates.
- Do not commit or push unless the user explicitly asked.
- Secrets and personal data in examples — placeholders only.
