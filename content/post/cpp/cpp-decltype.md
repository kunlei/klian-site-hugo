---
title: C++ - decltype
date: 2020-01-18
draft: false
tags: ["c++"]
---

The keyword `decltype` is similar to `auto` in that it also helps us to deduce variable type, but there are some subtle differences.

For one, `decltype` does not ignore top-level const.

```cpp
const int a = 10;
decltype(a) aa = 20; // aa is defined as const int, not int
```

For two, `decltype` sometimes returns a reference type.

```cpp
int a = 1, *pa = &a, &ra = a;
decltype(*pa) = a; // int&
decltype(ra) = a; // int&
```
