---
title: C++ - Array
date: 2020-01-21
draft: false
tags: ["c++"]
---

Arrays are used less frequently than the `vector` library type, but may provide a option to define a list of objects when the size does not change.
I always find the definition of arrays in C++ counterintuitive, partly due to the fact the a pair of brackets must be put after the name of the variable.
I actually use dynamically allocated arrays more often in my daily programming experiences.

## Initialization

The code below shows how to define fixed-sized arrays.

```cpp
int a[10]; // array of ten integers
int *b[10]; // array of ten pointers to integers
Car c[10]; // array of ten Car objects
```

It might help if we realize that it's a compound type and special declaration rules must be used, like when we define references or pointers with the help of `&` and `*`, respectively.

Arrays are default initialized if we don't provide explicit initializers, like the code below:

```cpp
int a[2] = {1, 2}; // a = {1, 2}
int b[3] = {1}; // b = {1, 0}
int c[] = {1}; // c = {1}
```

One caveat is the initialization of a character array using string literals, which end with a null character:

```cpp
char d[] = "hello"; // the size of d is 6!!!
```

## Array Pointer and Reference

We can always try to make our lives harder by defining a pointer or reference to an array.

```cpp
int a[10] = {1, 2};
int *b[2];
int (*pa)[10] = &a; // pa is a pointer to array a
int *(*pb)[2] = &b; // pb is a pointer to b
int (&ra)[10] = a; // ra is a reference to a
int *(&rb)[2] = b; // rb is a reference to b
```

## Array Name Demystified

The name of an array has a special property -- it's actually a pointer to the first element in the array.

```cpp
int a[10] = {1, 2};
cout << a << endl; // output: 0x7ffee7ab77c0
cout << &a[0] << endl; // output: 0x7ffee7ab77c0
```

## Visiting Array Elements

We can use two possible ways to access individual elements in an array:

```cpp
int a[10] = {1, 2};
// option 1
for (auto i : a) {
  cout << i << endl;
}

// option 2
for (size_t i = 0; i < 10; ++i) {
  cout << a[i] << endl;
}

// option 3
int *ab = begin(a); // equal to a or &a[0]
int *ae = end(a); // equal to &a[10]
for (int *ptr = ab; ptr != ae; ++ptr) {
  cout << *ptr << endl;
}
```

The option 3 needs a bit explanation, the variables `ab` and `ae` are essentially pointers to the first and one-past-the-last element in the array.

## Multidimensional Arrays

Multidimensional arrays are defined in similar ways, with more brackets after the array name.

```cpp
int aa[1][2] = {
  {2, 2},
  {3, 3}};
```

Iterating elements in an array can be done using a foreach loop.

```cpp
for (auto &r : aa) {
  for (auto &c : r) {
    cout << c << endl;
  }
}
```
