---
layout: default
title: "Why chimera implements Rational"
date: 2018-03-17 16:20:00 -0700
categories: chimera python learning design
---

I have been implementing a Python3 interpreter from using the cPython spec [chimera](https://github.com/grandquista/chimera). It differs from the spec in a few places, and so this is the first post discussing how it differs and a little of why.

The entire implementation is pre alpha at the moment so all interfaces shown are subject to change.

In Python integers can grow to fill memory. There are a few documented places that do require the use of an integer bound to a c `long` or `int`. These are where underlying system interfaces are used, or taking the `len` of builtin iterators. Where math operations are involved there are few limitations.

Floats are a different type in the number hierarchy implemented using a c `float` or `double`. They are always bound to the underlying type max, min, and accuracy. This bounding of the implementation causes exceptions casting from `int` to python `float`. Since Python3 there is an implicit cast used in integer division, and a floor division operator implements the old python2 behavior.

Some Python code demonstrating the exceptions encountered and possible bugs that could be hidden.

First this statement will report the approximate limiting factor for `int` to `float`. All numbers used in this statement are parsed as integers and this is important to find the limit without triggering an exception.
```py
next(i for i in range(1, 1 << 300) if (1 / (1 << i)) == 0)
```

On my machine this reports back `1075`.

Using just the expression from the `if` expression above a bounding error is encountered but no exception is reported. The division is implicitly cast to a `float` and bounded to `0.0`.
```py
1 / (1 << 1075)  # 0.0
```

If a `float` is encountered before the division takes place, then an exception is raised based on the `int` being to large.
```py
1.0 / (1 << 1075)  # OverflowError
```

A `float` doesn't support bit operations even if it represents an exact integer.
```py
1.0 << 1075  # TypeError
```

This leads to at least three code paths to handle before or after a division expression. The first is `0` representing an edge case in many algorithms. It is therefore now an exception to do any division after the first division succeeds. The `OverflowError` in the second case can be handled in a `try` statement. The `TypeError` is more generic of an exception and might be better handled with a look before you leap check.

There is a solution to most of this in the standard library. The `rational` module implements a `Rational` class that will handle the first two cases.

Based on the existence of that module I have implemented `struct Rational` in chimera's number implementation to handle Python `float`. The number types in [https://github.com/grandquista/chimera/tree/master/library/object/number](https://github.com/grandquista/chimera/tree/master/library/object/number) use `std::variant` to build a hierarchy around `std::uint64_t`. A rational is then `numerator` and `denominator` fields as a `variant` of all integer types. This implementation of `float` is then just as unbounded as the `int`s it could be cast from.

Having a unified number interface underlying both types will also allow a memory pool trivially join both `float` and `int` values. This will be explored in future posts.
