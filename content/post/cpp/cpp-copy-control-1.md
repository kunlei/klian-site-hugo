---
title: C++ - Copy Control (1)
subtitle: The gang of three
date: 2020-02-09
draft: false
tags: ["c++"]
---

Copy control in C++ is too big a topic and I'll try to share my learnings in multiple posts.
Part of the reasons why C++ is so powerful lies in the granular control it grants programmers to decide how objects of a class are created (constructor), copied (copy constructor), assigned (copy-assignment operator) and destroyed (destructor).
The new standard introduces the move operators: move constructor and move-assignment operator.
Therefore, there are five operators to remember when designing a C++ class:

+ copy constructor
+ move constructor
+ copy-assignment operator
+ move-assignment operator
+ destructor

Note that the copy/move operators are used in an object initialization process, whereas the copy/move-assignment operators apply to the assignment process of one object to another.
The difference is subtle, initialization assumes that the object being initialized doesn't exist yet and we use another object of the same class to construct the object.
On the other hand, assignment happens when the object of the left hand already exists and the object of the right hand is copied to the object of the left hand. 

For illustration purpose, we'll try to define the copy control operators for an artificial `Car` class.

```cpp
class Car {
public:
  Car(); // constructor
  Car(int s, std::string n); // constructor
  Car(const Car&); // copy constructor
  Car& operator=(const Car&); // copy-assignment operator
  ~Car();

private:
  int size;
  std::string name;
};
```

## Copy Constructor

The copy constructor, like other constructors we might define for a class, has no return type.
The distinctive feature is the parameter type, it always takes a reference to a const object of the same type as the first parameter. 
It may optionally contain other parameters, but they must have default values.

```cpp
Car(const Car&);
```

Examining its signature, there are two things about a copy constructor that are worthing noting.
The first one is the `const` specifier, which is understandable as we only intend to copy the object passed as the parameter.
The other one is the `&` specifier, which is necessary here because without the reference specifier, the argument object passed to the copy constructor will have to be copied to the parameter first, but we have yet defined the copy constructor, which creates a paradox -- in order to define a copy constructor, we need a copy constructor first!
That's why the reference specifier `&` is necessary here to break the dilemma.

### Direct vs. copy initialization

It's easy to confuse the direct versus the copy form of initialization:

```cpp
Car car1(10, "car1");
Car car2 = car1;
```

In the code above, line 1 is an example of direct initialization, which essentially uses function matching to identify the best constructor to create a new object.
The second line represents the copy construction, in which the object on the right hand side is copied into the object on the left hand side.

One use case of copy initialization is in function definitions:

```cpp
Car makeCar(Car car) {
  return car;
}
```

The seemingly simple function above involves two copy initializations: the first one happens when the `car` parameter is initialization upon function call; and the other one happens when the function finishes executing and the `car` object is returned to the caller!

Another use case of copy initialization with with containers: 

```cpp
std::vector<Car> cars;
Car car(10, "car");
cars.push_back(car);
```

In this case, the car object that's put into the `cars` container is independent from the `car` created on line 2, this is because the container stores a copy of the object during the `push_back` function.

## Copy-assignment Operator

Examining the operator signature, it's easy to see that the copy-assignment operator overloads the `=` operator using the keyword `operator`.
Similar to the copy constructor, it takes a reference to a `const` object as its parameter.
One difference is that this function returns a reference.

```cpp
Car& operator=(const Car &other) {
  size = other.size;
  name = other.name;
  return *this;
}
```

## Destructor

The destructor works reversely to the constructor.
Note that a constructor actually involves two steps: (1) initialization and (2) function body, and the function body is executed after class members are initialized.
The destructor, on the other hand, runs its function body first before destroying class members. 
