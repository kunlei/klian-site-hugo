---
title: Functional Interface - Predicate
date: 2020-08-10
draft: false
tags: ["java"]
---

Functional interface in Java is a special type of interface that has only one abstract method.
A typical example is `Predicate<T>` which provides a `test(T t)` method to return a boolean variable:

```java
@FunctionalInterface
public interface Predicate<T> {
  boolean test(T t);
}
```

Note that `@FunctionalInterface` is not required here but can help the JDK recognize our intention.
Classes implementing this interface provide an evaluation function to decide whether a statement about a `T` object is true or not.
This is often used in filtering functions where we establish a filter criterion to decide whether an object meet the requirement or not.

To illustrate this, let's define a `Car` class with three attributes, namely, color, weight and year:

```java
package com.learn.java.java8demo;

public class Car {
  private String color;
  private double weight;
  private int year;

  public Car(String color, double weight, int year) {
    this.color = color;
    this.weight = weight;
    this.year = year;
  }

  public String getColor() {
    return color;
  }

  public double getWeight() {
    return weight;
  }

  public int getYear() {
    return year;
  }

  @Override
  public String toString() {
    return "Car{" +
        "color='" + color + '\'' +
        ", weight=" + weight +
        ", year=" + year +
        '}';
  }
}
```

Now we have a collection of cars:

```java
List<Car> cars = new ArrayList<>();
cars.add(new Car("green", 1000.0, 2018));
cars.add(new Car("blue", 2000.0, 2020));
cars.add(new Car("maroon", 3000.0, 2019));
cars.add(new Car("yellow", 2500.0, 2020));
cars.add(new Car("purple", 999.0, 2021));
```

If we want to view all cars with weight bigger than 1000 pounds, we can define a implementation class:

```java
import java.util.function.Predicate;

public class CarWeightPredicate implements Predicate<Car> {
  @Override
  public boolean test(Car car) {
    return car.getWeight() > 1000.0;
  }
}
```

This predicate can then be supplied into a filter function:

```java
Predicate<Car> weightPredicate = new CarWeightPredicate();
cars.stream().filter(weightPredicate)
    .collect(Collectors.toList())
    .forEach(System.out::println);
System.out.println();
```

```
Car{color='blue', weight=2000.0, year=2020}
Car{color='maroon', weight=3000.0, year=2019}
Car{color='yellow', weight=2500.0, year=2020}
```

A less verbose way to define a predicate is with the help of lambda function in Java 8:

```java
Predicate<Car> yearPredicate = car -> car.getYear() > 2019;
cars.stream().filter(yearPredicate)
    .collect(Collectors.toList())
    .forEach(System.out::println);
System.out.println();
```

```
Car{color='blue', weight=2000.0, year=2020}
Car{color='yellow', weight=2500.0, year=2020}
Car{color='purple', weight=999.0, year=2021}
```

Composite predicates can be defined by combining multiple predicates together:

```java
Predicate<Car> andPredicate = weightPredicate.and(yearPredicate);
cars.stream().filter(andPredicate)
    .collect(Collectors.toList())
    .forEach(System.out::println);
System.out.println();

Predicate<Car> orPredicate = weightPredicate.or(yearPredicate);
cars.stream().filter(orPredicate)
    .collect(Collectors.toList())
    .forEach(System.out::println);
System.out.println();

Predicate<Car> negatePredicate = weightPredicate.negate();
cars.stream().filter(negatePredicate)
    .collect(Collectors.toList())
    .forEach(System.out::println);
```

```
Car{color='blue', weight=2000.0, year=2020}
Car{color='yellow', weight=2500.0, year=2020}
Car{color='purple', weight=999.0, year=2021}

Car{color='blue', weight=2000.0, year=2020}
Car{color='yellow', weight=2500.0, year=2020}

Car{color='blue', weight=2000.0, year=2020}
Car{color='maroon', weight=3000.0, year=2019}
Car{color='yellow', weight=2500.0, year=2020}
Car{color='purple', weight=999.0, year=2021}

Car{color='green', weight=1000.0, year=2018}
Car{color='purple', weight=999.0, year=2021}
```
