---
title: Spring - Dependency Injection
date: 2020-07-30
draft: false
tags: ["java", "spring"]
---

Dependency injection is the underpinning concept of the Spring framework that is widely used to build enterprise applications using Java.
To understand the concept, it's important to grasp what dependency is.
Semantically speaking, dependency involves two things and describes the relationship between them.
In the context of Java programming, it refers to the relationship between two classes - a class is the dependency of another class if it provides indispensable functions for it.

Let's examine the concept from an example.
Imagine we have a `Room` class that is able to perform cleaning, either manually or automatically.
The cleaning activity is performed using any class implementing the `CleanStrategy` interface which provides a function, not surprisingly, called `clean()`.
In this case, whenever a room object needs cleaning, it will invoke the cleaning agent to do the actual work, and we call the class `Room` depends on a class implementing the interface `CleanStrategy`.

```java
public class Room {

  private final CleanStrategy cleanStrategy;

  public Room() {
    this.cleanStrategy = new ManualClean();
  }

  public void clean() {
    this.cleanStrategy.clean();
  }
}
```

```java
public class ManualClean implements CleanStrategy {

  @Override
  public void clean() {
    System.out.println("clean manually!");
  }
}
```

Now we are ready to unravel the concept of **dependency injection**.
In the above example, the `Room` class instantiates a `ManualClean` object within its constructor directly.
In case we want a different clean strategy, like automatic cleaning, in the future, we'll have to get into the constructor and make the change manually.
We can the classes `Room` and `ManualClean` are tightly coupled, which is not a desired design.

Instead, we can make the two classes loosely coupled by modifying the constructor like in the code below:

```java
public class Room {

  private final CleanStrategy cleanStrategy;

  public Room(CleanStrategy cleanStrategy) {
    this.cleanStrategy = cleanStrategy;
  }

  public void clean() {
    this.cleanStrategy.clean();
  }
}
```

Now we can change cleaning agents without modifying the class `Room` itself.
This way of decoupling two classes is also called the **strategy pattern** using the terminology of design patterns.

After we make the change, the dependency injection comes into play when we try to create a `Room` object by supplying a `ManualClean` object - we inject the `ManualClean` object into the `Room` object.

```java
public class Demo {
  public static void main(String[] args) {
    CleanStrategy cleanStrategy = new ManualClean();
    Room room = new Room(cleanStrategy);
    room.clean();
  }
}
```

This code change may seem trivial at first glance, but we'll learn its significance in future posts about the Spring framework.
