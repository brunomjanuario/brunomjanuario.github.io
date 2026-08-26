---
title: "Week 2, Day 8 — Calculus: Limits, L'Hôpital's Rule, and the Fundamental Theorem"
date: 2026-08-19 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, calculus, 3blue1brown]
math: true
---

Day 8 closes out the derivative half of the playlist with limits — the idea
that's been hiding underneath every $dx$ so far — and then opens integration:
the reverse question, and the theorem that ties the two together.

## Limits: making "approaches" precise

Up to now $dx$ has been treated as a concrete, finitely small nudge — draw
it, do algebra with it, then drop whatever's still attached to it at the end.
That's a useful trick, but it's not actually a definition. Limits are what
makes it rigorous.

The notational shift matters: in the context of limits, that nudge isn't
called $dx$ anymore. It's $\Delta x$, or often just $h$ — a genuine variable
that we then ask a question about: **what happens as this thing approaches
$0$?**

$$
\lim_{h \to 0} \frac{f(x+h) - f(x)}{h}
$$

The phrase "approaches" is the part that needs pinning down, and that's what
the epsilon-delta definition does.

## The epsilon-delta definition

Say the limit of $f$ as $x \to c$ is $L$. Pick any tolerance you like on the
output — call it $\varepsilon$, as small as you want. The claim is that
there's always some corresponding range around the input, of width
$\delta$, such that:

**every input within $\delta$ of $c$ produces an output within $\varepsilon$
of $L$.**

$$
0 < |x - c| < \delta \implies |f(x) - L| < \varepsilon
$$

And crucially, this has to work for *every* $\varepsilon$ — shrink the
output tolerance as much as you like, and there must still be some $\delta$
that squeezes the corresponding inputs down to match. The limit isn't a
statement about one tolerance; it's a guarantee that a tight enough $\delta$
always exists, no matter how tight you make $\varepsilon$ first.

That gives a clean way to say a limit *doesn't* exist: no matter how small
you shrink $\delta$ around $c$, the corresponding range of outputs stays too
big. There's no candidate $L$ that all of those outputs are eventually
squeezed within $\varepsilon$ of — shrinking the input window never shrinks
the output window to match.

## L'Hôpital's rule

Derivatives, it turns out, can be used to evaluate limits — including ones
that look undefined at first glance, like $0/0$.

If $f(a) = g(a) = 0$, the ratio $f(x)/g(x)$ looks meaningless right at $a$.
But zoom in close enough and both $f$ and $g$ are approximately linear near
$a$ — that's what a derivative *is*, the local slope. So near $a$:

$$
f(x) \approx f'(a)\,(x - a) \qquad g(x) \approx g'(a)\,(x - a)
$$

Divide, and the $(x-a)$ factors cancel:

$$
\lim_{x \to a} \frac{f(x)}{g(x)} = \frac{f'(a)}{g'(a)}
$$

This is exactly the tiny-nudge habit from the last few days, made rigorous:
both numerator and denominator are shrinking toward zero as $x \to a$, but
they're shrinking at *rates* given by their derivatives, and it's that ratio
of rates — not the vanishing values themselves — that the limit actually
measures.

## Integration: area as accumulated output

Switch questions entirely. Instead of *rates of change*, integration asks
about *accumulation*.

Take a function of time whose output is velocity. The area under that curve,
between two points in time, is the distance traveled. That's not a metaphor
— it falls straight out of chopping time into tiny slices $dt$, treating
velocity as roughly constant across each sliver, multiplying (velocity ×
time = distance for that sliver), and adding all the slivers up. The area
under the curve *is* that sum, in the limit as the slivers shrink to zero
width.

$$
\text{distance} = \int_{a}^{b} v(t)\,dt
$$

## Signed area, not just area

One detail that trips people up: whenever the graph dips below the
horizontal axis, that region counts as **negative** area. Integrals don't
measure area in the everyday sense — they measure **signed area**.

That's exactly consistent with the velocity picture: negative velocity means
moving backward, and moving backward should *subtract* from net distance
traveled, not add to it. A car that drives forward for a while and then
backs up the same amount ends up with zero net displacement — and the
integral reflects that by having the region above the axis cancel the region
below it.

## Takeaway

Limits are the thing that lets $dx$-style hand-waving become actual
mathematics: instead of "small enough to ignore," you get a precise
guarantee — for any output tolerance, an input window exists that delivers
it. L'Hôpital's rule is that machinery paying off directly, turning an
undefined $0/0$ into a clean ratio of derivatives. And integration flips the
whole derivative story around: instead of asking how fast something changes,
it asks how much has accumulated, with area above the axis counted for and
area below counted against.

## Sources

3Blue1Brown's [Essence of Calculus](https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr)
playlist:

7. [Limits, L'Hôpital's rule, and epsilon delta definitions](https://www.youtube.com/watch?v=kfF40MiS7zA)
8. [Integration and the fundamental theorem of calculus](https://www.youtube.com/watch?v=rfG8ce4nNh0)
