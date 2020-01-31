---
title: C++ - Standard Library Sequential Containers
date: 2020-01-28
draft: false
tags: ["c++"]
---

C++ provides a set of containers to hold elements of the same type.
These containers can be generally put into two categories:

+ Sequential containers
+ Associative containers

This post examines the various sequential containers, which include

+ vector
+ deque
+ list
+ forward_list
+ array
+ string

## Container Iterators

Traversing elements in a container can be done with the help of iterators, which exist in various forms:

+ iterator -- supports reading and writing elements, obtained using `begin()` and `end()`.
+ const_iterator -- only supports reading, obtained using `cbegin()` and `cend()`.
+ reverse_iterator -- visits elements in reverse order and supports both read and write, obtained using `rbegin()` and `rend()`.
+ const_reverse_iterator -- visits elements in reverse order and only support read, obtained using `crbegin()` and `crend()`.

Below shows examples of using iterators to traverse a vector.

```cpp
vector<int> vec = {1, 2, 3};
// using iterator
for (vector<int>::iterator itr = vec.begin(); itr != vec.end(); ++itr) {
  cout << *itr << endl;
}

// using const_iterator
for (vector<int>::const_iterator itr = vec.cbegin(); itr != vec.cend(); ++itr) {
  cout << *itr << endl;
}

// using reverse_iterator
for (vector<>::reverse_iterator itr = vec.rbegin(); itr != vec.rend(); ++itr) {
  cout << *itr << endl;
}
```

## Container Constructors

Sequential containers generally support various forms of constructors, list constructor being one of them:

```cpp
deque<int> deq = {1, 2, 3};
```

or they can also take an initial size parameter as well as an element initializer:

```cpp
list<int> lt(10, 1); // a list of 10 elements with value 1
```

## New Container Type -- array

The new standard introduces another container type, `array`, that stores fixed number of elements and supports fast random access:

```cpp
array<int, 10> arr = {1, 2};
```

## Querying Container Size

There are two most useful functions to inspect the size of a container: `size()` and `empty()`.

## Adding Elements to Containers

The standard library defines several functions to add new elements to containers:

+ `push_back(e)`
+ `push_front(e)`
+ `insert(ptr, e)`

### `push_back()`

This operation is supported by `vector`, `deque`, `list` and `string`.

### `push_front()`

This operation is supported by `deque`, `list` and `forward_list`.

### `insert()`

The first parameter of this function is an iterator pointing to the position before which we want to insert the new element(s).
The other parameters denote the new element(s) we want to insert.

### `emplace()`

The operation of inserting an element actually copies the object into the container, the `emplace()` function uses its arguments to construct an element in the container.

## Removing Elements from Containers

Similar to adding new elements, there are functions defined to remove elements from containers:

+ `pop_back()`
+ `pop_front()`
+ `erase()`

