---
title: C++ - const member functions
date: 2020-01-19
draft: false
tags: ["c++"]
---

I used to find the usage of `const` after the parameter list of a member function within a class confusing -- the code snippet below shows an example in which `getSize()` is followed by a const specifier.
It turns out that making sense of the `const` usage here requires a deep understanding of another keyword -- `this`.

## What is `this`? (pun intended)

The specifier `this` is a reserved keyword in C++ that is commonly used in the definitions of class member functions to refer to the current object on which the functions are operating on.
It is a special pointer that saves the address of a class instance.
The code below creates a plain pointer `ptr` to refer to a `Car` object.
It's expected that `ptr` and `this` should have the same value, as can be seen in the output following the code.

```cpp
#include <iostream>
class Car {
public:
  Car() = default;
  int getSize() const {
    std::cout << "this: " << this << std::endl;
    return size;
  }
  void setSize(int size) {
    this->size = size;
  }
private:
    int size = 0;
};

int main() {
  Car car;
  Car *ptr = &car;
  std::cout << "ptr: " << ptr << std::endl;
  ptr->getSize();

  return 0;
}
```

```cpp
// code ouput
ptr: 0x7ffee0fc97f8
this: 0x7ffee0fc97f8
```

But `this` is no ordinary pointer -- it's special in the sense that it's a `const` pointer which cannot be bound to another object.
If we were able to define `this` by ourselves, it will look something like this:

```cpp
Car nissan, toyota;
Car * const this = &nissan; // only for illustration, won't compile
this = &toyota; // error, this has top-level const
```

Note that the `const`ness of `this` is top-level, so the line 3 in the code above is illegal; on the other hand, `this` doesn't have low-level const, meaning the object it points to is free to change.

## How to prevent `this` from modifying the underlying object?

One natural question is how to impose low-level const on `this`?
Here we finally come to the point to explain the usage of `const` of a member function parameter list.
Because `this` is implicitly define for us, we don't have opportunity to explicitly state that `this` refers to a const object, instead, we have to use a somewhat convoluted way to impose low-level const on `this`. 
The `getSize()` function has a trailing `const`, implying that the method is not allowed to modify the underlying object.

If we were to add the `const` specifier after the `setSize()` function, as in the code below, there will be a compiling error since the usage of `const` prevent any changes to the object `this` points to and therefore the attempt to assign a value to `size` through `this` is illegal.

```cpp
void setSize(int size) const {
  this->size = size; // compiling error
}
```

## What is really `this`?

My understanding of C++ memory model is still superficial at this moment, but the exploration of the `const` usage helps shed light on its internal working (what follows from this point is totally my personal view and in no way I can guarantee it's the correct interpretation, I'll probably come back and revise it in the future :-)).

When we define an object, C++ allocates certain physical memories to it and provides us a convenient pointer `this` to refer to them.
I believe it's inefficient to have a copy of all the member functions to each object -- after all, it's the member attributes that are different from object to object, and C++ is likely to keep the member functions on the class level, like static variables.
Then, what actually happens under the hood when we call a member function of an instantiated object? for example,

```cpp
int main() {
  Car car;
  std::cout << car.getSize() << std::endl;
  return 0;
}
```

I suspect the `.` pointer does all the magic in the process.
The function all is likely to be translated into something like this:

```cpp
Car::getSize(&car);
```

or using the implicit `this` keyword, 

```cpp
Car::getSize(this);
```

I will come back to verify this after I get deeper understanding of C++ in the future.
