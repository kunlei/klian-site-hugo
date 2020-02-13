---
title: C++ - Swap
date: 2020-02-12
draft: false
tags: ["c++"]
---

The C++ standard library defines a facility function that exchanges values of two variables.
It's defined in header `<utility>` and namespace `std`.
The code below shows an illustrative example.

```cpp
#include <utility>
#include <iostream>

int main() {
  int a = 10, b = 20;
  // before swap
  std::cout << "a = " << a << std::endl; // a = 10
  std::cout << "b = " << b << std::endl; // b = 20
  
  swap(a, b);

  // after swap
  std::cout << "a = " << a << std::endl; // a = 20
  std::cout << "b = " << b << std::endl; // b = 10

  return 0;
}
```

## Define Swap for Class

We might overload the swap function for our own classes, and the code below shows an example.

```cpp
class Car {
  friend void swap(Car&, Car&);

public:
  Car() : size(0), pWgt(new int(0)) {}
  ~Car() {
    delete pWgt;
  }

private:
  int size;
  int *pWgt;
};
inline void swap(Car &a, Car &b) {
  using std::swap;
  swap(a.size, b.size);
  swap(a.pWgt, b.pWgt);
}
```

Note that the `swap` function used in the definition should **not** be `std::swap`.
