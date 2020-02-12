---
title: C++ - Copy Control (3)
subtitle: Create objects that share states
date: 2020-02-11
draft: false
tags: ["c++"]
---

In an earlier post, we discussed ways to define copy control operators to make objects of the same class behave like values, in other words, copies of an object are independent from each other and don't share resources.
However, sometimes we do want multiple objects to share the same data and the objects themselves behave more like pointers.
In this post, we will examine how to achieve this, using an illustrative class `Car` which has a price variable and weight variable to represent, not surprisingly, its price and weight, respectively. 

```cpp
class Car {
public:
  // constructor
  Car(int p, int w);
  // copy constructor
  Car(const Car &c);
  // copy-assignment operator
  Car& operator=(const Car &c);
  // destructor
  ~Car();

private:
  int price; // price of the car
  int *pWeight; // weight of the car
  int *pCnt; // count
};
```

## Counter

The fact that multiple objects may share the same resource necessitates a way to check whether it's safe to free the resource or not.
Clearly, an object cannot free the resource if another object is still use it.
On the other hand, if an object sees that it's the last one with access to the object, it's obliged to free it during the object's destruction process.
To this purpose, we need to keep track of the number of objects sharing the same resources, and the counter variable comes to our rescue.
Note that the counter variable is a pointer too so that multiple objects can communicate about the number of other objects sharing the same data.

## Constructor and Copy Constructor

The following code illustrates the definition of the constructor, which initializes the counter variable to 1 upon object creation.

```cpp
// constructor
Car::Car(int p, int w) : price(p),
  pWeight(new int(w)),
  pCnt(new int(1)) {
}
```

The copy constructor has slightly more works to do.
In addition to copy values of price and weight, it has to manage the counter variable.
The counter pointer is first copied into the object on the left-hand side, then its value is increased by 1 to indicate that now there is one more object sharing the resource.

```cpp
Car::Car(const Car &c) : price(c.price),
  pWeight(c.pWeight),
  pCnt(c.pCnt) {
  ++*pCnt;
}
```

## Copy-assignment operator

The copy-assignment operator definition is more involved since the case of self-assignment must be taken care of.
The problem is when should we free the resource used by the left-hand side object.
The shared resource indicated by the left-hand object no longer exists should we free it first; then in the case of self-assignment, the right-hand object is still using the already-freed resource, which is precarious.
To circumvent this, we should increment the counter through the right-hand object first before checking whether to free the resource in the left-hand object.

```cpp
Car& Car::operator=(const Car &c) {
  ++*c.pCnt;
  --*pCnt;
  if (*pCnt == 0) {
    delete pWeight;
    delete pCnt;
  }
  price = c.price;
  pWeight = c.pWeight;
  pCnt = c.pCnt;
  return *this;
}
```

## Destructor

The destructor is supposed to free the resource when no other object is still using it.
This checking can be dong with the help of the counter variable.

```cpp
--*pCnt;
if (*pCnt == 0) {
  delete pWeight;
  delete pCnt;
}
```
