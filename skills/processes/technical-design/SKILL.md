---
name: technical-design
description: >-
  Creation, development, and writing of technical designs. Use when work involving technical design is required.
---

# Technical design

## Inputs data

- Task description
- Task requirements

## Conventions

- Name the technical design directory `technical-design`.

## Steps 

### 1. Initialize the artifacts.
  - Receive the input data from the user.
  - Analyze the input data.
  - Use the data schema provided by the user.
  - Create a technical design directory.
  - Create a blank technical design document.

### 2. Populate the artifact with the information already known.
  - Analyze the source data provided by the user. 
  - Populate the technical design artifact with this data.

### 3. Design the decision nodes (decision points).
  - Design system-level decision nodes.
  - Design application architecture-level decision nodes.
  - Design data design decision nodes.
  - Design observability decision nodes.

### 4. Validate the Decision Nodes
  - Assess the overall justification of the decision nodes (without designing the decisions themselves).

### 5. Design each decision collaboratively with the user, one at a time.
  - Design one decision per dialogue.
  - Present the user with decision options to choose from.
  - Record the decision in a Decision Record.

### 6. Validate each decision.

- Assess whether each decision is well-justified and, considering all relevant factors collectively, offers the greatest overall advantage compared with the alternatives.
