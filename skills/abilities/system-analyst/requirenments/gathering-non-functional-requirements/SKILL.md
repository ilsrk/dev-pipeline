---
name: gathering-non-functional-requirements
description: >-
  Gathering, creating, and writing non-functional requirements. Use this when you need to add a non-functional requirement.
---

# Gathering non-functional requirenments

Non-functional requirements describe how a system should work, rather than what the system should do.

They define the system's quality, performance, security, reliability, and usability.

## Goal

Receive the initial input, extract the requirements, and document them.

## Examples

**Login:** The system must allow users to log in using their email and password.
**Registration:** The system must allow new users to create an account.
**Search:** The system must allow users to search for products by name.
**Add to cart:** The system must allow users to add products to their shopping cart.
**Payment:** The system must allow users to pay for their orders.
**Order tracking:** The system must allow users to view their order status.

Each of these describes something the system must do, so each is a functional requirement.

## Criteria

Describe how well the system works. Focus on quality and constraints:
  - Example: Login must complete within 2 seconds
  
## Constratints 

- Functional requirements MUST have a link to a user story. 
- Functional requirements MUST NOT be invented; they must be driven by a user need.

## Steps

- Obtain functional requirements from the user.
- Derive non-functional requirements from them.
- Document the requirements.

## Hard constraints

- Do not write code.
- Do not analyze the codebase or technical documentation.

## What is non-functional requirenments

**Performance** - The system should load a page within 2 seconds.
**Security** - User passwords must be stored securely using strong password hashing.
**Reliability** - The system should operate without unexpected failures.
**Scalability** - The system should support 10,000 concurrent users without significant performance degradation.
**Usability** - A new user should be able to complete registration without assistance.
**Availability** - The system should be available 99.9% of the time.
**Maintainability** - Developers should be able to update a module without affecting unrelated modules.

## Why are NFRs important?

- They improve user experience.
- They protect data and privacy.
- They ensure the system can handle growth.
- They reduce downtime and failures.
- They help developers and testers measure system quality.

## How to write a functional requirement
