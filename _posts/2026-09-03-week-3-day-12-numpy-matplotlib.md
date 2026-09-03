---
title: "Week 3, Day 12 — NumPy and Matplotlib: Arrays, dtypes, Views, and Plots"
date: 2026-09-03 00:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, python, numpy, matplotlib]
math: true
---

Day 12 opens Week 3 — Python for ML — and leaves the pure math behind for the
tools. First up: **NumPy**, the array library everything numerical in Python
sits on, and **Matplotlib** for plotting. These are my notes, cleaned up.

## NumPy: Numerical Python

NumPy's whole reason to exist is the **array** — a grid of numbers that
behaves like the vectors and matrices from Week 1, but backed by fast,
compiled code instead of Python lists.

`np.array` converts a Python list into a NumPy array:

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
```

Two documentation habits I picked up on day one, because you look things up
constantly:

```python
help(np.arange)   # prints the full docstring for a function
np.arange?        # same thing, IPython/Jupyter shortcut
```

### Indexing and slicing

Indexing works like Python lists, including **negative indices** — `-1` is the
last element, `-2` the second-to-last:

```python
arr[-1]      # 5, the last element
arr[3:4]     # slice: array([4])
arr[::-1]    # reversed: array([5, 4, 3, 2, 1])
```

### Arithmetic is element-wise

The point of arrays is that operations apply to **every element at once**, no
loop required — addition, subtraction, multiplication, division, and
exponentiation with `**`:

```python
arr + 10     # adds 10 to every element
arr ** 2     # squares every element
np.around(arr / 3)   # divide, then round
```

This is the "vectorization" idea: you write the operation once and NumPy runs
it in compiled code over the whole array.

## Data types (and a sharp edge)

Every NumPy array has a **dtype** — the type each element is stored as. Types
like `int16`, `int32`, `float32`, `complex64` differ in **how many bits** each
number takes. You can set it explicitly with `dtype`:

```python
small = np.array([1, 2, 3], dtype=np.int8)
```

The gotcha I want to remember: a small integer type has a limited range, and
NumPy **wraps around (overflows) silently** instead of erroring. An `int8`
holds $-128$ to $127$, so pushing past the top loops back to the bottom:

```python
np.array([100], dtype=np.int8) + 100   # -> array([-56]), not 200
```

$100 + 100 = 200$ doesn't fit in an `int8`, so it wraps around into the
negatives. No exception, no warning — just a wrong number. This is exactly the
kind of bug that hides until your values happen to get large.

## Sorting: method vs function

NumPy gives you two ways to sort, and the difference matters:

- **`arr.sort()`** — a _method_ that sorts **in place**. It mutates the array
  and returns nothing (`None`), so you just call it; you don't assign it.
- **`np.sort(arr)`** — a _function_ that **returns a new sorted array** and
  leaves the original untouched.

```python
arr.sort()          # arr is now sorted; return value is None
new = np.sort(arr)  # new is sorted; arr is unchanged
```

Assigning `x = arr.sort()` is the classic mistake — `x` ends up `None`.

## Views vs copies

This one is subtle and a common source of bugs. Slicing an array gives you a
**view** — a window onto the _same underlying data_, not a fresh array. Change
the view and you change the original:

```python
view = arr[1:4]     # a view — shares memory with arr
view[0] = 999       # this also changes arr!
```

A **copy** is independent. If you want to modify without touching the original,
copy it explicitly:

```python
independent = arr.copy()
independent[0] = 999   # arr is untouched
```

The mental rule: **slicing = view (shared)**, **`.copy()` = copy
(independent)**. When in doubt about whether a change will leak back, copy.

## Aggregations and math constants

Aggregate functions collapse an array down to a summary — the expectation and
variance ideas from Day 11 live here in code:

```python
np.sum(arr)    # total
np.mean(arr)   # average
np.prod(arr)   # product of all elements
```

NumPy also ships the math constants and functions, so you rarely need the
`math` module:

```python
np.pi          # 3.14159...
np.e           # Euler's number, 2.71828...
np.cos(arr)    # cosine, element-wise
np.log(arr)    # natural logarithm (base e)
```

## Matplotlib: plotting

`matplotlib.pyplot` is the standard plotting interface, imported as `plt` by
convention. The basic line plot takes **x values first, then y values**, and
nothing shows until you call `plt.show()`:

```python
import matplotlib.pyplot as plt

plt.plot(x_values, y_values)
plt.show()
```

`np.linspace` pairs naturally with plotting — it generates evenly spaced points
between a start and an end, and you say **how many** points you want (unlike
`np.arange`, where you give the step):

```python
x = np.linspace(0, 2 * np.pi, 100)   # 100 points from 0 to 2π
y = np.sin(x)
plt.plot(x, y)
plt.xlabel("x")
plt.ylabel("sin(x)")
plt.show()
```

Other plot types are the same shape, just a different function:

```python
plt.bar(categories, heights)   # bar chart
plt.scatter(x, y)              # scatter plot
```

## Takeaway

This is the day the math from Weeks 1 and 2 becomes something I can actually
_run_. A vector is an `np.array`; element-wise arithmetic is vectorization;
aggregations are the mean and variance from probability; `np.linspace` + `sin`

- `plt.plot` is how you _see_ a function instead of just deriving it.

Two things I'll carry as hazards, because both fail silently: **integer
overflow** from an undersized dtype, and **views sharing memory** with the
array they were sliced from. Neither raises an error — they just hand you a
wrong number or a mutation you didn't intend. Knowing they exist is most of
avoiding them.

## Sources

Udemy — [Scientific Computing with NumPy](https://www.udemy.com/course/scientific-computing-with-numpy/),
plus the official docs for reference:

1. [NumPy quickstart](https://numpy.org/doc/stable/user/quickstart.html)
2. [Matplotlib pyplot tutorial](https://matplotlib.org/stable/tutorials/pyplot.html)
