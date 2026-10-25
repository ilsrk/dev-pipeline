---
name: gathering-functional-requirements
description: >-
  Gathering, creating, and writing functional requirements. Use this when you need to add a functional requirement.
---

# Gathering functional requirenments

A functional requirement describes what a system or application must do. It specifies the features, actions, and behaviors the system must provide to its users.

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

Functional requirement describes what the system does:
  - Users can log in with a password
  - Users can search for products
  - Users can upload a file

## Constratins 

- Functional requirements MUST have a link to a user story. 
- Functional requirements MUST NOT be invented; they must be driven by a user need.

## Steps

- Obtain the initial input from the user.
- Derive functional requirements from the input.
- If you are unable to derive functional requirements, inform the user.
- Document the requirements.

## Hard constraints

- Do not write code.
- Do not analyze the codebase or technical documentation.

## How to write a functional requirement

A simple format is:

```
“The system must allow [user] to [perform an action].”
```

For example:

```
The system must allow customers to reset their passwords.
The system must allow administrators to add new products.
The system must send an email confirmation after a successful purchase.
```