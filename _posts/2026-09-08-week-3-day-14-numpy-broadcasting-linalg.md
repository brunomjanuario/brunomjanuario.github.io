---
title: "Week 3, Day 14 — NumPy: Broadcasting, Advanced Indexing, and Linear Algebra"
date: 2026-09-08 00:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, python, numpy, linear-algebra]
math: true
---

Day 14 is the day Week 1 comes back. **Broadcasting** explains how NumPy
combines arrays of different shapes, **advanced indexing** is how you pull out
exactly the elements you want, and `numpy.linalg` turns the whole of Week 1's
linear algebra — dot products, determinants, inverses, solving systems — into
function calls. These are my notes, cleaned up.

## Broadcasting

Broadcasting is the rule that lets NumPy operate on arrays whose shapes don't
match. You've already used it without noticing: multiplying a matrix by a
single constant is broadcasting, because that one number gets stretched across
every element.

```python
matrix * 3        # the scalar is broadcast over the whole matrix
```

Matrix + vector is where it gets interesting — sometimes it works, sometimes
it doesn't. The rules are precise:

1. **Padding rule** — line the shapes up from the **right**, and pad the
   shorter shape on the **left** with 1s.
2. **Compatibility rule** — for each pair of dimensions, they must either be
   **equal**, or **one of them must be 1**. A dimension of 1 gets stretched to
   match the other.

If every pair passes, the operation works. If any pair fails, NumPy raises an
error.

A case that works — a `(3, 4)` matrix plus a `(4,)` vector:

```
matrix   (3, 4)
vector      (4,)  ->  padded to (1, 4)
                      row 1 is stretched to 3  ->  result (3, 4)
```

The vector is added to every row.

A case that fails — the same matrix plus a `(3,)` vector:

```
matrix   (3, 4)
vector      (3,)  ->  padded to (1, 3)
                      4 vs 3: not equal, neither is 1  ->  error
```

### Forcing it with `np.newaxis`

Failing doesn't always mean the operation is wrong — often you meant "add this
to every **column**" and NumPy just can't guess that from a flat `(3,)` shape.
You fix it by reshaping the vector into an explicit column:

```python
vector[:, np.newaxis]   # shape (3,) -> (3, 1)
vector.shape            # check what you actually have
```

Now the shapes are `(3, 4)` and `(3, 1)` — the 1 stretches to 4, and it works.

The habit worth keeping: when broadcasting fails, **print `.shape` first**.
The error is almost always a shape you assumed rather than checked.

## Advanced indexing

Three ways to select elements, beyond the basic slicing from Day 12.

### Slicing a matrix

Slices work per-axis, separated by a comma — rows first, then columns:

```python
matrix[3:, 3:]    # from row 3 onward, and from column 3 onward
```

### Indexing with lists

You can pass a **list of indices** to grab specific, non-contiguous rows or
columns:

```python
matrix[[0, 1, 4], :]   # rows 0, 1 and 4 — all columns
```

That's something plain slicing can't do, since those rows aren't evenly
spaced.

### Boolean indexing

The boolean masks from Day 13 can do more than feed `.all()` and `.any()` —
you can index **with** them, and get back only the elements where the mask is
`True`:

```python
matrix[(matrix > 15) & (matrix <= 20)]
```

Two things to note. The operators are `&` and `|` (bitwise), **not** `and` and
`or` — Python's keywords don't work element-wise on arrays. And each condition
needs its own **parentheses**, because `&` binds tighter than the comparison
operators and you'd otherwise get a confusing error.

This is the filtering idiom: build a condition, use it as an index, get the
matching values back.

## Dot product

`np.dot` is Week 1's dot product — pair up the entries, multiply each pair,
sum the results:

```python
np.dot(a, b)   # == a[0]*b[0] + a[1]*b[1] + a[2]*b[2]
```

$$
\vec{a} \cdot \vec{b} = \sum_i a_i b_i
$$

