---
title: C++ - Variable Initialization
draft: false
date: 2020-01-11
tags: ["c++"]
---

Programming in C++ boils down to creating and manipulating variables of either built-in or class types.
It's therefore imperative to understand how variables are initialized upon creation.

## Primitive Types

There are a number of ways to give value to newly created variables,

```cpp
int a(1); // option 1
int a = 1; // option 2
int a{3}; // option 3
int a = {4}; // option 4
```

Options 3 and 4 are called `list initialization`.

What happens if we don't explicitly specify a value when we create a variable?
It depends:

+ If the variable is outside any function body, it's automatically initialized to 0.
+ If the variable is within a function body, its value is undefined.

I almost always use the second form of initialization, which resembles an assignment but is actually not.

## Class Types

The way we initialize variables of class types should follow their corresponding constructor definitions, which also decides whether they can be default initialized.
For example, a `std::string` variable can be defined as below,

```cpp
std::string a; // default initialized to empty string
std::string b("empty");
```

On the other hand, a class may ask for explicit initialization.

```cpp
Car car; // error
Car car("red"); // must provide a color upon definition
```
