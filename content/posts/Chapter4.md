---
title: "Extracting Calculations from Actions"
date: 2025-07-10
draft: false
---

# Hello folks

## Introduction

In Chapter 4 of _Grokking Simplicity_, Eric Normand demonstrates how side-effect-heavy code can be simplified by **extracting pure calculations from actions**. This powerful technique leads to cleaner, testable, and reusable code — and it's a key step toward thinking functionally.

---

## Why Extract Calculations?

**Actions** are unpredictable:

- They depend on external systems (like databases or the DOM)
- They can fail
- They can change over time

In contrast, **calculations** are **pure**:

- They take inputs
- Return outputs
- And have **no side effects**

Extracting calculations helps you:

- Make logic **testable**
- **Reuse** code in multiple places
- **Isolate complexity** and avoid hidden dependencies

---

## Example: Shopping Cart Total at MegaMart

Let’s explore this idea with a classic example — calculating the total of a shopping cart.

### Original Imperative Code

```
function calc_cart_total() {
  shopping_cart_total = 0;
  for (var i = 0; i < shopping_cart.length; i++) {
    var item = shopping_cart[i];
    shopping_cart_total += item.price;
  }
  set_cart_total_dom();
  update_shipping_icons();
  update_tax_dom();
}
```

Problems with This Code

- Uses global variables (shopping_cart, shopping_cart_total)
- Directly interacts with the DOM
- Hard to test or reuse for other use cases (e.g., printing receipts)

## Step 1: Extract a Subroutine

First, we extract the logic that calculates the total:

```
function calc_cart_total() {
  calc_total();
  set_cart_total_dom();
  update_shipping_icons();
  update_tax_dom();
}

function calc_total() {
  shopping_cart_total = 0;
  for (var i = 0; i < shopping_cart.length; i++) {
    var item = shopping_cart[i];
    shopping_cart_total += item.price;
  }
}

```

This is cleaner, but calc_total() still mutates global state, so it’s still an action.

## Step 2: Identify Inputs and Outputs

To make calc_total() a **pure calculation**, we must:

- Eliminate side effects (no DOM, no globals)
- Make inputs explicit (cart)
- Use return values instead of modifying external state

Step 3: Refactor Into a Calculation

```
function calc_cart_total() {
  shopping_cart_total = calc_total(shopping_cart); // use return value
  set_cart_total_dom();
  update_shipping_icons();
  update_tax_dom();
}

function calc_total(cart) {
  var total = 0;
  for (var i = 0; i < cart.length; i++) {
    var item = cart[i];
    total += item.price;
  }
  return total;
}
```

Now calc_total(cart) is a **pure function**:

- no implicit inputs or outputs
- No side effects
- Easy to test!
- Easy to reuse

Recap: What Did We Learn?

- Identify parts of your actions that are actually calculations
- Extract those parts into pure functions
- Test the calculations in isolation
- Reuse them anywhere without side effects

Up Next...
Improving th design of actions....stay tuned.

Thanks for reading!
