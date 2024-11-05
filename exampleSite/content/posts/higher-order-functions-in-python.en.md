---
author:
  name: "Thiago S. Teixeira"
date: 2023-10-01
linktitle: "Higher-Order Functions in Python"
type:
- post
- posts
tags: ["python", "lisp"]
title: "Higher-Order Functions in Python"
description: "Exploring the potential of higher-order functions in Python."
weight: 100
---

![header](/static/blog/higher-order-functions-in-python/header.jpg)

## Introduction

One of the most compelling aspects of functional programming is the concept of higher-order functions. In many scenarios, designing a function that generates other functions is an exceptional tool for refactoring and elevating our code's abstraction level. This approach is a stepping stone towards metaprogramming — the idea of writing programs that generate programs.

Though Python is not a purely functional programming language, it treats functions as first-class citizens. This means:

- Functions can be assigned to variables.
- Functions can be passed as arguments to other functions.
- Functions can be returned from other functions.

Let's dive into some illustrative examples:

## Functions as Arguments

Python is equipped with a functional programming library, and one of its common higher-order functions is the `map` function. The `map` function applies another function to all items in a given list. For instance:

```python
def square(x):
    return x*x

numbers = [1, 2, 3, 4]
squared_numbers = list(map(square, numbers))
print(squared_numbers)  # [1, 4, 9, 16]
```

In this example, `map` iterates over each item in the `numbers` list and applies the `square` function to each of them.

## Functions Returning Functions

This is where things get intriguing. Functions that return other functions allow for powerful and flexible patterns. Let's explore:

### Power Function Factory

Imagine you wish to create functions that raise numbers to various powers:

```python
def power_factory(n):
    def nth_power(x):
        return x ** n
    return nth_power

square = power_factory(2)
cube = power_factory(3)

print(square(4))  # 16 (4^2)
print(cube(2))    # 8 (2^3)
```

Here, `power_factory` returns a new function that raises its argument to the `n`th power. It's a dynamic way to create different power functions.

### Logger Functions

Functions that wrap around other functions to introduce logging functionality:

```python
def logger(fn):
    def wrapped(*args, **kwargs):
        print(f"Calling {fn.__name__} with {args} and {kwargs}")
        return fn(*args, **kwargs)
    return wrapped

@logger
def add(a, b):
    return a + b

print(add(3, 4))
# Output:
# Calling add with (3, 4) and {}
# 7
```

The `logger` function enhances the `add` function to print its call signature before executing.

### Shift Function Factory

This creates functions that add a fixed value:

```python
def shift_by(value):
    def shift_fn(x):
        return x + value
    return shift_fn

shift_by_5 = shift_by(5)
print(shift_by_5(3))  # 8
```

`shift_by` returns a new function that adds its argument to a pre-defined value.

### Function Composition

A function that takes two other functions and returns their composition:

```python
def compose(f, g):
    return lambda x: f(g(x))

def double(x):
    return 2 * x

def increment(x):
    return x + 1

double_then_increment = compose(increment, double)
print(double_then_increment(5))  # 11 (2*5 + 1)
```

Here, `compose` creates a new function that first doubles its argument and then adds 1.

These examples highlight the versatility and strength of functions returning other functions in Python. They can streamline our code, encapsulate specific logic patterns, and add dynamic behavior.
## Crafting a Lisp-Style Linked List in Python

The functional programming paradigm, with Lisp at its heart, showcases distinct techniques and approaches. Among Lisp's hallmark structures is the linked list, which diverges from the conventional ones seen in languages like Python or Java. In Lisp, each list adopts a recursive composition, bifurcated into `car` and `cdr`.

The first time I encountered this in the Lisp language, I was genuinely astounded. It felt as though I was instantiating lists out of thin air, relying solely on function definitions.

### Understanding `car` and `cdr`

1. **`car`**: Originating from "Contents of Address Register", it signifies the list's first element.
2. **`cdr`** (often pronounced 'cudder'): An abbreviation for "Contents of Decrement Register", it points to the list's remainder.

Simply put, a Lisp-style list is akin to Russian nesting dolls, where each `car` retains the value and the `cdr` encompasses the next nested pair.

### Emulating the List in Python

We can mirror this structure in Python, leveraging higher-order and lambda functions.

```python
# Define the 'pair' structure for our car and cdr
def cons(x, y):
    def dispatch(m):
        if m == 0:
            return x
        elif m == 1:
            return y
    return dispatch

# Extractors for car and cdr
def car(z):
    return z(0)

def cdr(z):
    return z(1)
```

In this setup:

- `cons` initiates our pair, essentially crafting our list node.
- `car` extracts the leading element of our pair.
- `cdr` fetches the succeeding one.

For constructing and exploring a Lisp-style list:

```python
# Constructing a list: (1, (2, (3, None)))
my_list = cons(1, cons(2, cons(3, None)))
```

To functionally print all elements:

```python
def print_list(lst):
    if lst:
        print(car(lst))
        print_list(cdr(lst))

print_list(my_list) 
# Output:
# 1
# 2
# 3
```


What's truly captivating about our Lisp-style list is its reliance on higher-order functions. Our `cons` function is designed to return another function (`dispatch`). This internal function carries the logic, deciding whether to offer the `car` or the `cdr`. Such abstraction makes list interactions smooth and exemplifies the magic of functions begetting functions.

## Conclusion

Higher-order functions possess immense power. Regrettably, they aren't widely adopted across all programming arenas and are primarily confined to the realm of functional programming. This situation evokes a sentiment that we might be missing out on a vast potential by sidelining functional programming approaches.

Working with higher-order functions prompts us to think differently, allowing us to perceive beyond the immediate code. Beyond its aesthetic appeal, it serves as an enriching exercise in nurturing a functional mindset.