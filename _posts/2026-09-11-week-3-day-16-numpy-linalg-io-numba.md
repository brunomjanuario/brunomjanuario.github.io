---
title: "Week 3, Day 16 — NumPy: Eigenvectors, QR Decomposition, Saving Data, and Numba"
date: 2026-09-11 00:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, python, numpy, linear-algebra, numba]
math: true
---

Day 16 finishes the NumPy course. More of `numpy.linalg` — **eigenvectors**,
**orthogonal matrices**, and **QR decomposition** — plus handling the errors
linear algebra throws, **saving and loading** arrays to disk, a look at where
NumPy sits in the wider ecosystem, and **Numba** for when NumPy alone isn't fast
enough. It ends with the **Week 3 checkpoint**: load a CSV, clean it, and
plot it. These are my notes, cleaned up.

## Eigenvectors and eigenvalues

Straight from Week 1, Day 4: an **eigenvector** is a vector that a matrix only
**stretches** — it stays on its own line instead of being knocked off it. The
amount it gets stretched by is the **eigenvalue**.

$$
A\vec{v} = \lambda\vec{v}
$$

NumPy computes both at once:

```python
from numpy import linalg

eigenvalues, eigenvectors = linalg.eig(matrix)
```

One layout detail that's easy to get wrong: the eigenvectors come back as the
**columns** of the second array, not its rows. So the eigenvector paired with
`eigenvalues[i]` is `eigenvectors[:, i]`.

Which gives a check you can run instead of trust:

```python
v = eigenvectors[:, 0]
np.allclose(matrix @ v, eigenvalues[0] * v)   # True
```

That's the defining equation, asserted in one line.

## Determinant and invertibility

```python
linalg.det(matrix)
```

The rule from Day 2 and Day 14 again: a **non-zero determinant means the matrix
is invertible**. A zero determinant means the transformation squashed space
flat, and there's no undoing that.

## Orthogonal matrices

A matrix is **orthogonal** when its columns are:

- **orthogonal** to each other — every pair has a dot product of 0, and
- **unit length** — each column has a norm of 1.

The consequence is what makes them valuable. Because the columns are
perpendicular unit vectors, multiplying the matrix by its own transpose gives
the identity:

$$
Q^{T}Q = I \qquad\Longrightarrow\qquad Q^{-1} = Q^{T}
$$

So **the inverse of an orthogonal matrix is just its transpose** — and from
Day 15, transposing is free, since it only swaps the strides. The most
expensive operation in linear algebra becomes the cheapest one. Geometrically
these are the rotations and reflections: transformations that move space around
without stretching or squashing it, which is the "conserves the dot product"
idea from Week 1.

## QR decomposition

When a matrix is awkward to work with, it's often better to **break it into
simpler pieces**. QR decomposition says that any matrix can be written as a
product of two nicer ones:

$$
A = QR
$$

- $Q$ is **orthogonal** — so its inverse is free, as above.
- $R$ is **upper triangular** — every entry below the diagonal is zero.

```python
Q, R = linalg.qr(matrix)   # orthogonal first, upper-triangular second
```

Why that's a better shape: solving $A\vec{x} = \vec{b}$ becomes solving
$QR\vec{x} = \vec{b}$. Multiply both sides by $Q^{T}$ — free — and you're left
with $R\vec{x} = Q^{T}\vec{b}$. A triangular system solves by
**back-substitution**: the bottom row has a single unknown, so solve that, plug
it into the row above, and work upward. No inversion anywhere.

It's the same move as the Fourier transform on Day 15: change the
representation, and a hard problem becomes an easy one.

## When linear algebra fails: `LinAlgError`

Not every system has a solution. Ask `linalg.solve` to solve a system with a
singular matrix and it doesn't return garbage — it raises a `LinAlgError`. So
wrap it:

```python
try:
    x = linalg.solve(A, y)
except linalg.LinAlgError:
    print("No unique solution: the matrix is singular.")
```

Note the exception lives in the `linalg` module, so it's `linalg.LinAlgError`
(or `from numpy.linalg import LinAlgError`) — a bare `LinAlgError` is a
`NameError` unless you've imported it.

This is the graceful counterpart to Day 14's determinant check: rather than
testing the determinant up front, attempt the solve and handle the failure.

## Saving and loading data

### Text files (CSV)

`np.loadtxt` reads delimited text into an array:

```python
rain_data = np.loadtxt("filename.csv", delimiter=",")
```

Real data has holes, and Day 13's `NaN` is how they show up. `np.nan_to_num`
replaces them:

```python
np.nan_to_num(rain_data, nan=1, copy=False)
```

`nan=1` sets the replacement value, and `copy=False` edits the array **in
place** rather than returning a new one — the same in-place-versus-return
distinction as `arr.sort()` versus `np.sort()` from Day 12.

Writing back out is the mirror image:

```python
np.savetxt("filename.csv", matrix, delimiter=",", fmt="%.0f")
```

`fmt` controls how each number is written — `"%.0f"` means no decimal places.

One spelling trap: it's **`delimiter`**. Spelled `delimeter`, `loadtxt` raises
`TypeError: unexpected keyword argument` — at least it fails loudly.

### NumPy's own format (`.npy`)

For saving arrays to reload later in NumPy, `.npy` is the right format:

```python
np.save("filename", matrix)   # writes filename.npy
np.load("filename.npy")       # reads it back
```

`np.save` adds the `.npy` extension for you, but `np.load` needs the full
filename.

Why prefer it over CSV: it's **binary and exact**. It stores the dtype and
shape alongside the values, so an array comes back precisely as it went in — no
float precision lost to text formatting, no guessing the shape, no delimiter to
get wrong. CSV is for exchanging data with other tools; `.npy` is for NumPy
talking to itself.

