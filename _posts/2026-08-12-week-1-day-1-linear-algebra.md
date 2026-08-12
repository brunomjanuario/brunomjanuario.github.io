---
title: "Week 1, Day 1 — Linear Algebra: Vectors, Span, and Transformations"
date: 2026-08-12 09:00:00 +0000
categories: [AI Fundamentals]
tags: [linear-algebra, 3blue1brown]
math: true
---

First day of the roadmap. I worked through the first four chapters of
3Blue1Brown's _Essence of Linear Algebra_. These are my notes, cleaned up.

## What a vector actually is

There are three ways to look at a vector, and which one you pick depends on
who you are:

- **Physics**: an arrow with a direction and a magnitude.
- **Computer science**: an ordered list of numbers.
- **Math**: anything you can add and scale sensibly.

For ML purposes, the CS view is the working definition — a vector is a list of
numbers. But the geometric picture is what makes the operations intuitive, so
it's worth holding both at once.

Every vector starts at the **origin** `(0, 0)`. The first number is the
x coordinate, the second is the y coordinate, and a third number adds a
z axis for 3D.

## The two fundamental operations

Everything in linear algebra is built out of exactly two operations:

**Addition.** The key insight is that you don't measure the second vector from
the origin. You move along the first vector, and _then_ apply the second one
from where you landed — tip to tail. The result is where you end up.

**Scaling.** Multiplying a vector stretches it, squashes it, or (with a
negative number) flips its direction. The number doing the multiplying is
called a **scalar** — that's literally where the name comes from, it's the
thing that scales.

## Linear combinations, span, and basis vectors

The basis vectors of the standard xy coordinate system are **î** ("i hat") and
**ĵ** ("j hat"). When you write a vector as `(3, 2)`, what you're really saying
is: scale î by 3, scale ĵ by 2, and add them.

That's a **linear combination** — scale some vectors, add the results.

The **span** of two vectors is the set of _all_ their linear combinations —
everything you can possibly reach with them.

A trick that helped: when you're thinking about a single vector, picture an
arrow. When you're thinking about a whole _collection_ of vectors, picture
points instead. Otherwise the diagram becomes unreadable.

Some cases worth knowing:

- Two vectors in 2D generally span the whole plane.
- Two vectors in 3D span a **flat sheet** cutting through the origin — not the
  full space.
- Three vectors in 3D will _almost always_ span all of 3D space.

When a vector doesn't add anything new to the span — it already sits inside
what the others could reach — it's **linearly dependent**. When every vector
genuinely adds a new dimension, they're **linearly independent**.

So the definition of a **basis** falls out of this: a set of linearly
independent vectors that spans the space.

## Linear transformations and matrices

A **transformation** is just a function, but the word suggests movement, which
is the right way to picture it: it takes a vector in and gives a vector out.

What makes it **linear** is two visual conditions:

1. Grid lines stay **parallel** and **evenly spaced**.
2. The origin stays fixed.

Here's the part that clicked for me. A 2D linear transformation is completely
described by just **four numbers**: where î lands, and where ĵ lands. That's
it. Two coordinates each, four total.

And those four numbers, arranged in a box, _are_ the matrix. The columns of a
matrix are literally the landing spots of the basis vectors. Matrix-vector
multiplication isn't an arbitrary rule to memorize — it's asking "if î and ĵ
went _here_, where does this vector go?"

## Matrix multiplication as composition

If a matrix is a transformation, then multiplying two matrices means applying
one transformation and then the other — a **composition**.

A rotation followed by a shear is a single new transformation, and the matrix
product is just that new transformation written down.

Two things to remember:

- **Order matters.** Shear-then-rotate and rotate-then-shear give different
  results. Geometrically this is obvious once you picture it, which is a much
  better reason to believe it than grinding through the algebra.
- **You read right to left**, like function notation. In `AB`, the `B` happens
  first.

### The formula

Writing $M_1$ for the transformation applied first and $M_2$ for the one
applied second:

$$
\underbrace{
\begin{bmatrix} a & b \\ c & d \end{bmatrix}
}_{M_2}
\underbrace{
\begin{bmatrix} e & f \\ g & h \end{bmatrix}
}_{M_1}
=
\begin{bmatrix}
ae + bg & af + bh \\
ce + dg & cf + dh
\end{bmatrix}
$$

Rather than memorizing that grid, it's worth seeing where it comes from. Take
the second column of $M_1$ — that's where $\hat{\jmath}$ lands after the first
transformation. Now push it through $M_2$:

$$
\begin{bmatrix} a & b \\ c & d \end{bmatrix}
\begin{bmatrix} f \\ h \end{bmatrix}
= f \begin{bmatrix} a \\ c \end{bmatrix}
+ h \begin{bmatrix} b \\ d \end{bmatrix}
= \begin{bmatrix} af + bh \\ cf + dh \end{bmatrix}
$$

That's the second column of the answer. Do the same with $M_1$'s first column
$(e, g)$ and you get the first column. So each column of the product is just
"track one basis vector all the way through both transformations" — and each
of those steps is itself a linear combination of $M_2$'s columns, scaled by
the entries of the vector.

The whole formula is those two column calculations sitting side by side.

## Takeaway

The thing that reframed the whole topic for me: a matrix isn't a grid of
numbers that you apply rules to. It's a _transformation of space_, written
down as where the basis vectors end up. Every rule about matrices — including
why multiplication is defined so strangely — follows from that.

## Sources

All four chapters are from 3Blue1Brown's
[Essence of Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)
playlist:

1. [Vectors, what even are they?](https://www.youtube.com/watch?v=fNk_zzaMoSs)
2. [Linear combinations, span, and basis vectors](https://www.youtube.com/watch?v=k7RM-ot2NWY)
3. [Linear transformations and matrices](https://www.youtube.com/watch?v=kYB8IZa5AuE)
4. [Matrix multiplication as composition](https://www.youtube.com/watch?v=XkY2DOUCWMU)
