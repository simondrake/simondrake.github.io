---
layout: post
title: Differences between `int` and `uint`
subtitle: And what the numbers after them mean
tags: [beginner]
comments: true
---

What is the difference between `int`, `uint`, `int8`, `int32` and `int64`?

The number after the `int` type determines how many bits are used to hold the number, which determines the maximum size number it can hold.

* `int8` has 8 bits of storage space and can hold 256 values (2^8)
* `32-bit` has 32 bits of storage space and can hold X values (2^32)

There are signed integers (`intX`) and unsigned integers (`uintX`). Unsigned will hold that range as all positive values and zero, while signed values will split the range between positive and negative numbers.

For the 8-bit types, unsigned range is 0-255, while the signed int will range between -128 and + 127 (256 values, when you include zero).

