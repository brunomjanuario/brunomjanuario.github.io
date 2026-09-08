---
title: "Week 3, Day 13 — NumPy: Randomness, Statistics, and Matrices"
date: 2026-09-07 00:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, python, numpy, statistics]
math: true
---

Day 13 continues Week 3 with the rest of NumPy: generating **random numbers**,
the **statistics** functions that put Day 11's probability into code, and
working with **matrices** — 2D arrays, their shapes, and boolean masks. These
are my notes, cleaned up.

## Random numbers

Modern NumPy generates randomness through a **generator object** rather than
module-level functions. You create one with `default_rng`, then call methods
on it:

```python
from numpy import random

rng = random.default_rng()
```

That `rng` object is the source of everything random. The methods I used:

```python
rng.integers(start, end)   # random integer in a range
rng.random(size)           # array of random floats
rng.normal()               # draw from a normal distribution
```

`rng.normal()` is the direct link back to Day 11 — it samples from the bell
curve the Central Limit Theorem keeps producing.

Two more that operate on an existing array:

```python
rng.shuffle(arr)              # shuffles the order of elements, in place
rng.choice(arr, size=3)       # picks 3 elements from arr at random
```

Worth noting `rng.shuffle` follows the same in-place pattern as `arr.sort()`
from Day 12 — it mutates the array and returns nothing, so you call it rather
than assign it. `rng.choice`, by contrast, returns a new array of picks.

## Statistics

These are the summary numbers from Day 11's probability, now as one-liners
over an array:

```python
np.mean(arr)     # average — the expectation
np.median(arr)   # middle value when sorted
np.std(arr)      # standard deviation — the spread
np.var(arr)      # variance
```

The relationship between the last two is worth stating explicitly, because
it's the definition rather than a coincidence:

$$
\text{var} = \text{std}^2 \qquad\Longleftrightarrow\qquad \text{std} = \sqrt{\text{var}}
$$

So `np.var(arr)` equals `np.std(arr) ** 2`. Variance is the average squared
deviation from the mean (Day 11), which makes it live in *squared* units —
standard deviation is the square root that brings it back to the same units as
the data, which is why it's the one people quote.

And for seeing what's actually in a dataset:

```python
np.unique(arr)   # every distinct value, sorted
```

## Matrices

A matrix is just a **2D array** — a list of lists handed to `np.array`:

```python
matrix = np.array([[1, 2, 3],
                   [4, 5, 6]])
```

To go back to plain Python (useful for printing or JSON), `.tolist()` returns
a real list of lists instead of a NumPy object:

```python
matrix.tolist()   # [[1, 2, 3], [4, 5, 6]]
```

### Indexing a matrix

Indexing now takes two positions — rows first, then columns. A `:` means "all
of them", which is how you grab a whole column:

```python
matrix[:, 2]   # every row, column index 2 -> array([3, 6])
matrix[1, :]   # row index 1, every column -> array([4, 5, 6])
```

### Inspecting shape

Three attributes describe an array's structure, and they answer different
questions:

```python
matrix.ndim    # 2 — number of dimensions
matrix.shape   # (2, 3) — (rows, columns)
matrix.size    # 6 — total number of elements
```

`ndim` is how many axes there are, `shape` is how long each axis is, and
`size` is the product of the shape. A 1D vector has `ndim == 1`; a matrix has
`ndim == 2`.

### Reshaping

`reshape` rearranges the same elements into a new shape. The trick is that
**`-1` means "you figure this dimension out"** — NumPy infers it from the
total number of elements:

```python
matrix.reshape(-1, 2)   # "2 columns, as many rows as needed"
```

With 6 elements and 2 columns requested, NumPy computes 3 rows. That saves
doing the arithmetic yourself (and keeps working when the size changes).

To collapse a matrix back down to one dimension:

```python
matrix.flatten()   # 1D copy: array([1, 2, 3, 4, 5, 6])
```

Note `flatten` returns a **copy**, not a view — the distinction from Day 12.
Changing the flattened result leaves the original matrix alone.

### Building matrices

NumPy can construct common matrices directly, no literals needed:

```python
np.zeros((2, 3))   # all zeros
np.ones((2, 3))    # all ones
np.eye(3)          # identity: 1s on the diagonal, 0s elsewhere
```

`np.eye(3)` is the **identity matrix** from Week 1 — the transformation that
changes nothing.

And aggregations work across the whole matrix:

```python
matrix.sum()   # sum of every element
```

## Boolean masks

This is the pattern that clicked hardest. A comparison against an array
doesn't give one `True`/`False` — it applies element-wise and gives back a
**matrix of booleans** the same shape:

```python
matrix < 5
# array([[ True,  True,  True],
#        [ True, False, False]])
```

Two methods then collapse that grid into a single answer:

```python
bool_matrix.all()   # True only if EVERY element is True
bool_matrix.any()   # True if AT LEAST ONE element is True
```

So `(matrix < 5).all()` asks "is everything under 5?" and `(matrix < 5).any()`
asks "is anything under 5?". Comparison to build the mask, `.all()`/`.any()`
to reduce it — that's the whole idiom.

## NaN — the missing value

`NaN` ("not a number") is NumPy's null: the placeholder for a value that's
missing or undefined. It's the thing that quietly poisons results, because
arithmetic involving `NaN` produces `NaN`, so one missing entry can turn a
whole mean into nothing useful.

You can't test for it with `== np.nan` (NaN isn't equal to anything, including
itself). You need `np.isnan`, and combined with `.any()` it becomes a clean
data-quality check:

```python
np.isnan(matrix).any()   # True if the matrix contains ANY missing value
```

That's the boolean-mask idiom doing real work: build a grid of "is this one
NaN?", then ask whether any of them are.

## Takeaway

Day 12 was arrays; Day 13 is arrays with **structure and randomness** on top.

The part that ties back hardest is statistics. Mean, variance, and standard
deviation were definitions on Day 11 — here they're single calls over real
data, and `rng.normal()` generates exactly the distribution the Central Limit
Theorem predicts. Being able to sample a distribution and immediately measure
its spread is what makes the probability week feel usable rather than
abstract.

The idiom I'll reuse most is the **boolean mask**: compare to get a grid of
booleans, then `.all()` or `.any()` to reduce it to a decision. `np.isnan(x).any()`
is the first real instance — a one-line "is my data clean?" check — and that
same shape shows up everywhere in data cleaning, which is exactly what the
Week 3 checkpoint asks for.

## Sources

Udemy — [Scientific Computing with NumPy](https://www.udemy.com/course/scientific-computing-with-numpy/),
plus the official docs for reference:

1. [Random generator (`default_rng`)](https://numpy.org/doc/stable/reference/random/generator.html)
2. [Array manipulation and shapes](https://numpy.org/doc/stable/user/basics.creation.html)
