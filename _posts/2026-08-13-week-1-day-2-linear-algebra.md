---
title: "Week 1, Day 2 — Linear Algebra: Determinants, Inverses, and Duality"
date: 2026-08-13 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, linear-algebra, 3blue1brown]
math: true
---

Day 2 of the roadmap: chapters 5 through 9 of 3Blue1Brown's _Essence of Linear
Algebra_. Day 1 set up the core idea — a matrix is a transformation of space,
written down as where the basis vectors land. Today every topic is a
consequence of that.

## Three-dimensional linear transformations

The logic from 2D carries over unchanged. The only difference is that there's
now a third basis vector, **k̂** ("k hat"), pointing along the z axis.

So a 3D transformation is described by where three basis vectors land, at
three coordinates each — **nine numbers**, arranged as the three columns of a
3×3 matrix. The rules for "linear" are the same: grid lines stay parallel and
evenly spaced, and the origin stays put.

Matrix-vector multiplication is still the same question — "if the basis
vectors went _here_, where does this vector go?" — which means it's still just
a scaled sum of the columns:

$$
\begin{bmatrix} 0 & 1 & 2 \\ 3 & 4 & 5 \\ 6 & 7 & 8 \end{bmatrix}
\begin{bmatrix} x \\ y \\ z \end{bmatrix}
= x \begin{bmatrix} 0 \\ 3 \\ 6 \end{bmatrix}
+ y \begin{bmatrix} 1 \\ 4 \\ 7 \end{bmatrix}
+ z \begin{bmatrix} 2 \\ 5 \\ 8 \end{bmatrix}
$$

Column one is where î lands, column two is where ĵ lands, column three is
where k̂ lands. Nothing new to memorize — the pattern just got one term longer.

## The determinant

A transformation stretches and squashes space. The factor by which it scales
**any** area is called the **determinant**.

The word "any" is doing real work there. You'd think different regions would
be affected differently, but because grid lines stay parallel and evenly
spaced, a single number describes what happens to every area at once. Measure
what happens to the unit square and you know what happens to everything.

Three cases worth separating:

**det = 0.** The transformation squishes space into something
lower-dimensional. In 2D that means everything collapses onto a line or a
single point; in 3D it collapses onto a plane, a line, or a point. Either way,
the output has fewer dimensions than the input — and that turns out to be the
whole story behind when a system of equations has a unique solution.

**det < 0.** The orientation of space has been flipped — in 2D, like turning
the plane over; in 3D, like a right hand becoming a left hand. The sign tells
you about the flip; the absolute value still tells you the scaling factor.

**In 3D the determinant scales volume.** The unit cube spanned by î, ĵ, and k̂
starts with a volume of 1. After the transformation it becomes a slanted box —
a **parallelepiped** — and the volume of that box _is_ the determinant.

### The formulas

For 2×2:

$$
\det\left(\begin{bmatrix} a & b \\ c & d \end{bmatrix}\right) = ad - bc
$$

The $ad$ term is the pure stretching along each axis; the $bc$ term corrects
for how much the parallelogram has been slanted. If $b$ and $c$ are both zero,
the unit square just becomes an $a \times d$ rectangle.

For 3×3, it breaks into three 2×2 determinants:

$$
\det\left(\begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}\right)
= a \det\left(\begin{bmatrix} e & f \\ h & i \end{bmatrix}\right)
- b \det\left(\begin{bmatrix} d & f \\ g & i \end{bmatrix}\right)
+ c \det\left(\begin{bmatrix} d & e \\ g & h \end{bmatrix}\right)
$$

Each term drops the row and column of its leading entry, and the signs
alternate.

One consequence that falls straight out of the geometric definition:

$$
\det(M_1 M_2) = \det(M_1)\det(M_2)
$$

If one transformation scales areas by 3 and the next scales them by 2, doing
both scales them by 6. No algebra required to believe it.

## Inverse matrices, column space, and null space

A linear system of equations is really just a matrix equation. Every equation
in the system can be rearranged so all the variables sit on the left with
their coefficients and a constant sits on the right, and stacking those gives:

$$
A\vec{x} = \vec{v}
$$

Which reads geometrically as: **find the vector $\vec{x}$ that lands on
$\vec{v}$ after the transformation $A$.**

**When $\det(A) \neq 0$**, you can answer that by playing the transformation
in reverse. That reverse transformation is $A^{-1}$, and running one after the
other leaves everything exactly where it started — the **identity
transformation**:

$$
A^{-1}A = I
$$

So $\vec{x} = A^{-1}\vec{v}$, and it's the only solution.

