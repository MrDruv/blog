---
title: "Functional Thinking: Distinguishing Actions, Calculations, and Data"
date: 2025-07-09
draft: false
---

> Functional programming isn’t just a coding style—it’s a way of thinking

## Introduction

Eric Normand introduces a powerful mental model that divides all code into three categories: **Actions**, **Calculations**, and **Data**.

In previous blog Welcome to _Grokking Simplicity_, the blog laid the foundation by shifting our perspective away from traditional programming paradigms and toward a simpler mental model. It challenged the reader to rethink complexity by introducing:

> The idea that programs should describe facts about events, not orchestrate step-by-step instructions.

> A powerful classification system for all code: Actions, Calculations, and Data.

> The notion that _Actions_ are infectious, spreading unpredictability through our systems, while _Calculations_ and _Data_ offer safety, testability, and clarity.

## Distinguishing Actions, Calculations, and Data

Understanding these three categories helps simplify your code and make it more predictable.

| Category        | Definition                                                          | Examples                                        |
| --------------- | ------------------------------------------------------------------- | ----------------------------------------------- |
| **Action**      | Depends on _when_ or _how often_ it's run; causes side effects      | Sending an email, reading a file, writing to DB |
| **Calculation** | Pure function: same input always gives same output; no side effects | `sum([1,2,3])`, validating an email address     |
| **Data**        | Immutable facts about events; inert and passive                     | User email, shopping list, API response         |

### Why It Matters

- **Actions** are unpredictable and hard to test.
- **Calculations** are safe, reusable, and testable.
- **Data** is the foundation—easy to store, compare, and interpret.

---

## Applying Functional Thinking to New Code

When starting fresh, functional thinking encourages you to design with clarity:

### Strategy

- **Model Data First**: Define the facts your system will work with.
- **Write Pure Calculations**: Functions that transform data without side effects.
- **Isolate Actions**: Keep side effects at the edges of your system.

### Example: Shopping Cart

| Code Task                      | Category    | Why?                                                   |
| ------------------------------ | ----------- | ------------------------------------------------------ |
| `getCartTotal(cart)`           | Calculation | Always returns the same total for the same cart        |
| `sendConfirmationEmail(user)`  | Action      | Depends on when it’s sent — triggers external behavior |
| `user.email`                   | Data        | A fact — doesn’t change unless explicitly updated      |
| `addItem(cart, item)`          | Action      | If it mutates the cart directly                        |
| `addItemImmutable(cart, item)` | Calculation | Returns a new cart without mutating the original       |

---

## Applying Functional Thinking to Existing Code

Refactoring legacy code becomes easier when you apply this model.

### Strategy

- **Identify Actions**: Look for time-sensitive or side-effecting logic.
- **Extract Calculations**: Refactor logic into pure functions.
- **Promote Data Structures**: Replace imperative logic with declarative data.

### Before and After Example

#### Before (Imperative)

```js
function updateUserProfile(userId, formData) {
  const user = db.getUser(userId);
  user.name = formData.name;
  user.email = formData.email;
  db.save(user);
}
```

**updateUserProfile(userId, formData) – Imperative, Action-Oriented**

This function:

- Fetches user data from the database: db.getUser(userId) -> Action
- Mutates the user object directly: user.name = ... -> Impure Calculation (mutation)
- Saves the updated object back to the database: db.save(user) -> Action

In functional terms:

- This function is action-heavy — it depends on external systems (database) and mutates existing data.
- It’s harder to test, since it has side effects and relies on context like a DB being available.

#### After (Functional)

```js
function updateProfile(user, formData) {
  return {
    ...user,
    name: formData.name,
    email: formData.email,
  };
}
```

**updateProfile(user, formData) – Pure, Functional**

This one:

- Takes in a user object and form data.
- Returns a new user object with updated name and email.

In functional terms:

- This is a pure calculation — no mutation, no database calls, no side effects.
- Easier to test and reuse, because you control the inputs and always get the same output.

## Final Thoughts

Introduces the core model: All code can be classified as either Actions, Calculations, or Data.

Defines each clearly:

> Actions depend on time or frequency and cause side effects.

> Calculations are pure functions—same input, same output, no side effects.

> Data is inert, immutable, and represents facts about events.

Explains how to identify each: Actions often hide other actions or calculations; calculations can be composed of smaller calculations and data; data only contains more data.

Emphasizes separation: Keeping these categories distinct makes code easier to test, debug, and reason about.

Functional thinking says: **Use calculations to prepare data → use actions only where effects are needed**.

Whether you're starting a new project or refactoring old logic, this mindset helps you design systems that are easier to reason about, more resilient to change, and elegantly simple.

Let your code describe facts about events, not control them.

## Up Next

We’ve learned how to see the three categories in our code. However, that is not enough. Functional programmers will want to transform code from actions to calculations in order to gain the benefits of calculations. We’ll see how in the next chapter.

Thanks for reading!
