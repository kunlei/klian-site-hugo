---
title: C++ - auto
date: 2020-01-18
draft: false
tags: ["c++"]
---

The C++ new standard introduces the keyword `auto` to help us deduce the type of a variable that's assigned to the value of an expression.

```cpp
int a = 1, b = 2;
int c = a + b; // old way
auto d = a + b; // new way
```

One thing to keep in mind is that the top-level const is normally ignored during the type deduction:

```cpp
const int a = 3;
auto aa = a; // aa is an int, not const int
```

However, the top-level const is not ignored when we define a reference:

```cpp
const int a = 3;
auto &aa = a; // aa is a const int
```
