---
title: "Week 1, Day 4 — Linear Algebra: Change of Basis and Eigenvectors"
date: 2026-08-15 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, linear-algebra, 3blue1brown]
math: true
---

Day 4, and the last one for Week 1: chapters 13 through 16 of 3Blue1Brown's
_Essence of Linear Algebra_. Change of basis, eigenvectors, and a first step
outside of arrows-and-grids into abstract vector spaces.

## Change of basis

Coordinates are just scalars on a set of basis vectors — normally î and ĵ. But
nothing forces that choice. Someone else's coordinate system, with a different
pair of basis vectors, describes the same points using different numbers.

A matrix whose columns are someone else's basis vectors, written in your
coordinates, **translates their coordinates into yours**. To go the other
way — translate your coordinates into theirs — you apply the **inverse** of
that matrix.

The more interesting question is applying a transformation described in your
own coordinates to a vector that's expressed in someone else's basis. That
takes three steps:

1. **Translate** their vector into your coordinate system (multiply by the
   change-of-basis matrix).
2. **Apply** the transformation, as normal, in your coordinates.
3. **Translate back** into their coordinate system (multiply by the inverse
   change-of-basis matrix).

Written as one expression, with $A$ the change-of-basis matrix and $M$ the
transformation:

$$
A^{-1} M A
$$

This sandwich — inverse, transformation, forward — shows up constantly:
whenever you see $A^{-1} M A$, read it as "$M$, but performed from the
perspective of a different basis."

## Eigenvectors and eigenvalues

An **eigenvector** of a transformation is a vector that stays on its own
span — the transformation only stretches or squashes it, never rotates it off
its line. The factor it gets scaled by is the **eigenvalue**:

$$
A\vec{v} = \lambda\vec{v}
$$

$\lambda$ is the eigenvalue, $\vec{v}$ the eigenvector.

Not every transformation has eigenvectors. A pure rotation, for instance, has
none — every vector gets pushed off its own span, so solving the equation
turns up no solution.

When both basis vectors happen to be eigenvectors, that's an **eigenbasis**.
In that basis, the transformation's matrix is **diagonal** — every entry off
the diagonal is zero, and the diagonal entries are the eigenvalues themselves.
That's the payoff for having an eigenbasis: instead of doing full matrix
multiplication, applying the transformation $n$ times is just raising each
diagonal entry to the $n$-th power.

## A quick trick for computing eigenvalues

For a 2×2 matrix, there's a shortcut that skips the full characteristic
polynomial:

$$
m = \frac{a + d}{2} \quad \text{(mean of the diagonal)}
$$

$$
p = ad - bc = \det(A) \quad \text{(product} = \det(A))
$$

$$
\lambda_1, \lambda_2 = m \pm \sqrt{m^2 - p}
$$

The mean of the two eigenvalues is the mean of the diagonal entries, and their
product is the determinant — so this is really just solving for two numbers
given their sum and product, dressed up as a linear algebra formula. If
$m^2 < p$, the eigenvalues are complex, which corresponds to the
transformation being a rotation (possibly combined with scaling) — no real
eigenvectors, consistent with the rotation example above.

## Abstract vector spaces

The last chapter pulls the lens back. A **derivative** is a function that
transforms one function into another — and it turns out to be **linear**,
in exactly the same sense as a matrix:

$$
\text{Additivity:} \quad L(\vec{v} + \vec{w}) = L(\vec{v}) + L(\vec{w})
$$

$$
\text{Scaling:} \quad L(c\vec{v}) = cL(\vec{v})
$$

- **Additivity**: transforming the sum of two vectors gives the same result as
  transforming each one separately and adding.
- **Scaling**: transforming a scaled vector gives the same result as scaling
  the transformed vector.

The derivative of a sum of functions is the sum of the derivatives, and
scaling a function before differentiating gives the same result as scaling
after — so the derivative satisfies both, which makes it a linear
transformation, even though there's no grid or arrow in sight. Functions can
be added and scaled just like vectors can, which is really all "vector" ever
required — polynomials, functions, arrows, and lists of numbers are all
vectors as long as they obey the same handful of axioms (rules for how
addition and scaling are allowed to behave).

That's the real takeaway of the chapter: **"vector" isn't about arrows.**
It's about anything that plays by the same two rules, and once something
qualifies, everything built on those two rules — span, linear
transformations, eigenvectors, all of it — applies to it too.

## Week 1 takeaway

Four days in, the throughline has held: a matrix is a transformation, not a
grid of numbers, and every other idea — determinant, rank, null space, dot
product, cross product, eigenvectors — is a property of that transformation.
Today's biggest shift was realizing the transformation idea doesn't even need
vectors-as-arrows to hold; it needs additivity and scaling, full stop.

## Sources

Chapters 13-16 of 3Blue1Brown's
[Essence of Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab):

13. [Change of basis](https://www.youtube.com/watch?v=P2LTAUO1TdA)
14. [Eigenvectors and eigenvalues](https://www.youtube.com/watch?v=PFDu9oVAE-g)
15. [A quick trick for computing eigenvalues](https://www.youtube.com/watch?v=e50Bj7jn9IQ)
16. [Abstract vector spaces](https://www.youtube.com/watch?v=TgKwz5Ikpc8)
