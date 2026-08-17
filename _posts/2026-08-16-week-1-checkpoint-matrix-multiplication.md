---
title: "Week 1, Checkpoint — Matrix Multiplication From Scratch"
date: 2026-08-16 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, linear-algebra, python, numpy]
math: true
---

Four days of 3Blue1Brown got me the picture of a matrix as a transformation.
The Week 1 checkpoint is where that picture has to turn into code: implement
matrix multiplication in raw Python, no NumPy, then redo it in NumPy and
compare speed.

## Setting up the shapes

Two matrices `a` (shape `rows_a x cols_a`) and `b` (shape `rows_b x cols_b`)
can only be multiplied if `cols_a == rows_b`, and the result is
`rows_a x cols_b`. That constraint comes straight from Day 1: each output
column is `b`'s column pushed through `a`'s columns as a linear combination,
so the number of things being combined (`cols_a`) has to match the number of
rows in `b`.

I got this wrong on the first pass — I allocated the result matrix using
`len(a)` and `len(a[0])` for both dimensions, which only happens to work when
every matrix involved is square. Multiplying a 2×3 by a 3×2 immediately threw
an `IndexError`, which is exactly the kind of bug testing with only square
matrices hides.

## The raw Python version

```python
def matmul_raw(a: list[list[float]], b: list[list[float]]) -> list[list[float]]:
    """Multiply two matrices (lists of lists) without using NumPy.

    a is rows_a x cols_a, b is rows_b x cols_b. cols_a must equal rows_b.
    Returns a rows_a x cols_b matrix.
    """
    result = [[0.0 for j in range(len(b[0]))] for i in range(len(a))]

    for i in range(len(a)):
        for j in range(len(b[0])):
            value = 0
            for z in range(len(b)):
                value += a[i][z] * b[z][j]
            result[i][j] = round(value, 8)

    return result
```

The three loops map directly onto the definition: for every output cell
`(i, j)`, walk along row `i` of `a` and down column `j` of `b` at the same
time, multiplying matching entries and summing them up. `z` is the shared
dimension — it has to range over `cols_a`, which by the multiplication rule
is the same number as `rows_b`, so `range(len(b))` is correct there.

Once the result shape was fixed to `len(a)` rows by `len(b[0])` columns —
matching the docstring instead of assuming everything is square — a
non-square test confirmed it:

```python
a = [[1, 2, 3], [4, 5, 6]]       # 2x3
b = [[1, 0], [0, 1], [1, 1]]     # 3x2
matmul_raw(a, b)                 # [[4, 5], [10, 11]]
```

## The NumPy version

```python
def matmul_numpy(a: list[list[float]], b: list[list[float]]):
    """Multiply two matrices using NumPy."""
    return np.matmul(a, b)
```

One line. The whole point of the exercise is feeling that gap before trusting
it.

## Benchmarking

```python
def benchmark(n: int) -> None:
    a = random_matrix(n, n)
    b = random_matrix(n, n)

    start = time.perf_counter()
    raw_result = matmul_raw(a, b)
    raw_time = time.perf_counter() - start

    start = time.perf_counter()
    np_result = matmul_numpy(a, b)
    np_time = time.perf_counter() - start

    assert np.allclose(raw_result, np_result), "results diverge!"

    print(f"n={n}: raw Python = {raw_time:.4f}s, NumPy = {np_time:.6f}s, "
          f"speedup = {raw_time / np_time:.1f}x")
```

The `assert` matters more than the timing — it's the check that the raw
implementation and NumPy actually agree before comparing how fast they are.
Running it for a few sizes:

```
n=2:   raw Python = 0.0000s, NumPy = 0.000007s, speedup = 0.8x
n=5:   raw Python = 0.0000s, NumPy = 0.000004s, speedup = 3.8x
n=10:  raw Python = 0.0001s, NumPy = 0.000008s, speedup = 10.7x
n=50:  raw Python = 0.0060s, NumPy = 0.000829s, speedup = 7.2x
n=100: raw Python = 0.0445s, NumPy = 0.000331s, speedup = 134.2x
```

At `n=2` the pure-Python version is actually faster — NumPy's overhead
(allocating arrays, dispatching into compiled code) costs more than three
nested loops over a handful of numbers. But the gap flips fast: by `n=100`
NumPy is 134x faster, because it's calling into a compiled, vectorized BLAS
routine instead of doing scalar multiply-and-add in the Python interpreter
one element at a time. The raw version's cost grows with the loops directly —
$O(n^3)$ scalar operations, each paying Python's interpreter overhead. NumPy
pays that overhead once per call, not once per multiplication.

## Takeaway

The three nested loops aren't an implementation detail to memorize — they're
the $O(n^3)$ cost that all the machinery from Day 1 (spans, linear
combinations, basis vectors) was quietly hiding. NumPy doesn't avoid that
cost, it just pays it in compiled code instead of the Python interpreter,
which is the entire 134x.

## Code

Full script: `week01_linear_algebra.py` — raw matmul, NumPy matmul, and the
benchmark harness that checks them against each other before trusting the
timing.
