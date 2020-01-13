---
title: C++ - Reference
date: 2020-01-12
draft: false
tags: ["c++"]
---

Reference is one of the compound types in C++ that modifies the base type of a variable.
It is essentially another name of an existing variable, meaning that any operation on a reference is performed on the variable that the reference is bound to.
It is important to note that a reference `is not an object`.

A reference is defined using the `&` operator, as in the code below,

```cpp
int num = 3;
int &ref = num;
```

A reference must be bound to a variable upon creation, for example, the code below will trigger compilation error.

```cpp
int &ref; // error
```

One thing to heed is that a reference cannot be bound to another variable after creation.
Line 3 appears to bind the reference `ref` to a new variable `b`, but is actually an assignment operation -- assign the value in `b` to variable `a` to which the reference is bound to.

```cpp
int a = 1, b = 2;
int &ref = a;
ref = b; // assignment to a
```
