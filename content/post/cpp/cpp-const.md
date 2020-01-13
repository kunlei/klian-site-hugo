---
title: C++ - const
date: 2020-01-13
draft: false
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

Since reference is not an object, there is no `const` reference; however, a reference can refer to a `const` object.

```cpp
const int num = 10;
const int &rnum = num;
```

Actually it doesn't matter whether the variable `num` is const or not, meaning the code below is also common:

```cpp
int i = 20;
const int &ri = i;
```

This is an important property of reference to const -- we can use a const reference to refer to both the const and non`const` versions of the same variable.
`Declaring a reference to const only limits what we can do the bound object through the reference`.
In other words, the bound object can change through other means, it's just that the const reference won't be able to do so, as shown in the code below.

```cpp
i = 30; // okay, i is just a plain int variable, not const
ri = 10; // error, we cannot use ri to change i
```

## const pointer

The interaction between const and pointer is more interesting, mainly due to the fact that a pointer itself is an object.
Similar to references, we can use a pointer to const to refer to both the const and nonconst versions of a variable:

```cpp
int a = 10;
const int *pa = &a;
```

Here, we cannot use the pointer `pa` to modify the value of variable `a` even though it is not const itself.

On the other hand, we can only use a pointer to const to point to a const variable:

```cpp
const int b = 20;
const int *pb = &b; // okay
int *pbb = &b; // wrong
```

This is easy to understand as we cannot use a plain pointer to change a const variable.

Unlike references, a pointer itself can be const, which means that it must be initialized upon creation and cannot be updated after that.

```cpp
int c = 10, cc = 30;
int * const pc = &c;
*pc = 20; // okay
pc = &cc; // error
```

Declaring a const pointer only means that the pointer itself cannot be modified, but we might still modify the object that the pointer points to.
The line 3 in the code above is perfectly legal, while line 4 is not.

Now look at the code snippet below, pointer `pd` is a pointer to const, meaning that we cannot use `pd` to modify the value of variable `d`; however, `pd` can point to another variable, as long as the new variable is still marked with `const`. 
After all `pd` is just a pointer that saves the address of an const integer variable -- it doesn't care which variable it points to.
On the other hand, the pointer `pdd` is declared as `const` itself and the variable it points to is also declared as `const`, which restrict that the pointer can only point to `d` and cannot modify the value of `d`.

```cpp
const int d = 10;
const int * pd = &d;
const int * const pdd = &d;
```

### Top-level const vs. low-level const

The const declared on most types, including primitive types, classes and pointers, is called top-level const, which prevents the object itself from being modified.

```cpp
const int a = 10; // top-level const
const Car car("red"); // top-level const
int * const ptr = nullptr; // top-level const
```

Low-level const shows up in the base type of compound types:

```cpp
const int * pa = nullptr; // low-level const
int * const pb = nullptr; // top-level const
const int * const pc = nullptr; // both low- and top- level const
```