**When $\det(A) = 0$**, no inverse exists. The reason is nicer than the
algebra: if the transformation squashed a whole plane down onto a line, the
inverse would have to take each point on that line back out to a whole line of
points. That's one input producing many outputs, which isn't a function. A
solution might still exist — if $\vec{v}$ happens to sit on that line — it
just can't be found by inverting.

### Rank

**Rank** is the number of dimensions in the output of a transformation.

- If everything lands on a line, the transformation has **rank 1**.
- If everything lands on a 2D plane, it has **rank 2**.

More precisely, the set of all possible outputs is the **column space** — the
span of the matrix's columns, which makes sense given the columns are just
where the basis vectors land. Rank is the number of dimensions of that column
space.

When rank is as high as it can be — equal to the number of columns — the
matrix is **full rank**. For a square matrix, full rank is exactly the same
statement as $\det \neq 0$.

### Null space

The **null space** (or kernel) is the set of vectors that land on the origin
after the transformation.

For a full-rank matrix, the only thing that lands on the origin is the origin
itself, so the null space is just the zero vector. But when the transformation
squashes dimensions, an entire line — or plane — of vectors gets crushed to
zero, and that's the null space.

This is also the answer to $A\vec{x} = \vec{0}$: the null space is precisely
the set of solutions to that system.

## Nonsquare matrices as transformations between dimensions

Nothing says input and output need the same number of dimensions.

A **3×2 matrix** has two columns (so it takes a 2D input — one column per
input basis vector) and three rows (so each output is a 3D vector). It maps 2D
into 3D. If it's full rank, the column space is a 2D plane slicing through the
origin of 3D space — the transformation embeds the plane in a bigger space
without filling it.

Going the other way, a **2×3 matrix** takes 3D vectors and outputs 2D ones.

The shape reads as: **columns = input dimensions, rows = output dimensions.**

## Dot products and duality

Given two vectors of the same dimension — two lists of numbers of the same
length — the **dot product** pairs up the coordinates, multiplies each pair,
and adds the results.

Geometrically:

$$
\vec{v} \cdot \vec{w} = \|\vec{v}\|\,\|\vec{w}\|\cos\theta
$$

Which gives the sign rule:

- Pointing in generally the **same direction** → positive.
- **Perpendicular** → zero. (For nonzero vectors, zero dot product means
  perpendicular; the zero vector dots to zero with everything trivially.)
- Pointing in generally **opposite directions** → negative.

Order doesn't matter: $\vec{v} \cdot \vec{w} = \vec{w} \cdot \vec{v}$. That
looks arbitrary from the definition but it's obvious from symmetry — if the
two vectors were the same length, the picture is mirror-symmetric, so
projecting either one onto the other gives the same answer, and scaling one of
them scales the result the same way regardless of which one you scaled.

### The part that's actually interesting

Projecting a vector onto a line is a transformation from 2D to **1D** — it
takes a vector and gives back a number. So it can be written as a $1 \times 2$
matrix.

Take a unit vector $\hat{u}$ sitting on that line, with coordinates
$(u_x, u_y)$. By symmetry, projecting î onto that line gives $u_x$, and
projecting ĵ gives $u_y$. So the projection transformation is:

$$
\begin{bmatrix} u_x & u_y \end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
= u_x x + u_y y
$$

That's numerically identical to $\hat{u} \cdot (x, y)$. **The matrix of the
projection is the vector, tipped on its side.**

That's **duality**: every linear transformation from a space down to one
dimension corresponds to exactly one vector in that space, and applying the
transformation is the same as taking a dot product with that vector. The
"dual" of a vector is the transformation it encodes, and the dual of such a
transformation is a vector.

So the dot product isn't really a strange multiplication rule. It's what a
projection looks like when you write it down.

## Takeaway

Day 1's idea was that a matrix _is_ a transformation. Today every concept
turned out to be a property of that transformation rather than a property of
the grid of numbers:

- **determinant** — how much it scales area or volume, and whether it flips
  orientation
- **rank / column space** — how many dimensions survive it
- **null space** — what it crushes to zero
- **inverse** — whether it can be undone (only if it crushes nothing)
- **dot product** — a projection in disguise

The dot product one is the one I'd have never guessed on my own.

## Sources

Chapters 5-9 of 3Blue1Brown's
[Essence of Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab):

5. [Three-dimensional linear transformations](https://www.youtube.com/watch?v=rHLEWRxRGiM)
6. [The determinant](https://www.youtube.com/watch?v=Ip3X9LOh2dk)
7. [Inverse matrices, column space and null space](https://www.youtube.com/watch?v=uQhTuRlWMxw)
8. [Nonsquare matrices as transformations between dimensions](https://www.youtube.com/watch?v=v8VSDg_WQlA)
9. [Dot products and duality](https://www.youtube.com/watch?v=LyGKycYT2v0)
