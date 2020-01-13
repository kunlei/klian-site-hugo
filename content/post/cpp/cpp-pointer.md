---
title: C++ - Pointer
date: 2020-01-12
draft: false
tags: ["c++"]
---

Pointer in C++ is similar to reference in the sense that they both refer to another objects other than themselves, although the ways they achieve it are different:

- A reference bounds to another object and essentially exists as an alias.
- A pointer saves the address of the object it points to.

A key difference between them is that `a pointer itself is an object` while `a reference is not`.

A pointer is declared with the help of `*`:

```cpp
int *pa;
double *pb;
Car *pc;
```

It's worth noting that `pointers defined within function scopes are not initialized`.

As noted above, a pointer variable saves the address of another variable, which is obtained using the operator `&` shown in the code example below.

```cpp
int a = 10;
int *pa = &a;
int **ppa = &pa;
cout << "a = " << a << "\t&a = " << &a << endl;
cout << "pa = " << pa << "\t*pa = " << *pa << "\t&pa = " << &pa << endl;
cout << "ppa = " << ppa << "\t*ppa = " << *ppa << "\t&ppa = " << &ppa << endl;
```

The code output is as follows,

```console
a = 10	&a = 0x7ffeefbff52c
pa = 0x7ffeefbff52c	*pa = 10	&pa = 0x7ffeefbff520
ppa = 0x7ffeefbff520	*ppa = 0x7ffeefbff52c	&ppa = 0x7ffeefbff518
```

The fact that a pointer is also an object implies that it also has an address.
In the code above, the address of the integer variable `a` is saved into the pointer variable `pa` whose address is saved into another pointer variable `ppa`.

Another major difference between references and pointers is that the value a pointer variable can be changed while a reference can only bind to one and only one object, which is illustrated below,

```cpp
int a = 1, b = 2;
int &refa = a; // bind refa to a
refa = b; // assign b to a
int *pa = &a; // pa points to a
pa = &b; // pa now points to b
```

We can also define a reference to a pointer variable:

```cpp
int a = 10, *pa = &a;
int *&ra = pa;
```
