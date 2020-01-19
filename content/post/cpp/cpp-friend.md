---
title: C++ - friend
date: 2020-01-20
draft: false
tags: ["c++"]
---

Classes in C++ use access control specifiers like `public`, `protected` and `private` to decide which parts of their data attributes and/or member functions can be accessed by other objects.
The `public` keyword is the most generous one among them, by making all of the data/methods under this specifier freely available to other objects; `protected` and `private` are more prudent -- only certain types of objects can access their corresponding data/methods.
This post looks at one way of gaining access to the private data members of a class -- trying to be `friend`ly to a class.

## Friend Function

Let's say we have a class `Car` with a private data attribute:

```cpp
class Car {
public:
  Car() = default;

private:
  int size;
};
```

An ordinary function cannot directly access the variable `size` within a car object.
Sometimes it's desirable to grant direct access to a function, for example, we might want to override the `<<` operator to output our class to a stream.
To this purpose, we might declare the function as friend of the class:

```cpp
// Car.hpp
#include <iostream>
class Car {
  friend std::ostream& operator<<(std::ostream&, const Car&);

public:
  Car() = default;

private:
  int size;
};
std::ostream& operator<<(std::ostream&, const Car&);
```

The definition of `<<` may look like this:

```cpp
// Car.cpp
std::ostream& operator<<(std::ostream &strm, const Car &car) {
  return strm << "Car{size: " << car.size << "}";
}
```

The point is that we can retrieve the value of `size` from the `car` object directly from the function body of `operator<<`!