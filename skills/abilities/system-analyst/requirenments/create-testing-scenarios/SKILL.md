---
name: create-testing-scenarios
description: >-
  Writing test scenarios for requirements. Use this when you need to write test scenarios for a requirement.
---

# Create testing scenarios

## Understand the requirement
First, read the requirement carefully and identify:

-What the system should do.
- Who will use the feature.
- Conditions that must be met.
- Expected result when the feature works correctly.
- Negative cases — what should happen when something goes wrong.

## Write test scenarios
A test scenario is a high-level statement describing what functionality you want to verify.

For the login requirement, you could write:

- Scenario ID
- Test scenario

TS-001 Verify that a user can log in with a valid username and password.

TS-002 Verify that login fails when the password is incorrect.

TS-003 Verify that login fails when the username is incorrect.

TS-004 Verify that login fails when both username and password are incorrect.

TS-005 Verify that login fails when the username field is empty.

TS-006 Verify that login fails when the password field is empty.

TS-007 Verify that the system displays an appropriate error message for invalid credentials.

TS-008 Verify that the user is redirected to the dashboard after successful login.

# Steps

1. Read the requirement.

Understand the feature and its business rules.

2. Identify test conditions

Consider valid inputs, invalid inputs, boundary values, permissions, and error handling where relevant.

3. Write test scenarios

Use clear statements beginning with “Verify that…” or “Check whether…”.

4. Check requirement coverage

Make sure every requirement and its important conditions are covered by one or more scenarios.