## The ecosystem

NumPy is the foundation, not the whole building. The libraries that sit on top
of it:

- **Matplotlib** — visualisation (Day 12).
- **Pandas** — data structures for labelled, tabular data: the DataFrame. It's
  what the Week 3 checkpoint's "load a CSV, clean it" is really about.
- **PyTorch** — machine learning. Its tensors are close cousins of NumPy arrays,
  with GPU support and automatic differentiation added — the Week 2 gradients,
  computed for you.

## Numba

NumPy is fast when you can express a problem as whole-array operations. When
you can't — when you genuinely need a Python loop — you're back to paying the
interpreter's cost per step, exactly the Week 1 checkpoint problem.

**Numba** is an open-source **JIT (just-in-time) compiler** that translates
Python and NumPy code into fast machine code. It's built specifically for
scientific computing. You opt a function in with a decorator:

```python
from numba import jit, njit

@jit(nopython=True)
def my_function(x):
    ...

@njit              # shorthand for the same thing
def my_function(x):
    ...
```

`nopython=True` (capital `T` — it's a Python boolean) tells Numba to compile
the function **entirely**, with no fallback to the slow interpreter; if it
can't, it errors instead of quietly running slow. `@njit` is just the shorter
spelling.

"Just-in-time" means compilation happens the first time the function is
called, so that first call is slow and every call after it runs at compiled
speed.

## Week 3 checkpoint: load, clean, plot

The roadmap's checkpoint for this week is to _load a CSV dataset, clean it, and
plot two variables against each other — no tutorial open, from memory._ It uses
the practical half of today almost line for line.

The data is two small hand-made CSV files. The first is a 3×3 grid with one
value missing:

```
nan, 2, 3
4, 5, 6
7, 8, 9
```

The second is a single row of nine y-values:

```
10, 11, 12, 13, 14, 15, 16, 17, 18
```

The whole script:

```python
import numpy as np
import matplotlib.pyplot as plt

if __name__ == "__main__":
    matrix = np.loadtxt("week03_numpy_matrix.csv", delimiter=",")
    y_values = np.loadtxt("week03_numpy_matrix_y_values.csv", delimiter=",")
    np.nan_to_num(matrix, nan=1, copy=False)
    x_values = matrix.flatten()
    plt.plot(x_values, y_values)
    plt.show()
```

Four steps, and each one leans on something from earlier in the week.

**Load.** `np.loadtxt` reads both files, and the literal `nan` in the first
cell comes through as a real `NaN` rather than an error. The keyword is spelled
`delimiter` — the typo from my notes would have failed on the very first line.

**Clean.** `np.nan_to_num(..., nan=1, copy=False)` replaces the missing value
with 1, in place. Because `copy=False` edits `matrix` directly, the call isn't
assigned to anything — the same pattern as `arr.sort()` on Day 12.

**Reshape.** `plt.plot` needs x and y to be the same length, and they aren't:
the matrix is `(3, 3)` and the y-values are `(9,)`. `flatten()` from Day 13
turns the grid into `(9,)` — `[1, 2, …, 9]` — so the shapes line up. Skip it
and Matplotlib refuses: `x and y must have same first dimension`.

**Plot.** The result is a straight line from $(1, 10)$ to $(9, 18)$ — every
point sits on $y = x + 9$.

The line is perfectly straight because the fill value happens to be exactly
right: the grid counts 1 to 9, so the missing first entry "should" be 1. Real
data doesn't tell you that. A fill value is a guess, and the guess ends up in
the plot — which is why the usual choices are the column's mean or median, or
dropping the row entirely. A constant is the simplest option, not the safest.

Skipping the clean step wouldn't crash anything, either. Matplotlib just leaves
a gap where the `NaN` is, so the line would quietly start at $x = 2$ — a
missing point that's easy to miss. That's the argument for checking with
`np.isnan(matrix).any()` before plotting, not after.

## Takeaway

Week 3 ends back where Week 1 began — and the gap between them is the point.
Week 1 was eigenvectors and orthogonality as pictures. Day 16 is being able to
compute them, and more usefully, to **check** them: `np.allclose(A @ v, λ * v)`
is the eigenvector definition as an assertion, and `np.allclose(Q.T @ Q,
np.eye(n))` is orthogonality as an assertion.

QR decomposition is the idea I want to hold onto. It's the third instance this
week of the same move — strides make transposing free, Fourier makes smoothing
easy, QR makes solving a triangle — and the pattern is clearly the lesson:
**don't solve the hard problem; transform it into an easy one**.

The practical half is what the checkpoint actually used. Loading a CSV, filling
a `NaN`, and flattening a matrix so its shape matches the y-values is only five
lines of code, but each line leans on a different part of the week. The part
I'll take forward is the fill value: on toy data it's obviously right, and on
real data it's a decision that quietly shapes the result.

## Code

Full script: `week03_numpy.py`, with its two input files
`week03_numpy_matrix.csv` and `week03_numpy_matrix_y_values.csv`.

## Sources

Udemy — [Scientific Computing with NumPy](https://www.udemy.com/course/scientific-computing-with-numpy/),
plus the official docs for reference:

1. [Linear algebra (`numpy.linalg`)](https://numpy.org/doc/stable/reference/routines.linalg.html)
2. [Input and output (`loadtxt`, `savetxt`, `save`, `load`)](https://numpy.org/doc/stable/reference/routines.io.html)
3. [Numba — a 5 minute guide](https://numba.readthedocs.io/en/stable/user/5minguide.html)
