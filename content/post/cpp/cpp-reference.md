---
title: C++ - Reference
date: 2019-12-31
draft: true
tags: ["c++"]
---

I use references frequently in my C++ codes and don't usually think about the syntax requirements associated with it.
It turns out that there are some intricacies in its usage, below is a brief summary in order to internalize some basic rules.

## Define a Reference

A reference is defined using the `&` operator and is an alias (or alternate name) to another object (not literals).
For example, the first line in the code snippet below is correct but the second is not.

```cpp
int num = 3, &rnum = num; //right
int &ri = 5; // wrong
```

Note that a reference is not an object.

## Initialization

One requirement of using reference is that it must be initialized upon declaration, that is, the code below won't compile:

```cpp
int &ri;
```

Once a reference is initialized, it cannot be bound to another variable.

## Usage

### Function parameter
