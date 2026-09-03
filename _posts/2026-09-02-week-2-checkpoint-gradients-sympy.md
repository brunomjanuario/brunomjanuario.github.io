---
title: "Week 2, Checkpoint — Gradients by Hand, Verified with SymPy"
date: 2026-09-02 00:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, calculus, python, sympy]
math: true
---

Week 2 was calculus — derivatives, the chain rule, and on Day 10 partial
derivatives and the gradient. The checkpoint is where that turns into code:
compute the gradient of a simple multivariable function **by hand**, then
verify it against a symbolic library (SymPy). The point isn't to let the
computer do the calculus — it's to do the calculus myself and use the computer
as a grader.

## Picking a function

I wanted something small but not trivial. A pure polynomial would let me
differentiate half-asleep; I wanted a term that actually forces the **chain
rule** from Day 7, since that's the part that's easy to get wrong. So:

$$
f(x, y) = x^2 y + \sin(xy)
$$

The $x^2 y$ term is straightforward. The $\sin(xy)$ term is the interesting
one — differentiating it means applying the chain rule, and the inner
derivative is itself a partial derivative that depends on which variable
I'm holding constant.

## The gradient by hand

The rule from Day 10: to take a partial derivative, **hold the other variable
constant** and differentiate normally.

For $\partial f / \partial x$, treat $y$ as a constant:

- $x^2 y$ differentiates to $2xy$ (the $y$ rides along as a constant factor),
- $\sin(xy)$ differentiates to $\cos(xy) \cdot y$ — the chain rule, where the
  inner function $xy$ has $x$-derivative $y$.

$$
\frac{\partial f}{\partial x} = 2xy + y\cos(xy)
$$

For $\partial f / \partial y$, treat $x$ as a constant:

- $x^2 y$ differentiates to $x^2$,
- $\sin(xy)$ differentiates to $\cos(xy) \cdot x$ — same chain rule, inner
  derivative now $x$.

$$
\frac{\partial f}{\partial y} = x^2 + x\cos(xy)
$$

The gradient is just those two partials stacked into a vector:

$$
\nabla f = \begin{bmatrix} 2xy + y\cos(xy) \\ x^2 + x\cos(xy) \end{bmatrix}
$$

In code, that's a direct transcription — numbers in, numbers out:

```python
def grad_by_hand(x: float, y: float) -> tuple[float, float]:
    """Gradient worked out by hand, evaluated numerically at (x, y)."""
    df_dx = 2 * x * y + y * math.cos(x * y)
    df_dy = x**2 + x * math.cos(x * y)
    return (df_dx, df_dy)
```

## The SymPy version

This is where the checkpoint's "verify" comes from. The trick is that SymPy
doesn't work with numbers — it works with **symbols**. You build the function
out of symbolic placeholders, differentiate the expression itself, and only
plug numbers in at the very end.

```python
def grad_symbolic():
    x, y = sympy.symbols("x y")
    expr = x**2 * y + sympy.sin(x * y)

    df_dx = sympy.diff(expr, x)
    df_dy = sympy.diff(expr, y)
    return (df_dx, df_dy, (x, y))
```

`sympy.symbols("x y")` creates abstract placeholders with no value.
`sympy.diff(expr, x)` differentiates the expression *with respect to the
symbol* $x$ — applying the chain rule symbolically, the same steps I did by
hand, but mechanically. The function returns the symbols alongside the
derivatives on purpose: to plug numbers in later, you need the *same* symbol
objects that built the expression.

Printing what SymPy produces, next to my hand formulas:

```
df/dx = 2*x*y + y*cos(x*y)
df/dy = x**2 + x*cos(x*y)
```

Identical to what I derived — which is the first half of the check. The second
half is confirming they also agree *numerically* at actual points.

## Verifying

Numbers enter the symbolic expressions through `.subs()`, which walks the
expression and swaps each symbol for a value:

```python
def verify(point: tuple[float, float]) -> None:
    px, py = point

    hand = grad_by_hand(px, py)

    df_dx, df_dy, (x, y) = grad_symbolic()
    symbolic = (
        float(df_dx.subs({x: px, y: py})),
        float(df_dy.subs({x: px, y: py})),
    )

    assert math.isclose(hand[0], symbolic[0], rel_tol=1e-9, abs_tol=1e-12)
    assert math.isclose(hand[1], symbolic[1], rel_tol=1e-9, abs_tol=1e-12)
```

The two `assert`s are the actual checkpoint — the same role as the
`np.allclose` check in the Week 1 script. `math.isclose` instead of `==`
because both sides go through floating-point `cos`, so demanding exact
equality would be wrong; I want "equal to within rounding," which is what the
tolerances say. I test a few points on purpose, including edge cases —
the origin, and $x = \pi$ — since a single lucky point can hide a sign error.

Running it:

```
df/dx = 2*x*y + y*cos(x*y)
df/dy = x**2 + x*cos(x*y)

point (x, y) = (1.0, 2.0)
  f(x, y)        = 2.909297
  grad by hand   = (3.167706, 0.583853)
  grad by sympy  = (3.167706, 0.583853)
  match          = OK

point (x, y) = (0.0, 0.0)
  grad by hand   = (0.000000, 0.000000)
  grad by sympy  = (0.000000, 0.000000)
  match          = OK

point (x, y) = (-1.5, 0.5)
  grad by hand   = (-1.134156, 1.152467)
  grad by sympy  = (-1.134156, 1.152467)
  match          = OK

point (x, y) = (3.141592653589793, 1.0)
  grad by hand   = (5.283185, 6.728012)
  grad by sympy  = (5.283185, 6.728012)
  match          = OK
```

Both symbolically and numerically, my hand-derived gradient matches SymPy.

## The two ways x and y "enter" the function

The thing that clicked writing this: the by-hand and symbolic versions differ
entirely in *how the variables get in*.

- `grad_by_hand(x, y)` takes $x$ and $y$ as **function arguments** — real
  numbers plugged straight into Python arithmetic.
- `grad_symbolic()` never takes them as arguments. It **creates** $x$ and $y$
  as symbols, differentiates the expression abstractly, and numbers arrive
  only later through `.subs()`.

That build-symbolically-then-substitute split is exactly what lets SymPy do
the calculus for you: it manipulates the *expression*, not values, so it can
apply the chain rule to the form of the function and only touch numbers at the
very end.

## Takeaway

The gradient stops being a definition and becomes a concrete object here: two
partials, each computed by freezing the other variable, stacked into a vector
I can evaluate at any point. Doing it by hand and having SymPy agree — both in
symbolic form and numerically across several points — is the difference
between "I read about gradients" and "I can produce one and check it."

And that's the whole game for what's coming. Every training loop later in the
roadmap computes a gradient of a loss function; the only differences are that
the function has millions of inputs instead of two, and that a library
(autograd, then PyTorch) plays the role SymPy played here. Deriving one by
hand first is what makes the automatic version legible instead of magic.

## Code

Full script: `week02_calculus.py` — the hand gradient, the SymPy gradient, and
the `verify` harness that checks them against each other symbolically and
numerically before trusting either.
