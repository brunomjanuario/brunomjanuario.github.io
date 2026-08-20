---
title: "Week 1, Day 6 — Calculus: Integrals, Derivatives, and the Paradox"
date: 2026-08-17 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, calculus, 3blue1brown]
math: true
---

Linear algebra is done, and it closed with matrix multiplication in raw
Python. Calculus is next, and I started it the same way I started the last
topic: 3Blue1Brown, first two chapters of _Essence of Calculus_. These are my
notes, cleaned up.

## Inventing calculus from a circle

The series opens by deriving the area of a circle rather than quoting it, and
the derivation is the whole subject in miniature.

Take a circle of radius $R$ and slice it into concentric rings. Pick one ring
at radius $r$ with a tiny thickness $dr$. Unroll it and it's very nearly a
rectangle: length $2\pi r$ (the circumference at that radius), height $dr$.
So its area is:

$$
2\pi r \, dr
$$

The approximation gets better the thinner the ring is, because a thinner ring
has less difference between its inner and outer circumference — there's less
room for it to be a trapezoid instead of a rectangle.

Now line those rectangles up side by side under the graph of $2\pi r$, with
$r$ running from $0$ to $R$. Each rectangle's area is one ring's area, so the
total area of the circle is **the area under that graph**. And $2\pi r$ is a
straight line, so the area under it is just a triangle:

$$
\frac{1}{2} \cdot R \cdot 2\pi R = \pi R^2
$$

The formula I memorized as a kid falls out of a triangle.

The pattern underneath is the one worth keeping:

> A hard problem becomes a sum of many small values, each of which is easy.

That's the move calculus makes over and over. The `dr` is the knob: the
smaller you make it, the more terms in the sum and the better the
approximation, and the exact answer is what the sum approaches as $dr \to 0$.

## Integrals and derivatives, and the link between them

The **integral** of $f(x)$ is the area under its graph. Call that area
function $A(x)$ — the area accumulated from the start up to $x$.

Now nudge $x$ by a tiny amount $dx$. The area grows by a sliver $dA$, and
that sliver is almost exactly a rectangle: width $dx$, height $f(x)$. So:

$$
dA \approx f(x) \, dx
\qquad \Longrightarrow \qquad
\frac{dA}{dx} \approx f(x)
$$

That ratio — a slight nudge to the output, divided by the slight nudge to the
input that caused it — is the **derivative**. And what it just told us is
that the derivative of the area-under-a-graph function gives you back the
function defining the graph.

That's the **fundamental theorem of calculus**, and the reason the two halves
of the subject are one subject: integration and differentiation are inverse
operations.

The other framing of the derivative that stuck with me: it's a measure of
**how sensitive a function is to small changes in its input**. Which, looking
ahead to gradient descent, is exactly the question you ask about a loss
function.

## The paradox of the derivative

Chapter 2 sets up the problem with a car driving from A to B. Plot distance
$s$ against time $t$, and velocity is $ds/dt$ — how much the distance changed,
divided by how much time passed.

The paradox is in the phrase "instantaneous rate of change". Change over
_what_? At a single instant, no time passes and no distance is covered. The
car doesn't move during an instant, so there's nothing to take a ratio of.

Work it through with $s(t) = t^3$. Over a real, finite window $dt$:

$$
\frac{ds}{dt}
= \frac{(t + dt)^3 - t^3}{dt}
= \frac{3t^2\,dt + 3t\,(dt)^2 + (dt)^3}{dt}
= 3t^2 + 3t\,dt + (dt)^2
$$

Two of those three terms have a $dt$ in them, so they shrink to nothing as
$dt$ gets smaller. What survives is:

$$
\frac{ds}{dt} = 3t^2
$$

Notice what actually happened: I never set $dt = 0$. If I had, the very first
step would have been $0/0$. The derivative is what the ratio _approaches_ as
$dt$ shrinks — the terms that don't depend on the size of the window.

So the resolution to the paradox is that the derivative isn't a rate of
change at an instant at all. It's the **best constant approximation to the
rate of change around that point** — "instantaneous rate of change" is a
conceptual shorthand, and taking it literally is what makes it sound
contradictory.

## Takeaway

Two ideas carried both chapters. First, hard quantities become sums of many
small easy ones, and the exact answer is the limit as those pieces shrink.
Second, the derivative is a sensitivity: how much the output moves when you
nudge the input, with the terms that depend on the size of the nudge thrown
away.

Neither is really about areas or cars. They're about what happens to a
function when you push on it slightly — which is the entire mechanism behind
training a model.

## Sources

Both chapters from 3Blue1Brown's
[Essence of Calculus](https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr)
playlist:

1. [The essence of calculus](https://www.youtube.com/watch?v=WUvTyaaNkzM)
2. [The paradox of the derivative](https://www.youtube.com/watch?v=9vKqVkMQHKk)
