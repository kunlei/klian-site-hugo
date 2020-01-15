---
title: C++ - vector
date: 2020-01-20
draft: false
tags: ["c++"]
---

The C++ `vector` defines a library template to contain a list of objects of the same type.
The appropriate header and namespace must be included to use it:

```cpp
#include <vector>
using std::vector;
```

It's worth noting that `vector` is not a class, instead it defines a template from which a variety of classes can be instantiated.
To this purpose, an additional parameter, specifically, the type of objects in the vector, must be provided.
For example, the code snippet below instantiates two separate classes, one for holding a list of integers, and the other for holding a list of objects of use-defined type `Car`.

```cpp
vector<int> a;
vector<Car> b;
```

### Vector Initialization

There are actually a number of ways to initialize a vector object, as in the example below.

```cpp
vector<int> a; // an empty vector
vector<int> b(a); // b is a copy of a
vector<int> c = a;
vector<int> d(10, 1); // d is a vector of 10 elements, which is all 1
vector<int> e{1, 2, 3}; // define a vector of 3 elements, 1, 2, 3
vector<int> f = {1, 2, 3}; // a vector of 3 elements specified in the braces
```

### Modify a Vector

One key characteristic of a `vector` is that we can dynamically grow it at runtime, in other words, a vector object is not restricted by its initial size and more objects of the defined type can be added to the vector, which is often the purpose of using the vector in the code.
The code below shows the key function for this purpose:

```cpp
vector<int> a;
for (auto i = 0; i < 10; ++i) {
  a.push_back(i);
}
```

Here, we create an empty vector object and subsequently add ten more integers to it, and all we need to do is to call the `push_back()` function.

### Accessing or Traversing a Vector

There are multiple ways to iterate through a vector, as shown in the code below:

```cpp
vector<int> a{1, 2, 3, 4, 5};

// option 1
for (decltype(a.size())i = 0; i < a.size(); ++i) {
  cout << a[i] << endl; // or a.at(i)
}

// option 2
for (auto i : a) {
  cout << i << endl;
}

// option 3
for (auto itr = a.begin(); itr != a.end(); ++itr) {
  cout << *itr << endl;
}
```
