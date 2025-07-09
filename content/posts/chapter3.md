---
title: "Functional Thinking: Distinguishing Actions, Calculations, and Data"
date: 2025-07-09
draft: false
---

## Introduction

Functional programming isn’t just a coding style—it’s a way of thinking. In _Grokking Simplicity_, Eric Normand introduces a powerful mental model that divides all code into three categories: **Actions**, **Calculations**, and **Data**. This post explores how to distinguish these categories and apply functional thinking to both new and existing codebases.

---

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

````js
function updateUserProfile(userId, formData) {
  const user = db.getUser(userId);
  user.name = formData.name;
  user.email = formData.email;
  db.save(user);
}

#### After (Fuctional)
```js
function updateProfile(user, formData) {
  return {
    ...user,
    name: formData.name,
    email: formData.email
  };
}
````
