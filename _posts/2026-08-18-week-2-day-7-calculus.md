---
title: "Week 2, Day 7 — Calculus: Derivative Rules, e, and Implicit Differentiation"
date: 2026-08-18 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, calculus, 3blue1brown]
math: true
---

Day 6 was the two ideas behind calculus. Day 7 is the machinery: chapters 3
through 6 of _Essence of Calculus_ — where the derivative rules come from,
why $e$ is the exponential that matters, and what implicit differentiation is
actually doing. These are my notes, cleaned up.

## Derivative formulas through geometry

Every rule in this chapter comes out of the same move: **take the tiny nudge
seriously and draw it.**

Start with $f(x) = x^2$, and picture it as the area of a square with side
$x$. Nudge the side by $dx$ and the area grows by three pieces: a thin
rectangle along the bottom, an identical one up the right side, and a tiny
square in the corner.

$$
d(x^2) = \underbrace{2x\,dx}_{\text{two rectangles}} + \underbrace{(dx)^2}_{\text{corner}}
$$

Divide by $dx$:

$$
\frac{d(x^2)}{dx} = 2x + dx
$$

And here's the habit that runs through the whole subject: **anything still
carrying a $dx$ gets dropped.** As $dx$ shrinks toward zero, $2x$ stays put
and the corner square becomes irrelevant — it's the area of a square whose
side is already tiny, so it vanishes faster than the thing it's competing
with. Left with $2x$.

The same picture in 3D — a cube of side $x$ nudged by $dx$ — gives three
slabs of area $x^2$ each, plus edge and corner scraps carrying $(dx)^2$ and
$(dx)^3$. Drop the scraps, and $d(x^3)/dx = 3x^2$.

Two data points, and the pattern generalizes to the **power rule**:

$$
\frac{d}{dx} x^n = n x^{n-1}
$$

It's worth naming what makes this easy, because it's the recurring gift of
calculus: **a chunk of the complexity is allowed to be ignored**, purely
because we're heading toward zero. You don't have to be exact about the
corner square. You just have to know it dies faster than the terms you kept.

Sine works the same way, except the picture is the unit circle instead of a
square. Nudging the angle $\theta$ moves you a tiny distance along the
circumference, and the vertical component of that motion — the change in
$\sin(\theta)$ — is governed by the slope of the circle there, which is
exactly $\cos(\theta)$:

$$
\frac{d}{d\theta}\sin(\theta) = \cos(\theta)
$$

## The sum, product, and chain rules

Three rules for combining functions, and only one of them is boring.

**Sums.** The derivative of a sum is the sum of the derivatives. Nothing to
picture, nothing to remember.

**Products.** Picture $f(x) \cdot g(x)$ as the area of a rectangle: width
$f$, height $g$. Nudge $x$ and both sides move a little, so the area gains a
strip along the bottom and a strip up the side (plus the corner, which we
drop as usual):

$$
d(fg) = \underbrace{f \, dg}_{\text{bottom strip}} + \underbrace{g \, df}_{\text{side strip}}
$$

$$
\frac{d}{dx}\big(f(x)g(x)\big) = f(x)g'(x) + g(x)f'(x)
$$

The mnemonic that goes with the picture is **"left d(right) + right
d(left)"** — one strip for each side that moved.

**Composition.** For $g(h(x))$, nudging $x$ moves $h$, and moving $h$ moves
$g$. Two nudges chained together, so you multiply the sensitivities:

$$
\frac{dg}{dx} = \frac{dg}{dh} \cdot \frac{dh}{dx}
$$

Written in Leibniz notation the rule looks almost like cancelling fractions,
and that's not an accident — it's the ratio of nudges at each stage, composed.
This is the rule that ends up mattering most later: backpropagation is the
chain rule applied layer by layer through a network.

## What's so special about $e$?

Ask what the derivative of $2^t$ is, and you get something suspicious. Work
out the nudge:

$$
\frac{2^{t+dt} - 2^t}{dt} = 2^t \cdot \frac{2^{dt} - 1}{dt}
$$

The $2^t$ factors straight out, and what's left doesn't depend on $t$ at all
— it's just a number, roughly $0.6931$. So:

$$
\frac{d}{dt} 2^t = 0.6931 \cdot 2^t
$$

**The derivative of an exponential is proportional to itself.** Not equal to
itself — proportional, with some mystery constant out front that depends on
the base. Try base 3 and you get $1.0986$. Base 8, $2.0794$.

The obvious question is which base makes that constant exactly $1$, so the
function is _literally_ its own derivative. That base is
$e \approx 2.71828$ — **Euler's number** — and it's the reason $e^t$ is the
exponential everyone actually writes:

$$
\frac{d}{dt} e^t = e^t
$$

And the mystery constant stops being mysterious: it's the **natural log of
the base**. That's not a coincidence either, because any exponential can be
rewritten in base $e$:

$$
2^t = e^{\ln(2)\,t}
\qquad \Longrightarrow \qquad
\frac{d}{dt} 2^t = \ln(2)\, e^{\ln(2)t} = \ln(2)\cdot 2^t
$$

The chain rule pulls the $\ln(2)$ out front, and $\ln(2) = 0.6931$ — the
number from the start. So there aren't really many exponential functions with
many different constants. There's $e^{ct}$, and every other base is that
function wearing a different label.

Which also explains why $e$ is everywhere in ML rather than being a quirky
constant: sigmoid, softmax, and cross-entropy are all written in base $e$
because that's the base where the calculus stays clean.

## Implicit differentiation

The last chapter handles the case where **$y$ isn't isolated on one side** —
you can't write $y = f(x)$, so there's nothing obvious to differentiate.

The example is a circle, $x^2 + y^2 = 5^2$, and the question is the slope of
the tangent line at a point on it, say $(3, 4)$.

The trick is to treat both $x$ and $y$ as things that get nudged, and
differentiate both sides of the equation:

$$
2x\,dx + 2y\,dy = 0
$$

Then just solve for the ratio:

$$
\frac{dy}{dx} = -\frac{x}{y} = -\frac{3}{4}
$$

What makes this feel strange at first is differentiating an equation rather
than a function. But the reasoning is the same tiny-nudge argument as
everywhere else: the point has to _stay on the curve_, so whatever the nudge
does to $x^2$ must be cancelled by what it does to $y^2$. The expression
$x^2 + y^2$ is pinned at $25$, so its total change is zero, and that
constraint is the whole equation.

## Takeaway

Every rule today came from the same place — nudge the input, draw what
changes, throw away the pieces that shrink faster than $dx$. The power rule is
a square growing, the product rule is a rectangle growing, and the chain rule
is two nudges passed along in sequence.

That last one is the one to actually internalize. Training a model is the
chain rule run backwards through a stack of functions, and $e$ shows up
throughout that stack for exactly the reason in chapter 5: it's the base that
differentiates into itself.

## Sources

Chapters 3 to 6 of 3Blue1Brown's
[Essence of Calculus](https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr)
playlist:

3. [Derivative formulas through geometry](https://www.youtube.com/watch?v=S0_qX4VJhMQ)
4. [Visualizing the chain rule and product rule](https://www.youtube.com/watch?v=YG15m2VwSjA)
5. [What's so special about Euler's number e?](https://www.youtube.com/watch?v=m2MIpDrF7Es)
6. [Implicit differentiation, what's going on here?](https://www.youtube.com/watch?v=qb40J4N1fa4)
