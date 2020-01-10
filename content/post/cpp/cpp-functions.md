---
title: C++ - Functions
date: 2020-01-09
draft: true
tags: ["c++"]
---

A function in C++ has four components:

- Return type
- Name
- Zero or more parameters
- Body

A simple example function looks like below,

```cpp
int sum(int a, int b) {
  return a + b;
}
int i = 1, j = 2;
cout << sum(i, j) << endl;
```

There is a distinction between *parameter* and *argument* - variables enclosed in function definition parenthesis are parameters, whereas values supplied to the function when it's called are arguments.
Put it another way, a calling function provides arguments to initialize parameters of a called function.
In the above example, variables `a` and `b` are function parameters, while `c` and `d` are arguments.

Note that the function body defines its own scope and all the parameters are local variables within that scope.
Once a function is called, all the parameter variables are initialized using values from arguments and destroyed when the function finishes execution.
