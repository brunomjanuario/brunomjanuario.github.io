---
title: "Week 1, Day 3 — Linear Algebra: Cross Products and Cramer's Rule"
date: 2026-08-14 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, linear-algebra, 3blue1brown]
math: true
---

Day 3 of the roadmap: chapters 10 and 11 of 3Blue1Brown's _Essence of Linear
Algebra_. Two topics today, and both turn out to be the same trick — reading a
geometric question as a linear transformation to the number line, then finding
the vector that encodes it.

## Cross products

The **2D cross product** of $\vec{v}$ and $\vec{w}$ is the signed area of the
parallelogram they span:

$$
\vec{v} \times \vec{w} = \det\left(\begin{bmatrix} v_1 & w_1 \\ v_2 & w_2 \end{bmatrix}\right)
$$

It's just the determinant. The sign tells you orientation: positive if
$\vec{w}$ is a counterclockwise turn from $\vec{v}$, negative if $\vec{v}$ is
to the left of $\vec{w}$.

The **3D cross product** is a different animal — it takes two 3D vectors and
returns a new **3D vector**, not a number:

$$
\vec{v} \times \vec{w} = \vec{p}
$$

- The **length** of $\vec{p}$ is the area of the parallelogram spanned by
  $\vec{v}$ and $\vec{w}$ — same idea as the 2D determinant, just repackaged
  as a magnitude instead of a signed number.
- The **direction** of $\vec{p}$ is perpendicular to both $\vec{v}$ and
  $\vec{w}$.

There are always two directions perpendicular to a plane, so the **right-hand
rule** picks the one that's the actual convention: point the fingers of your
right hand along $\vec{v}$, curl them toward $\vec{w}$, and the thumb points
along $\vec{p}$.

## Cross products via linear transformations

The formula for the 3D cross product looks like it came out of nowhere:

$$
\vec{v} \times \vec{w} =
\begin{bmatrix} v_2 w_3 - v_3 w_2 \\ v_3 w_1 - v_1 w_3 \\ v_1 w_2 - v_2 w_1 \end{bmatrix}
$$

But it falls out of duality, the idea from [Day 2](/posts/week-1-day-2-linear-algebra/)
that every linear transformation from a space down to one dimension
corresponds to some vector, via a dot product.

Define a function of $(x, y, z)$ that computes the volume of the parallelepiped
spanned by $(x, y, z)$, $\vec{v}$, and $\vec{w}$:

$$
f(x, y, z) = \det\left(\begin{bmatrix} x & v_1 & w_1 \\ y & v_2 & w_2 \\ z & v_3 & w_3 \end{bmatrix}\right)
$$

This is linear in $(x, y, z)$, so it's a transformation from 3D to 1D — which
means, by duality, it corresponds to some vector $\vec{p}$ such that
$f(x, y, z) = \vec{p} \cdot (x, y, z)$ for all inputs. Expanding the
determinant and matching coefficients against a dot product gives exactly the
formula above.

The geometric meaning: $\vec{p}$ is the vector such that dotting it with any
$(x, y, z)$ gives the volume spanned by that point, $\vec{v}$, and $\vec{w}$.
Since volume is maximized when $(x, y, z)$ is perpendicular to the base
parallelogram, $\vec{p}$ must point perpendicular to $\vec{v}$ and $\vec{w}$,
and its length has to be the base area for the dot product to equal volume
correctly — which is exactly the definition from the section above. Duality
isn't just a curiosity from Day 2; it's the reason the cross-product formula
looks the way it does.

## Cramer's rule, geometrically

Cramer's rule is a method for solving $A\vec{x} = \vec{v}$ that only works
when $\det(A) \neq 0$ — Gaussian elimination is faster in practice, but the
geometric reasoning behind Cramer's rule is worth seeing once.

It relies on **orthonormal transformations** — transformations that preserve
the dot product, i.e. they keep basis vectors perpendicular and unit length.
For those, computing a coordinate is a projection, and geometrically an area
scaled by such a transformation becomes:

$$
\text{Area} = \det(A) \cdot x
$$

so:

$$
x = \frac{\text{Area}}{\det(A)}
$$

Generalizing beyond orthonormal transformations: to find a coordinate $x_i$ of
the solution vector, take the determinant of $A$ with its $i$-th column
replaced by $\vec{v}$, and divide by $\det(A)$:

$$
x_i = \frac{\det(A_i)}{\det(A)}
$$

where $A_i$ is $A$ with column $i$ swapped out for $\vec{v}$. Geometrically,
swapping in $\vec{v}$ measures how much the parallelepiped's volume changes
along that one axis when you require the solution to land exactly on
$\vec{v}$ — and dividing by $\det(A)$ converts that change back into an actual
coordinate.

## Takeaway

Both of today's topics were "reverse-engineer a vector from a function that
maps to the number line" — duality doing more work than I expected on Day 2.
Cramer's rule is elegant to look at but not something I'd reach for over
Gaussian elimination in practice; it's here for the geometric intuition, not
as a computational tool.

## Sources

Chapters 10-11 of 3Blue1Brown's
[Essence of Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab):

10. [Cross products](https://www.youtube.com/watch?v=eu6i7WJeinw)
11. [Cross products in the light of linear transformations](https://www.youtube.com/watch?v=BaM7OCEm3G0)
12. [Cramer's rule, explained geometrically](https://www.youtube.com/watch?v=jBsC34PxzoM)
