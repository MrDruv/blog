---
title: "What I Learned from Chapters 1 of *Grokking Simplicity*"
date: 2025-07-08
author: "MrDruv"
menu:
  main:
    name: "MrDruv-Blog"
    weight: 2
tags:
  [
    "functional programming",
    "software design",
    "grokking simplicity",
    "clean code",
  ]

categories: ["Programming", "Functional Thinking"]
summary: A practical and human-centered summary of the core ideas from the opening chapters of *Grokking Simplicity* by Eric Normand.
---

> Functional programming isn’t just about avoiding side effects—it’s about thinking clearly and writing code that behaves beautifully.

---

## Welcome to _Grokking Simplicity_

In the opening chapters of Eric Normand's _Grokking Simplicity_, we’re introduced to a more intuitive and practical approach to functional programming (FP)—one that trades dry theory for **real-world clarity**.

### Problem with Typical FP Definitions

Most FP definitions emphasize:

- No side effects
- Use of pure functions only

But in real life, software _needs_ side effects:

- We send emails
- Save data to databases
- Respond to button clicks

Normand argues that these definitions, while academically correct, aren’t useful for _working engineers_. Enter a more practical, clear-headed way to think functionally…

---

## Actions, Calculations, and Data: The Core Mental Model

This is the **first big idea** of the book. Here's how functional programmers mentally categorize code:

| Type             | Description                               | Examples                   |
| ---------------- | ----------------------------------------- | -------------------------- |
| **Actions**      | Depend on _when_ or _how often_ they run  | Send email, update DB      |
| **Calculations** | Deterministic computations based on input | Sum of list, string length |
| **Data**         | Immutable facts, easy to transmit         | JSON records, lists        |

By **identifying and separating** these concerns, we can:

- Write testable, reusable code
- Avoid mysterious bugs
- Architect more maintainable systems

**other examples to understand action, calculations and data**
| Code Task | Category | Why? |
|------------------------------|--------------|----------------------------------------------------------------------|
| `getCartTotal(cart)` | **Calculation** | Always returns the same total for the same cart |
| `sendConfirmationEmail(user)`| **Action** | Depends on when it’s sent — triggers external behavior |
| `user.email` | **Data** | A fact — doesn’t change unless explicitly updated |
| `addItem(cart, item)` | **Action** | If it mutates the cart directly, it’s an action |
| `addItemImmutable(cart, item)`| **Calculation** | Returns a new cart — no mutation, pure function |

---

## Functional Thinking in Action: Toni's Robot Pizza Shop

To bring the concepts alive, the book introduces **Toni**, a futuristic pizza shop owner using robots and functional code to manage her kitchen.

**Scenario**:

- 3 robots handle dough, sauce, and cheese in parallel.
- Without coordination, actions run out of order → chaos!

**Solution**: Introduce a “cut”—a synchronization point where robots wait for each other before proceeding. This ensures that prep steps finish **before** assembly starts.

This story cleverly visualizes FP principles in real-world operations, showing how **timeline diagrams** and **controlled coordination** bring order to distributed systems.

---

## Stratified Design

A smart way to organize code by **rate of change**:

- **Top Layer**: Business logic (changes frequently)
- **Middle**: Domain rules (pizza recipes, inventory handling)
- **Bottom**: Tech stack (arrays, JS syntax)

This structure helps developers:

- Isolate change
- Build on a stable foundation
- Avoid “spaghetti” dependencies

---

## Final Thoughts

The opening chapters of _Grokking Simplicity_ aren't about memorizing definitions—they’re about shifting your mental model.

Functional thinking isn’t limited to one language or paradigm. It’s a **pragmatic lens** that applies across all codebases and all scales—from a small function to a full-blown distributed system.

---

_Up next_: We dive deeper into refactoring actions into calculations and organizing code by layers to enhance testability and reuse.

Thanks for reading!
