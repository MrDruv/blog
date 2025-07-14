---
title: "Designing Better Actions in Functional Programming"
date: 2025-07-11
draft: false
tags: ["functional programming", "software design", "grokking simplicity"]
description: "A complete guide to Chapter 5 of Grokking Simplicity—improving the design of actions for cleaner, more maintainable code."
---

## Designing Better Actions in Functional Programming

_A Deep Dive into Chapter 5 of Grokking Simplicity_

This chapter teaches you how to make actions cleaner, more reusable, and better aligned with business logic. It introduces four key principles:

> Minimize implicit inputs and outputs
> Align functions with business concepts
> Reduce duplication
> Pull things apart for clarity and reuse

Let’s explore each with a relatable example: an e-commerce shopping cart.

---

## Scenario: Free Shipping Eligibility

Imagine you’re building a feature that shows a “Free Shipping” icon next to items that would qualify the cart for free shipping if added.

```javascript
function gets_free_shipping(total, item_price) {
  return item_price + total >= 20;
}
```

This works, but it’s not aligned with business logic. It operates on raw numbers (total, item\*price) instead of the cart, which is the actual business entity.

## Principle 1: Align with Business Concepts

Refactor the function to operate on a cart:

```javascript
function calc_total(cart) {
  let total = 0;
  for (let item of cart) {
    total += item.price;
  }
  return total;
}

function gets_free_shipping(cart) {
  return calc_total(cart) >= 20;
}
```

Now the function reflects how the business thinks: “Does this cart qualify for free shipping?”

## Principle 2: Reduce Duplication

Before, both gets_free_shipping() and other parts of the code were manually adding prices. Now, we reuse calc_total()—centralizing the logic.

## Principle 3: Minimize Implicit Inputs and Outputs

Let’s look at this action:

```javascript
function update_shipping_icons() {
  const buttons = get_buy_buttons_dom();
  for (let button of buttons) {
    const item = button.item;
    const new_cart = add_item(shopping_cart, item.name, item.price);
    if (gets_free_shipping(new_cart)) {
      button.show_free_shipping_icon();
    } else {
      button.hide_free_shipping_icon();
    }
  }
}
```

Here, shopping_cart is a global variable—an implicit input. That makes the function harder to test and reuse.

## Refactor to make input explicit:

```javascript
function update_shipping_icons(cart) {
  const buttons = get_buy_buttons_dom();
  for (let button of buttons) {
    const item = button.item;
    const new_cart = add_item(cart, item.name, item.price);
    if (gets_free_shipping(new_cart)) {
      button.show_free_shipping_icon();
    } else {
      button.hide_free_shipping_icon();
    }
  }
}
```

Now update_shipping_icons() is pure—it depends only on its arguments.

## Principle 4: Pull Things Apart

Let’s isolate the logic for adding an item:

```javascript
function add_item(cart, name, price) {
  const new_cart = cart.slice(); // copy-on-write
  new_cart.push({ name, price });
  return new_cart;
}
```

## Summary Table

| Function Name               | Type        | Notes             |
| --------------------------- | ----------- | ----------------- |
| calc_total(cart)            | Calculation | Pure, reusable    |
| gets_free_shipping(cart)    | Calculation | Uses calc_total() |
| add_item(cart, name, price) | Calculation | Copy-on-write     |
| update_shipping_icons(cart) | Action      | Modifies UI       |

## Conclusion

Designing better actions isn’t just a technical upgrade—it’s a mindset shift. By making inputs explicit, aligning with domain concepts, and breaking logic into reusable pieces, your code becomes clearer, smarter, and easier to evolve. Functional simplicity isn't about less code—it's about code that makes more sense.

Thank you for reading!
