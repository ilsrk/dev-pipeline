---
name: create-acceptance-criteria
description: >-
  
---

# Create acceptance criteria

## Acceptance Criteria

### The standard format

~~~
**Given** — Initial condition or context.
**When** — Action performed by the user.
**Then** — Expected result.
**And** — An additional condition, action, or expected result related to the preceding step.
~~~

### Examples

~~~
AC1: Add a product to the shopping cart

**Given** the customer is viewing a product that is available for purchase
**And** the product has a valid price
**When** the customer clicks the “Add to Cart” button
**Then** the system should add the selected product to the shopping cart
**And** display a confirmation message that the product was added successfully
**And** update the shopping cart item count
~~~

~~~
AC2: Add multiple different products

**Given** the customer has already added a product to the shopping cart
**When** the customer adds another different product
**Then** the system should add the new product to the cart
**And** retain the previously added product
**And** update the cart total and item count accordingly
~~~

### Quality Checklist

Good acceptance criteria should be:
- **Clear** — Everyone interprets them the same way.
- **Complete** — Important scenarios and business rules are covered.
- **Testable** — QA can determine whether each criterion passes or fails.
- **Consistent** — They align with the user story and do not conflict with other requirements.
- **Observable** — The expected outcome can be verified.
- **Agreed upon** — Business and delivery stakeholders understand the expected behavior.
