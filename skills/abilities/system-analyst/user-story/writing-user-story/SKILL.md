---
name: writing-user-story
description: >-
  Create, write, or generate a user story. Use this when starting a new user story.
---

# Writing user story

## Overview

**A User Story is** a short description of a feature or requirement written from the perspective of the person who will use it. It explains who wants something, what they want, and why they want it.

**Acceptance criteria** are the specific conditions that must be satisfied for a user story to be considered complete.

## Story Format

### The standard format

~~~
**As a** [type of user], 
**I want** [a feature or action], 
**So that** [a benefit or goal].
~~~

### Examples

~~~
**As a** customer,
**I want** to add products to my shopping cart,
**So that** I can purchase multiple products in one order.
~~~

Here:
- Who? Customer
- What? Add products to the cart
- Why? Purchase multiple products in one order

### Quality Checklist (INVEST)

**Independent**

Can the story be developed and tested without depending heavily on another story?
Example: Adding a product to the cart should be a distinct story from making a payment.

**Negotiable**

Can the implementation details be discussed and refined with the team?
Example: The team can discuss how the cart icon displays the number of items.

**Valuable**

Does the story provide clear value to the customer or business?
Example: Customers can collect products before purchasing them.

**Estimable**

Does the development team have enough information to estimate the effort?
Example: The team understands the expected behavior when adding an available product to the cart.

**Small**

Is the story small enough to complete within one sprint, according to the team's working practices?
Example: Start with adding products to the cart instead of combining the cart, checkout, payment, and order tracking into one story.

**Testable**

Can QA verify whether the story meets its acceptance criteria?
Example: When a customer adds an available product, the cart displays that product and the correct quantity.

## Steps 

- Refine the user story.
- Refine the acceptance criteria.

## Requirenments

- Acceptance criteria should always be attached to the user story.
