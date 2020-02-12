---
title: C++ - Copy Control (2)
subtitle: Create objects that act independently
date: 2020-02-10
draft: false
tags: ["c++"]
---

In this post, we will put our copy control skill in use by designing an artificial `Car` class that manages its own resources.
A `Car` object has two member attributes, price and weight, both are integer variables.
The synthesized copy control operators would be enough if both of them are represented in primitive types.
For the sake of illustration and in order to make this code more interesting, we use a pointer to a integer variable to save a car's weight.
Also all the functions are defined within the class header file to simplify the demonstration.
The skeleton of the class looks as follows:

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
};
```

## Constructor and Copy Constructor

We define a normal constructor for this class that takes two input parameters, price and weight, correspondingly.
Note that the weight pointer is initialized in the initialization list by allocating a space using the `new` operator.

The copy constructor is straightforward too -- we simple copy the value of `price` from the input object and create a new pointer with the value of `weight`.
Note that the newly created object is thereafter independent from the input object `c`, and changes made to either of them have no effect on the other object.

```cpp
// constructor
Car::Car(int p, int w) : price(p), pWeight(new int(w)) {}

// copy constructor
Car::Car(const Car &c) {
  price = c.price;
  pWeight = new int(*c.pWeight);
}
```

## Copy-assignment Operator

One has to pay heed to the definition of copy-assignment operator, this is because the space used by the object on the left-hand side of the `=` operator must be properly freed before we copy the object from the right-hand side.
One caveat is that the two objects might be the same one, in other sides, one might assign one object to itself.
The code below provides one way to address this.
It starts with allocating memory to hold the weight value before releasing the memory pointed by the weight pointer.
The pointer to this newly created memory is then assigned to the object on the left-hand side of the operator.

```cpp
// copy-assignment operator
Car& Car::operator=(const Car &c) {
  int *pw = new int(*c.pWeight);
  delete pWeight;
  price = c.price;
  pWeight = pw;
}
```

## Destructor

The destructor simply needs to free the space used by the weight variable.

```cpp
// destructor
Car::~Car() {
  delete pWeight;
}
```

## Summary

This post shows an example class whose objects are independent from each other after instantiation.
However, sometimes it's desirable that objects of the same class same the same states, which we will explore in future posts.
