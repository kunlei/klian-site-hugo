---
title: C++ - Expressions
date: 2020-01-11
draft: false
tags: ["c++"]
---

Expressions are the building blocks in C++ programs.
They consist of one or more operands connected by one or more operators.

## Precedence and Associativity

When multiple operators show up in the same expression, one need to heed the precedence rules defined by the language.
Fortunately, we can always use parentheses to manually override these rules, which saves us the efforts of memorizing them.
One caveat is that, within the same precedence, order of evaluation is undefined.
Below is an example,

```cpp
func1() + func2() * func3()
```

The precedence rule defined by C++ makes sure that the results of `func2()` and `func3()` are multiplied before adding to the result of `func1()`, but it says nothing about which of the three functions is evaluated first, meaning one cannot depend the logic on the order of evaluation.

## Short-circuit Evaluation

This always happen during operand evaluations involving logical `&&` and `||` operators.
Specifically, the left operand of these two operators is always evaluated first and the evaluation of the right operand might be skipped based on the evaluation result of the left operand.
In the example below, evaluation of `exp2()` is skipped if `exp1()` is `false` in the first case or `true` in the second case.

```cpp
exp1() && exp2() // case 1
exp1() || exp2() // case 2
```

## Increment and Decrement

There is a subtle difference between the prefix (`++`) and postfix (`--`) operators: the former yields a lvalue and the latter returns a rvalue.
Note that a lvalue can appear on the left of an assignment, so the first case in the code below is legal.
On the other hand, postfix operator return a rvalue that can only be put on the right of an assignment, so the case 2 is not allowed by C++.

```cpp
  int i = 1, j = 3;
  ++i = j; // case 1: correct
  i++ = j; // case 2: error
```

Note: the prefix and postfix operators have a higher precedence than the deference operator, which means that the `*ptr++` in the code below is equivalent to `*(ptr++)`.

```cpp
int num = 1, *ptr = &num;
*ptr++ = 2;
```