## Cross product

`np.cross` is the other Week 1 product, and unlike the dot product it's
**only defined for 2- and 3-dimensional vectors**:

```python
np.cross(a, b)
```

The property that makes it useful is that the result is **orthogonal** —
perpendicular — to both inputs. Which gives a satisfying way to verify it:
since perpendicular vectors have a dot product of zero (Day 2), taking the dot
product of either original vector with the cross product must give 0.

```python
c = np.cross(a, b)
np.dot(a, c)   # 0
np.dot(b, c)   # 0
```

That's a self-check you can actually run, not just a fact to memorise.

## `numpy.linalg`

The heavier linear algebra lives in its own module:

```python
from numpy import linalg
```

### Vector length

```python
linalg.norm(vector)   # the length (magnitude) of the vector
```

### Matrix multiplication

Two equivalent ways, `@` being the readable one:

```python
first_matrix @ second_matrix
np.matmul(first_matrix, second_matrix)
```

The mental model straight from Week 1: take the first **column** of the second
matrix, and use its entries as scalars on the first matrix's columns — $x$
times the first column, $y$ times the second, $z$ times the third, summed.
That gives the first column of the result. Repeat for each remaining column.
It's a linear combination of columns, which is exactly what the Week 1
checkpoint implemented by hand with three nested loops.

Raising a matrix to a power — applying the same transformation repeatedly:

```python
linalg.matrix_power(first_matrix, 4)
```

### Transpose

```python
first_matrix.T
first_matrix.transpose()   # same thing, spelled out
```

### Determinant

```python
linalg.det(matrix)
```

From Day 2, the determinant is the factor by which a transformation scales
area (or volume). And the key consequence: **a determinant of 0 means the
matrix is not invertible** — the transformation squashed space flat, and you
can't undo that.

In practice floating-point arithmetic rarely gives you a clean zero — you get
something like `1.1e-17` instead. So round before comparing:

```python
np.around(det, 10)   # turns floating-point noise into an actual 0
```

"Determinant is essentially 0" is the real test, not "determinant equals 0".

### Inverse

```python
linalg.inv(matrix)   # only valid when det != 0
```

### Solving a linear system

The payoff. A system of equations written as $A\vec{x} = \vec{b}$ is solved in
one call:

```python
linalg.solve(matrix, vector)
```

You could do it as `linalg.inv(matrix) @ vector`, but `solve` is both faster
and more numerically stable — it doesn't compute the full inverse just to
throw most of it away.

## Takeaway

Week 1 was pictures: a matrix as a transformation, the determinant as an area
scaling factor, the dot product as a projection. Day 14 is the same list of
ideas with a function name attached to each one — and being able to *compute*
them changes them from things I can describe into things I can check.

The verification angle is what I want to keep. `np.dot(a, np.cross(a, b)) == 0`
proves the cross product is orthogonal. `np.around(linalg.det(m), 10) == 0`
proves a matrix is singular before `linalg.inv` blows up. The theory tells you
what should be true; NumPy lets you assert it — the same by-hand-then-verify
pattern as the Week 2 checkpoint.

Broadcasting is the genuinely new idea, and it's the one that will bite. It
fails loudly when shapes are incompatible, which is fine — but it *succeeds*
whenever a dimension happens to be 1, which means a wrongly-shaped array can
silently produce a plausible-looking result instead of an error. Checking
`.shape` is cheap; debugging a silent broadcast is not.

## Sources

Udemy — [Scientific Computing with NumPy](https://www.udemy.com/course/scientific-computing-with-numpy/),
plus the official docs for reference:

1. [Broadcasting](https://numpy.org/doc/stable/user/basics.broadcasting.html)
2. [Indexing on ndarrays](https://numpy.org/doc/stable/user/basics.indexing.html)
3. [Linear algebra (`numpy.linalg`)](https://numpy.org/doc/stable/reference/routines.linalg.html)
