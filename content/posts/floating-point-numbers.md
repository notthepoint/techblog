---
title: "What are Floating Point Numbers?"
date: 2019-08-07T15:08:23+01:00
draft: false
---

Take a look at the following calculation, written in Ruby.

```ruby
0.1 + 0.2
=> 0.30000000000000004
```

You may be wondering why the answer isn't simply `0.3`. Well, it is because we just encountered a floating point error. High five! 🙌

Let's break it down to figure out why that happens.

To start thinking about floating numbers we can think about the problem of storing numbers in a computer. Numbers have infinite range, from tiny weeny numbers like the approximate mass of an electron (0.000000000000000000000000000000911) to mahoooosive numbers like the estimate for the mass of the earth (5,972,200,000,000,000,000,000,000).

The problem is that a computer has limited memory so can't store numbers with infinite precision. They have to be cut off at some point, but where? How accurate is accurate enough?


### Computers store a number and its exponential

When dealing with very large numbers and/or very small numbers a computer would need to store a lot of data! How can we reduce the storage whilst keeping accuracy?

Let's again take the approximate mass of an electron (0.000000000000000000000000000000911) and the estimate for the mass of the earth (5,972,200,000,000,000,000,000,000). That's a lot of 0s that need storing! However, if we were to store numbers like their scientific notation, we could have high accurary with low storage. In scientific notation we have;


$$0.000000000000000000000000000000911 = 9.11 \times 10^-31$$
$$5972200000000000000000000 = 5.9722 \times 10^{24}$$.


This is how floating point numbers work, they store two parts for any number.

1. The number's digits (otherwise known as the significand)
2. Where the 'point' should be in the number (its exponent) relative to the beginning of the significand

Below are some example numbers with their scientific notation form and the significand and exponents.

|Value|Scientific Notation|Significand|Exponent|
|---|---|---|---|
| 197100  | $$1971 \times 10^2$$ | 1971 | 2|
| 230000000000| $$23 \times 10^{10}$$  | 23 | 10|
| 0.0000001401| $$1401 \times 10^{-10}$$  | 1401 | -10 |
| 1313000 | $$1313 \times 10^3$$ | 1313 | 3 |


### Computers use base 2

The numbers we use in everyday life are base 10. That is, each digit represents a multiple of 10. For example,

```
312 = (3 * 100) + (1 * 10) + (2 * 1)
```

Computers, however, deal with base 2.
For example, the number 12 in base two is `1100`, that is,

```
12 = (1 * 8) + (1 * 4) + (0 * 2) + (0 * 1)
```


### Computers store each number in 32 bits (or sometimes 64 bits)

Computers store numbers in exact spaces of 32 bits (or sometimes 64 bits). It is because of this that we end up with floating point errors.

Let's run through an example to understand how floating point errors occur.
Let's first take 1/3 in base 10. We know the following to be true.

1/3 + 1/3 + 1/3 = 1

In base 10, 1/3 is $0.3333\overline{3}$. The 3 repeats infinitely. So if we were add up 0.3333... we end up with the following.

$$0.33333... + 0.33333... + 0.333333 = 0.999999...$$

It doesn't _quite_ add up to 1. A rounding error!

Now let's take an example in base 2. We can take the number `0.1`.

0.1 in base 2, much like 1/3 in base 10, cannot be represented exactly. In the table below we have 0.1 in base 2 to 5 significant figures.

|4|2|1|.|1/2|1/4|1/8|1/16|1/32|
|---|---|---|---|---|---|---|---|---|
|0|0|0|.|0|0|0|1|1|

Written down with more significant figures we see a pattern.

$$0.0\overline{0011} = 0.00011001100110011......$$

The 0011 repeats for inifinity (and beyond).

So to store this, the computer would store a significand and its exponential. However, as the computer has limited memory it cannot store the infinitely repeating pattern - it must cut it off when it runs out of space. This is how we end up with a floating point rounding error!

It is for this fact that computers cannot store infinite numbers and we end up with our first statement.

```
0.1 + 0.2 = 0.30000000000000004
```

### Why use them?

Floating-point numbers are great for when you either need fractions and don't mind the loss of accuracy or when you require the vast range in number they can store.

Floating point numbers are not suitable if you require pinpoint accuracy.

