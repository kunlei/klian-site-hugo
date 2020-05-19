---
title: C++ - tuple
date: 2020-05-18
draft: false
tags: ["c++"]
---

In my coding work today, I came across a need to store three values, of different types, within a single container so that I can save them in a set.
Normally I would define a dedicated class or structure, but a brief search online led me to an existing solution provided by C++ -- **tuple**.

#### What is tuple?

In a nutshell, tuple provides an convenient way to let programmers define a type that contains data elements of various types.
For example, a tuple can have an *int*, a *double* and a *std::string* as its elements.
The definition is straight forward

```cpp
#include <tuple>

std::tuple<int, double, std::string> t;
```

#### How to create tuple objects?

C++ provides us a function to create a tuple object, that is, `make_tuple`.

```cpp
t = std::make_tuple(1, 2, "tuple");
```

#### How to access elements within a tuple object?

To access individual elements within a tuple, we need the help of another function - `get`.

```cpp
cout << "first element: " << get<0>(t) << endl
     << "second element: " << get<1>(t) << endl
     << "third element: " << get<2>(t) << endl;
```
