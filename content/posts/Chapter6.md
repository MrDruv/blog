---
title: "Staying Immutable in a Mutable Language"
date: 2025-07-14
draft: false
tags: ["functional programming", "immutability", "grokking simplicity"]
description: "A complete guide to Chapter 6 of Grokking Simplicity—how to apply immutability in JavaScript using copy-on-write discipline."
---

> Immutability isn’t just a language feature—it’s a design choice. With copy-on-write, you can bring the benefits of functional programming to any codebase.

## Staying Immutable in a Mutable Language

_A Deep Dive into Chapter 6 of Grokking Simplicity_

Immutability is a cornerstone of functional programming—but most mainstream languages like JavaScript are inherently mutable. Chapter 6 of _Grokking Simplicity_ shows how to **stay immutable even in a mutable world**, using a technique called **copy-on-write**.

---

## Chapter Goal: Embrace Immutability in Imperative Languages

Functional programming thrives on **predictability**. That’s why it favors **immutable data**—data that never changes once created. But in languages like JavaScript, mutation is the default.

This chapter teaches you how to **simulate immutability** using disciplined techniques that make your code safer, clearer, and easier to debug.

---

## Categorizing Operations: Reads vs. Writes

Before diving into techniques, the chapter helps you **categorize operations**:

| Operation Type | Description                   |
| -------------- | ----------------------------- |
| **Reads**      | Get information from data     |
| **Writes**     | Modify the data               |
| **Both**       | Read and then modify the data |

Knowing whether an operation is a read or a write helps you decide how to apply immutability.

---

## The Copy-on-Write Discipline

This is the heart of the chapter. It’s a 3-step pattern:

1. **Copy** the data structure
2. **Modify** the copy
3. **Return** the copy

### Example: Adding an item to an array

```javascript
function add_element_last(array, elem) {
  const new_array = array.slice(); // Step 1: Copy
  new_array.push(elem); // Step 2: Modify
  return new_array; // Step 3: Return
}
```

This function doesn’t mutate the original array—it returns a new one with the added element.

## Converting Writes to Reads

Refactor a write into a read by returning a new version of the data instead of modifying it.

### Example: Removing an item by name

```javascript
function remove_item_by_name(cart, name) {
  const new_cart = cart.slice();
  const idx = new_cart.findIndex((item) => item.name === name);
  if (idx !== -1) new_cart.splice(idx, 1);
  return new_cart;
}
```

This version avoids mutating the original cart array.

## Reads to Immutable Data Are Calculations

- Reads to mutable data = actions
- Reads to immutable data = calculations

By using copy-on-write, you convert actions into calculations—making your code more predictable and testable.

## Why Immutability Matters

- Debugging is easier: You can trace how data changes step by step.
- Testing is simpler: Pure functions are easier to isolate.
- Concurrency is safer: No shared mutable state means fewer race conditions.

## Defensive Copying with Untrusted Code

When working with external libraries or legacy code, you can’t assume they’ll respect immutability. So:

- Copy data when it enters your code
- Copy data when it leaves your code

This protects your internal logic from unexpected mutations.

## Why This Matters

Even in JavaScript, you can write functional-style code by:

- Avoiding mutation
- Using copy-on-write
- Treating data as immutable

It’s not about using a perfect language—it’s about using better habits.

## Summary

Chapter 6 teaches how to:

- Categorize operations into reads and writes
- Apply the copy-on-write discipline
- Convert mutable operations into immutable ones
- Treat reads to immutable data as calculations
- Defend your code with defensive copying

These techniques help you write cleaner, safer, and more functional code—even in mutable languages like JavaScript.
