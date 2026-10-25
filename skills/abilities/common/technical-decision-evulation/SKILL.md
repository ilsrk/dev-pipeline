---
name: technical-decision-evulation
description: >-
  Evaluate decisions by analyzing their pros, cons, advantages, and disadvantages. This is similar to SWOT analysis but specifically adapted for technical decisions. Use this approach whenever you need to assess a technical decision.
---

# Technical decision evulation

## Evaluation System

### Props and cons

Pros and cons are tactical considerations evaluated over the short term.

#### Props

**Positive factors** that can help us solve the problem and satisfy all requirements.

#### Cons

**Negative factors** that can prevent us from solving the problem and satisfying all requirements.

### Advantages and disadvantages

Advantages and disadvantages are strategic considerations evaluated over the long term.

#### Advantages

**Positive factors** that can help us maintain and scale the solution in the long term.

#### Disadvantages

**Negative factors** that can hinder our ability to maintain and scale the solution in the long term.

## Factor Weighting

- Assign each factor a weight between 0.1 and 1.0, in increments of 0.1.

## Factor Scoring

- Rate each factor on a scale from 1 to 5, where:
  - 1 = Minimal impact
  - 5 = Highest impact

## Steps

- Accept the technical decision from the user.
- Create a matrix of pros, cons, advantages, and disadvantages.
- Assign a weight to each factor.
- Evaluate each factor.
- Calculate the weighted total for each factor category.
- Calculate the overall result using the formula:
  - Overall Score = **Pros** − **Cons** + **Advantages** − **Disadvantages**
- Develop a strategy to mitigate negative factors.

