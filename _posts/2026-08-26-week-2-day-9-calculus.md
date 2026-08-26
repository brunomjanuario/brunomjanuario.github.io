---
title: "Week 2, Day 9 — Calculus: Averages, Taylor Series, and Derivatives as Stretching"
date: 2026-08-26 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, calculus, 3blue1brown]
math: true
---

Day 9 finishes _Essence of Calculus_: chapters 9 through 12. Average values,
higher order derivatives, Taylor series, and a completely different way to
picture what a derivative is. These are my notes, cleaned up.

## What does area have to do with slope?

The chapter is built around a question that sounds unrelated to calculus:
what's the **average value** of a continuous function over an interval?

You can't add up infinitely many values and divide by infinity. But you
already have a tool for "add up infinitely many things" — the integral. So
add up the heights with an integral, then divide by how wide the interval
is:

$$
\text{average} = \frac{1}{b-a}\int_a^b f(x)\,dx
$$

Area divided by width. It's the same thing as asking what height a rectangle
would need to have, over that same interval, to enclose the same area.

And to actually compute that integral, you need the **antiderivative** — a
function $F$ whose derivative is $f$ — and then evaluate it at both ends:

$$
\int_a^b f(x)\,dx = F(b) - F(a)
$$

Which is where the title comes from. Rewrite the average with that in mind:

$$
\frac{1}{b-a}\int_a^b f(x)\,dx = \frac{F(b) - F(a)}{b - a}
$$

The right-hand side is rise over run — the **average slope** of $F$ between
$a$ and $b$. So the average height of a function is the average slope of its
antiderivative. Area and slope aren't two separate subjects that happen to be
inverses; that inverse relationship is the whole point.

## Higher order derivatives

Once you have a derivative, it's a function too, so you can differentiate it
again.

The **second derivative** tells you how much the derivative itself is
changing — how the rate of change is changing. Graphically it's curvature:
positive means the curve bends upward, negative means it bends downward,
near-zero means it's locally close to a straight line.

The car example from Day 6 gives the cleanest naming:

- position $s(t)$ — where the car is,
- first derivative — **velocity**, how fast the position changes,
- second derivative — **acceleration**, how fast the velocity changes,
- third derivative — **jerk**, how fast the acceleration changes.

Jerk is the one you feel as a passenger: constant acceleration presses you
into the seat steadily, but a jerk is the acceleration itself changing, which
is the lurch.

## Taylor series

Taylor series are about **approximating a function near a point with a
polynomial**, and the strategy is to match derivatives.

The idea: pick a point, then build a polynomial whose value matches the
function there, whose slope matches, whose second derivative matches, whose
third matches, and so on. Each new term buys you agreement one derivative
deeper, which buys accuracy over a wider neighborhood.

The subtlety — and the part my notes flagged — is that you can't just set
each coefficient equal to the derivative you want it to produce. Take $x^n$
and differentiate it $n$ times, letting the power rule cascade:

$$
x^n \to n x^{n-1} \to n(n-1)x^{n-2} \to \cdots \to n!
$$

Every differentiation pulls a factor down out front, so by the time you've
done it $n$ times the term has picked up $1 \cdot 2 \cdot 3 \cdots n = n!$.
That factorial has to be cancelled out, so each coefficient gets divided by
it:

$$
f(x) \approx \sum_{n=0}^{N} \frac{f^{(n)}(a)}{n!}(x-a)^n
$$

Written out around $a = 0$, that's:

$$
f(x) \approx f(0) + f'(0)\,x + \frac{f''(0)}{2!}x^2 + \frac{f'''(0)}{3!}x^3 + \cdots
$$

The factorial isn't a decoration — it's there to undo the cascade the power
rule creates. And what the formula is really saying is that **all the
information about a function near a point is encoded in its derivatives at
that point**: derivative information at a single input, converted into
function behavior in a neighborhood around it.

## The other way to visualize derivatives

Up to here, every mental picture has been a graph: derivative as slope,
integral as area. Chapter 12 throws the graph away.

Instead, picture the input as a **point on a number line**, and the function
as something that moves it to a new spot on an output line. A function is
then a transformation of the line, not a curve above it.

Now zoom in near one input and mark a few evenly spaced points around it.
Push them all through the function and look at where they land. They won't
still be evenly spaced by the same amount — they'll be spread apart or
squeezed together. **The derivative at that input is the factor by which they
got spread or contracted.** A derivative of 3 means the neighborhood gets
stretched to three times its size; a derivative of $\tfrac{1}{2}$ means it
gets squeezed to half. A negative derivative means the neighborhood gets
flipped.

That's the same number as the slope, just read off a different picture — and
this picture is the one that survives when the graph stops being available,
because it doesn't need an output axis at all.

It also gives a clean way to think about **repeatedly applying** a function.
A **fixed point** is an input the function leaves alone, $f(x) = x$. Whether
nearby points get pulled into it or pushed away from it depends entirely on
the derivative there:

- $\lvert f'(x) \rvert < 1$ — the neighborhood contracts, nearby points get
  drawn in. The fixed point is **stable**.
- $\lvert f'(x) \rvert > 1$ — the neighborhood expands, nearby points get
  pushed away. The fixed point is **unstable**.

So stability is decided by a magnitude compared against one, which is a
surprisingly concrete payoff from an abstract change of picture.

## Takeaway

Two things stood out on the last day of the playlist.

Taylor series, because of what they claim: knowing a function's derivatives
at one point tells you how it behaves _around_ that point. That's the
justification for every method that takes a local measurement and uses it to
decide on a step — gradient descent included, which is a first-order Taylor
approximation with the higher terms dropped.

And the number-line picture, because it reframes the derivative as a
**stretching factor** rather than a slope. That framing carries straight over
to linear algebra from Week 1 — a matrix stretches space, a derivative
stretches a neighborhood — and the stability rule (magnitude above or below
one) is the same reasoning behind why repeated multiplication either explodes
or dies out.

## Sources

Chapters 9 to 12 of 3Blue1Brown's
[Essence of Calculus](https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr)
playlist:

9. [What does area have to do with slope?](https://www.youtube.com/watch?v=FnJqaIESC2s)
10. [Higher order derivatives](https://www.youtube.com/watch?v=BLkz5LGWihw)
11. [Taylor series](https://www.youtube.com/watch?v=3d6DsjIBzJ4)
12. [The other way to visualize derivatives](https://www.youtube.com/watch?v=CfW845LNObM)
