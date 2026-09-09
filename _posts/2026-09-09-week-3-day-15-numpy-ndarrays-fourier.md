---
title: "Week 3, Day 15 — NumPy: N-Dimensional Arrays, Strides, and the Fourier Transform"
date: 2026-09-09 00:00:00 +0000
categories: [AI Fundamentals]
tags: [ai-fundamentals, python, numpy, fourier-transform]
math: true
---

Day 15 closes out NumPy by going past two dimensions. **N-dimensional arrays**
and the `axis` argument, **images** as plain arrays of numbers, **strides** —
how NumPy actually lays memory out — and the **Fourier transform** for moving
between a signal and its frequencies. These are my notes, cleaned up.

## N-dimensional arrays

Nothing about arrays stops at 2D. The same `ndarray` handles 3, 4, or more
dimensions — you just supply more indices, one per axis:

```python
three_dim_array[1, 0, 1]
```

And the constructors take a shape tuple of any length. A 4-dimensional array of
zeros is no harder to make than a matrix:

```python
np.zeros((3, 2, 2, 2))   # 4D: shape (3, 2, 2, 2)
```

Slicing generalises the same way — one slice per axis, comma-separated, and `:`
still means "everything on this axis":

```python
array[0, 0:2, :]   # index 0 on axis 0, first two on axis 1, all of axis 2
```

### The `axis` argument

This is the idea that makes higher dimensions usable. Aggregations from Day 12
collapse the *whole* array by default:

```python
array.sum()   # one number: the total of every element
```

But passing `axis` tells NumPy **which dimension to collapse**, leaving the
others intact:

```python
my_matrix.sum(axis=0)    # collapse the rows -> one total per column
my_matrix.mean(axis=0)   # collapse the rows -> one average per column
```

The wording that finally stuck for me: `axis=0` doesn't mean "operate on rows",
it means **"eliminate axis 0"**. For a `(3, 4)` matrix, summing with `axis=0`
removes the 3 and leaves shape `(4,)` — one value per column. With `axis=1` it
removes the 4 and leaves `(3,)` — one value per row.

Reading it as "the axis that disappears" makes the result shape predictable
without guessing.

## Images are just arrays

The cleanest demonstration that arrays aren't abstract: an image *is* a 3D
array. `scikit-image` ships sample images to play with.

```python
from skimage import data
import matplotlib.pyplot as plt

cat = data.chelsea()
plt.imshow(cat)
```

And its shape tells you exactly what it is:

```python
cat.shape   # (300, 451, 3)
```

The **first two numbers are the spatial dimensions** — height and width in
pixels — and the **third is the colour channel**, the 3 being RGB. So the
picture is a grid of pixels where each pixel holds three numbers.

Which means everything from the last four days applies to images directly.
Slicing crops. Boolean masks select pixels by brightness. `cat[:, :, 0]` pulls
out just the red channel. Image processing turns out to be array manipulation
with a nicer viewer.

## Strides

Strides are the mechanism underneath all of it, and understanding them explains
several things that otherwise look like magic.

```python
my_array.strides
```

An array is **stored in memory as one flat, contiguous run of bytes** — a single
row — even when it's displayed as a grid. The **strides** are how many bytes to
jump to take one step along each axis. That tuple is what turns flat memory into
an apparently multi-dimensional shape.

For a `(3, 4)` array of 8-byte values, the strides are `(32, 8)`: moving one
column over is 8 bytes forward, and moving one row down is 32 bytes — a whole
row of four.

The payoff is `.transpose()`. Transposing **doesn't move any data** — it just
**reverses the strides**, so `(32, 8)` becomes `(8, 32)` and the same bytes get
read column-wise instead of row-wise. That's why transposing a huge matrix is
instant, and why it returns a view rather than a copy (the Day 12 distinction
again).

## Complex numbers

Python has complex numbers built in, and NumPy uses them heavily — which
matters for the next section. The imaginary unit is written **`1j`**, not `i`:

```python
imag = 1j
complex_num = 3 + 2j
```

The two parts come off as **attributes, not method calls**:

```python
complex_num.real   # 3.0
complex_num.imag   # 2.0
```

## The Fourier transform

Here's the big idea of the day. A **Fourier transform takes a signal and turns
it into frequencies** — instead of "what is the value at each moment in time",
you get "how much of each frequency is present". The **inverse** transform
takes the frequencies back to a signal.

$$
\text{signal (time domain)} \;\xrightarrow{\;\text{FFT}\;}\; \text{frequencies}
\;\xrightarrow{\;\text{inverse FFT}\;}\; \text{signal}
$$

It lives in its own module:

```python
from numpy import fft
```

Transforming a signal gives back an **array of complex numbers** — which is why
complex numbers came first. Each one encodes both the strength and the phase of
a frequency:

```python
f_f = fft.rfft(simple_signal)
```

`rfft` is the variant for **real-valued** input (an ordinary signal), which is
the common case.

The transform gives you magnitudes but not the frequencies they belong to.
`rfftfreq` builds that axis, from the number of samples and the **spacing
between them**:

```python
frequency = fft.rfftfreq(samples, sample_spacing)
plt.plot(frequency, f_f.real)
```

Plot the frequency axis against the transform and you can *see* which
frequencies the signal is made of — spikes where a frequency is present.

Going back is one call:

```python
fft.irfft(f_f)   # frequencies -> signal
```

### Smoothing a signal

This is where it stops being a party trick. If noise lives at high frequencies,
you can **delete those frequencies and transform back** — that's a low-pass
filter, and it's four lines:

```python
f_f = fft.rfft(signal)
frequency_domain = fft.rfftfreq(600, 1/300)   # 600 samples, spacing 1/300

f_f[frequency_domain > 40] = 0                # zero out everything above 40 Hz

smooth = fft.irfft(f_f)
```

The middle line is the boolean-mask idiom from Days 13 and 14 doing real work:
`frequency_domain > 40` is a mask, and assigning `0` through it wipes out every
component above 40 Hz. Transform back, and the noise is gone.

The reason this is worth the detour: smoothing is *hard* in the time domain and
*trivial* in the frequency domain. Changing what you're looking at changes what
counts as a difficult problem.

### 2D Fourier transform

Since an image is just a 2D array, the transform generalises to it:

```python
from skimage.data import camera

fft.rfft2(camera())
```

Same idea in two dimensions — the frequencies are now spatial. Low frequencies
are broad gradients, high frequencies are edges and fine detail, which is
roughly how image compression decides what to throw away.

## Takeaway

Week 3 was learning the tool; today was the day the tool stopped feeling like
a container for numbers.

The thread running through it is **representation**. An image is a 3D array. A
grid is flat memory plus a stride tuple. A signal is a set of frequencies. In
every case the underlying data doesn't change — only the way you address it —
and the right representation makes an operation either trivial or impossible.
Transposing is free because strides can be swapped; smoothing is easy because
frequencies can be zeroed.

The `axis` argument is the thing I'll use most day to day, and reading it as
"the axis that disappears" is the version of it I want to keep. Everything else
here — masks, views, shapes — is machinery from earlier in the week showing up
again, just in more dimensions.

## Sources

Udemy — [Scientific Computing with NumPy](https://www.udemy.com/course/scientific-computing-with-numpy/),
plus the official docs for reference:

1. [Discrete Fourier Transform (`numpy.fft`)](https://numpy.org/doc/stable/reference/routines.fft.html)
2. [ndarray internals and strides](https://numpy.org/doc/stable/reference/arrays.ndarray.html)
3. [scikit-image sample data](https://scikit-image.org/docs/stable/api/skimage.data.html)
