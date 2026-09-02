---
title: "Week 2, Day 11 — Probability: Random Variables, Distributions, and the Central Limit Theorem"
date: 2026-09-01 09:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, probability, statistics, seeing-theory]
math: true
---

Day 11 opens a new subject: **probability**. Worked through the
first chapters of Brown's _Seeing Theory_ — basic probability, compound
events, the common distributions, and the Central Limit Theorem. These are my
notes, cleaned up.

## Basic probability

Probability theory is the framework for reasoning about chance events in a
logically sound way. The **probability of an event** is a number saying how
likely it is to occur, always between 0 and 1: $0$ means impossible, $1$ means
certain.

The classic example is a fair coin toss — two outcomes, heads or tails, each
with probability $\tfrac{1}{2}$.

The moment you **assign numbers to outcomes** — say 1 for heads, 0 for tails —
you've created a **random variable**. That's the bridge from "things that can
happen" to "numbers you can do math on."

Two numbers summarize a random variable:

- **Expectation** — a number that tries to capture the _center_ of the
  distribution. Roll a fair die many times and the running sample mean
  converges to the expectation, $3.5$.
- **Variance** — where expectation measures centrality, variance measures
  _spread_. It's the average squared difference between the random variable
  and its expectation:

$$
\operatorname{Var}(X) = \mathbb{E}\big[(X - \mathbb{E}[X])^2\big]
$$

Squaring is what makes it a measure of spread: it keeps deviations on both
sides positive so they don't cancel, and it punishes large deviations more
than small ones.

## Compound probability

To talk about combinations of events, probability borrows **set notation**. A
set is just a collection of objects, and it lets you specify compound events
cleanly: the event "roll an even number" is the set $\{2, 4, 6\}$.

The idea that mattered most here is **conditional probability** — updating a
probability to account for information you already have. The probability it
rains tomorrow _in general_ is different from the probability it rains
tomorrow _given that it's cloudy today_. The second is a conditional
probability, written $P(\text{rain} \mid \text{cloudy})$, because it folds in
the relevant evidence.

## Probability distributions

A **probability distribution** specifies the relative likelihoods of all
possible outcomes. Formally, a random variable is a **function that assigns a
real number to each outcome** in the probability space, and its distribution
is how likelihood is spread across those numbers.

Distributions split into two major classes:

- **Discrete** — a finite or countable number of possible values.
- **Continuous** — values over a continuous range.

The discrete ones from the chapter, each solving a specific counting problem:

- **Bernoulli** — takes value 1 with probability $p$ and 0 with probability
  $1 - p$. The model for a single binary experiment, like one coin toss.
- **Binomial** — the sum of $n$ independent Bernoulli variables with the same
  $p$. Models the number of successes in $n$ identical binary trials, e.g. the
  number of heads in five tosses.
- **Geometric** — counts the number of trials needed to see the _first_
  success, each trial independent with success probability $p$. E.g. how many
  rolls until a six shows up.
- **Poisson** — counts the number of events in a fixed interval of time or
  space, given an average rate $\lambda$. Used for things like meteor showers
  or goals in a soccer match.
- **Negative binomial** — counts the number of successes before $r$ failures
  occur in a sequence of Bernoulli trials. E.g. how many heads before three
  tails.

The pattern I took away: they're all built out of the same Bernoulli atom,
just asking different questions about it — _how many successes_ (binomial),
_how long until one_ (geometric), _how many before some number of failures_
(negative binomial).

## Central Limit Theorem

The **Central Limit Theorem** is the one that ties it together. It says that
the **sample mean of a large enough number of i.i.d. random variables is
approximately normally distributed** — and the larger the sample, the better
the approximation.

The striking part is that it _doesn't matter what distribution you started
with_. Average enough independent draws from almost any distribution and the
distribution of that average drifts toward the same bell curve.

## Takeaway

This is the day the subject shifts from "how functions change" to "how to
reason under uncertainty" — the language the rest of machine learning is
written in.

Two ideas feel foundational. **Expectation and variance** are the summary
statistics that show up everywhere downstream — a loss is an expected error, a
model's uncertainty is a variance. And the **Central Limit Theorem** is the
quiet reason the normal distribution is the default assumption almost
everywhere: it's not arbitrary, it's what you get whenever many small
independent effects add up.

There's also a thread back to earlier weeks. A random variable is a
**function** (Week 2's framing), and expectation is a weighted sum — a dot
product between outcomes and their probabilities — which is Week 1's linear
algebra showing up again inside probability.

## Sources

Brown University's [Seeing Theory](https://seeing-theory.brown.edu/index.html),
an interactive visual introduction to probability and statistics:

1. [Basic Probability](https://seeing-theory.brown.edu/basic-probability/index.html)
2. [Compound Probability](https://seeing-theory.brown.edu/compound-probability/index.html)
3. [Probability Distributions](https://seeing-theory.brown.edu/probability-distributions/index.html)
