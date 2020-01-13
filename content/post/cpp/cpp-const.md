---
title: C++ - const
date: 2020-01-13
draft: true
tags: ["c++"]
---

Normally operations performed on a variable can be put into two categories:

1. operations that retrieve the value of the variable.
2. operations that modify the value of the variable.

We can prohibit a variable being changed by type 2 operations using the qualifier `const`, as in the code below.

```cpp
const int var = 1;
```

Here, the base type of variable `var` is still `int`, the `const` keyword just deprives the variable of certain operations that are allowed by its non`const` version.
As the intention of `const` is to prevent its value from unintended changes, a `const` variable must be initialized upon creation.

## const variable used across files

Variables defined as `const` are treated as separate variables even if they have the same name, like the variable `var1` in the code below.
This is because the compiler will substitute all the `const` variable with its initialized value and the initializer must be visible to the compiler for this purpose.
In other words, if a `const` variable is just declared, the compiler has no way to retrieve its value to do the substitution.

To use the same `const` variable across multiple files, the correct way is to add the `extern` keyword in both the file where it's defined and all the other files where it's used, as illustrated by the variable `var2` below.

```cpp
// file1.cpp
const int var1 = 10;
extern const int var2 = 20;
```

```cpp
// file2.cpp
const int var1 = 10;
extern const int var2;
```

## const reference

## const pointer
