---
title: "Week 2, Day 10 — Multivariable Calculus: Partial Derivatives, Gradient, and the Directional Derivative"
date: 2026-08-28 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, calculus, multivariable-calculus, khan-academy]
math: true
---

Day 10 steps out of single-variable calculus and into functions of several
variables. Just three ideas, but they're the ones that everything in machine
learning optimization is built on: the **partial derivative**, the
**gradient**, and the **directional derivative**. These are my notes,
cleaned up.

## Partial derivatives

A partial derivative is almost exactly an ordinary derivative — the only new
thing is that the function now has more than one input, say $f(x, y)$.

The trick is to look at one variable at a time. To take the partial with
respect to $x$, you **treat $y$ as a constant** and differentiate as usual.
To take the partial with respect to $y$, you freeze $x$ instead. Everything
you already know about derivatives carries over unchanged; you're just
holding the other variables still while you nudge one of them.

The notation switches from $d$ to $\partial$ to signal that other variables
are being held fixed. Written as a limit, the partial with respect to $x$ at
a point $(a, b)$ is:

$$
\frac{\partial f}{\partial x}(a, b) = \lim_{h \to 0} \frac{f(a + h, b) - f(a, b)}{h}
$$

Only the first slot gets the nudge $h$; the second stays pinned at $b$. The
partial with respect to $y$ is the mirror image — nudge the second slot,
freeze the first:

$$
\frac{\partial f}{\partial y}(a, b) = \lim_{h \to 0} \frac{f(a, b + h) - f(a, b)}{h}
$$

You can keep going. A **second partial derivative** is just differentiating
twice, again picking which variable to freeze each time. The one that
surprised me: the **mixed partial doesn't care about order**. Differentiating
by $x$ then by $y$ gives the same result as $y$ then $x$:

$$
\frac{\partial^2 f}{\partial x\, \partial y} = \frac{\partial^2 f}{\partial y\, \partial x}
$$

## Gradient

The **gradient** packs all of a function's first partial derivatives into a
single vector. For $f(x, y)$ that's:

$$
\nabla f = \begin{bmatrix} \dfrac{\partial f}{\partial x} \\[2mm] \dfrac{\partial f}{\partial y} \end{bmatrix}
$$

The $\nabla$ symbol ("nabla" or "del") is best read as a **vector of partial
derivative operators** waiting to be applied to a function. Feed it $f$ and
each slot fills in with the corresponding partial.

What makes it worth naming is that this bundle of numbers behaves like a
direction in the input space — it points the way the function increases
fastest, and that's exactly the object gradient descent walks against.

## Directional derivative

A partial derivative measures change along one of the axes. But you might want
to know the rate of change while moving in some **arbitrary direction**, not
just along $x$ or $y$. That's the directional derivative.

The clean way to compute it is a **dot product of your direction vector with
the gradient**:

$$
\nabla_{\vec{v}} f = \vec{v} \cdot \nabla f
$$

Written as a limit, it's the same nudge idea as before, except the nudge is
taken along $\vec{v}$ instead of along a single axis:

$$
\nabla_{\vec{v}} f(a) = \lim_{h \to 0} \frac{f(a + h\vec{v}) - f(a)}{h}
$$

This ties the three ideas together. The partial derivatives are directional
derivatives in the special directions of the axes; the gradient collects them
up; and the dot product lets you recover the rate of change in _any_
direction from that one collected object.

## Takeaway

This is the day the single-variable calculus from the rest of the week starts
pointing directly at machine learning.

A loss function has millions of inputs, not one. The gradient is how you take
its derivative anyway — one partial per parameter, bundled into a vector — and
the directional-derivative view explains _why_ stepping against the gradient
is the right move: the dot product $\vec{v} \cdot \nabla f$ is largest when
$\vec{v}$ points along $\nabla f$, so the negative gradient is the steepest
way down. Gradient descent is exactly that, repeated.

It also closes a loop with Week 1. The gradient is a vector, the directional
derivative is a dot product, and "which direction changes the output most" is
a projection question — the same linear-algebra machinery, now doing the work
inside calculus.

## Sources

Grant Sanderson's multivariable calculus videos for
[Khan Academy](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives),
covering the derivatives-of-multivariable-functions unit:

1. [Partial derivatives, introduction](https://www.youtube.com/watch?v=AXqhWeUEtQU)
2. [The gradient](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives/partial-derivative-and-gradient-articles/a/the-gradient)
3. [Gradient and directional derivatives](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives#gradient-and-directional-derivatives)
