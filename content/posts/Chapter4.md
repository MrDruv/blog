---
title: "Extracting Calculations from Actions"
date: 2025-07-10
draft: false
---

hello folks,

## Introduction

In Chapter 4 of _Grokking Simplicity_, Eric Normand shows how messy, side-effect-heavy code can be transformed by **extracting pure calculations** from actions. This technique helps make your code easier to test, reuse, and reason about.

---

## Why Extract Calculations?

Actions are unpredictable—they depend on timing, external systems, and often mutate state. But **calculations** are pure: they take inputs and return outputs without side effects.

By pulling calculations out of actions, you:

- Make logic testable
- Reuse it in multiple places
- Avoid hidden dependencies

---

Lets dive into the Extraction of Calculation from Actions using following example.

## Example: Shopping Cart Total(Megamart)

Ex: Megarmart.com(Shopping Cart)
Below is the original Imperative code.

```
function calc_cart_total() {
shopping_cart_total = 0;
for(var i = 0; i < shopping_cart.length; i++) {
var item = shopping_cart[i];
shopping_cart_total += item.price;
}
set_cart_total_dom();
update_shipping_icons();
update_tax_dom();
}
```

Reusable is not posible in this case.
Reason:

- The code reads the shopping cart from a global variable, but they need to process orders from the database, not the variable.
- The code writes directly to the DOM, but they need to print tax receipts and shipping labels.

## We need Change this code to funtional

Suggessions to make it reusable:

- Separate business rules from DOM updates.
- Get rid of global variables.
- Return the answer from the function.

First step is to distinguish Actions,Calculations and data.This will give some idea of code.
Here the code is action.So lets apply funtional programming.

## Extracting a Calculation from an Action

Now lets dive into the main part of the chapter(i.e: Extraction).

First, we’ll isolate the calculation code, then convert its inputs and outputs to arguments and return values.

original

```
function calc_cart_total() {
shopping_cart_total = 0;
for(var i = 0; i < shopping_cart.length; i++) {
var item = shopping_cart[i];
shopping_cart_total += item.price;
}
set_cart_total_dom();
update_shipping_icons();
update_tax_dom();
}
```

We are creating new function.
Extracted

```
function calc_cart_total() {
calc_total();
set_cart_total_dom();
update_shipping_icons();
update_tax_dom();
}
function calc_total() {
shopping_cart_total = 0;
for(var i = 0; i < shopping_cart.length; i++) {
var item = shopping_cart[i];
shopping_cart_total += item.price;
}
}
```

This is function still an Action.
Lets turn it into a calculation.

The refactoring we just did might be called an extract subroutine.

To change new function into **Calculation**, it is important to identify inputs and outputs.
This function has two outputs and one input.
Output:

```
shopping_cart_total=0;
shopping_cart_total +=item.price;
```

input:

```
shopping_cart
```

```

function calc_cart_total() {
// Use return value to set the global variable
shopping_cart_total = calc_total();
set_cart_total_dom();
update_shipping_icons();
update_tax_dom();
}

function calc_total() {
// Convert it to a local variabe
var total = 0;
for(var i = 0; i < shopping_cart.length; i++) {
var item = shopping_cart[i];
total += item.price;
}
return total;
}
```

We pass shopping cart as an argument

```
function calc_cart_total() {
shopping_cart_total = calc_total(shopping_cart);
set_cart_total_dom();
update_shipping_icons();
update_tax_dom();
}
// Add an argument and use instead of global
function calc_total(cart) {
var total = 0;
for(var i = 0; i < cart.length; i++) {
var item = cart[i];
total += item.price;
}
return total;
}

```

At this point, **calc_total()** is a calculation. The only inputs and outputs are arguments and return values. And we’ve successfully extracted a calculation
