---
layout: post
title: Possibility Calculator
subtitle: Learn how to calculate how many possibilites there are
tags: [go]
comments: true
---

In this article I'm going to create a "possibilities calculator", and then I'll create a simple cracker to demonstrate how longer passwords can help you stay secure.

## What is a possibilites calculator

If you have a password policy that allows `[a-zA-z0-9!@£$%^&*]`, that gives us a characer set of 70:

* 26 lowercase letters (`a-z`)
* 26 uppercase letters (`A-Z`)
* 10 numbers (`0-9`)
* 8 special characters (`!@£$%^&*`)

A possibilities calculator will calculate how many possibilities a password can have, given the length of the password.

## Non-code solution

As we're discussing passwords, we're going to allow every combination; duplication (e.g. `aa`, `AA`, `00`) is allowed and `AB` and `BA` are considered different combinations.

To calculate the number of possibilites, we times the length of the password by the character set (70).

For a password length of 10, the calculation is 70<sup>10</sup> (or 70 x 70 x 70 x 70 x 70 x 70 x 70 x 70 x 70 x 70).

That gives us 2,824,752,500,000,000,000 possible combinations; that's nearly three Quintillion!

| Name | Number of Zeros | Groups of Three Zeros |
| --- | -- | -- |
| Ten | 1 | 0 |
| Hundred | 2 | 0 |
| Thousand| 3 | 1 (1,000) |
| Ten thousand | 4 | 1 (10,000) |
| Hundred thousand | 5 | 1 (100,000) |
| Million | 6 | 2 (1,000,000) |
| Billion | 9 | 3 (1,000,000,000) |
| Trillion | 12 | 4 (1,000,000,000,000) |
| Quadrillion | 15 | 5 |
| Quintillion | 18 | 6 |
| Sextillion  | 21 | 7 |
| Septillion  | 24 | 8 |
| Octillion   | 27 | 9 |
| Nonillion   | 30 | 10 |
| Decillion   | 33 | 11 |
| Undecillion | 36 | 12 |
| Duodecillion | 39 | 13 |
| Tredecillion | 42 | 14 |
| Quattuordecillion | 45 | 15 |
| Quindecillion | 48 | 16 |
| Sexdecillion | 51 | 17 |
| Septen-decillion | 54 | 18 |
| Octodecillion | 57 | 19 |
| Novemdecillion | 60 | 20 |
| Vigintillion | 63 | 21 |
| Centillion | 303 | 101 |

## Code Solution


