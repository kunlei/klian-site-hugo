---
title: Spring - Resolving Multiple Candidate Dependencies
date: 2020-08-05
draft: false
tags: ["java", "spring"]
---

In this post, we'll look at possible ways of resolving the issue with multiple candidate dependencies.
To illustrate the problem, let's say we have a `Room` class that has a dependency on an object implementing the `CleanStrategy` interface:

```java
@Component
public class Room {
  private final CleanStrategy cleanStrategy;

  @Autowired
  public Room(CleanStrategy cleanStrategy) {
    this.cleanStrategy = cleanStrategy;
  }

  public void clean() {
    this.cleanStrategy.clean();
  }
}
```

Now we have two ways to clean the room, manually and automatically, and the corresponding class implementations are as follows:

```java
@Component
public class ManualClean implements CleanStrategy {
  @Override
  public void clean() {
    System.out.println("clean manually!");
  }
}
```

```java
@Component
public class AutomaticClean implements CleanStrategy {
  @Override
  public void clean() {
    System.out.println("clean automatically!");
  }
}
```

Because both classes have the `@Component` annotation, Spring will have trouble determining which one it should use to inject into the `Room` class.
To address this, we have three possible ways.

### @Primary

It we want to use the `ManualClean` as the underlying cleaning strategy for the `Room` class, we can use the annotation `Primary` to let Spring know our intent.

```java
@Component
@Primary
public class ManualClean implements CleanStrategy {
  @Override
  public void clean() {
    System.out.println("clean manually!");
  }
}
```

### By naming convention

Instead of using the `@Primary` annotation, we can change the parameter name of the constructor.
Here, we specify that the input parameter is named as `automaticClean`, and Spring will recognize the name and choose the associated class.

```java
@Component
public class Room {
  private final CleanStrategy cleanStrategy;

  @Autowired
  public Room(CleanStrategy automaticClean) {
    this.cleanStrategy = automaticClean;
  }

  public void clean() {
    this.cleanStrategy.clean();
  }
}
```

Note that in case of autowiring on class attributes, their names should be changes instead of constructor parameters. 

### @Qualifier

The last option we have is to use the `@Qualifier` annotation.
To do this, we give a nickname for each candidate dependency class, like below:

```java
@Component
@Qualifier("manual")
public class ManualClean implements CleanStrategy {
  @Override
  public void clean() {
    System.out.println("clean manually!");
  }
}
```

```java
@Component
@Qualifier("automatic")
public class AutomaticClean implements CleanStrategy {
  @Override
  public void clean() {
    System.out.println("clean automatically!");
  }
}
```

Then, in the `Room` class constructor we just need to specify the cleaning strategy we want through its associated qualifier.

```java
@Component
public class Room {
  private final CleanStrategy cleanStrategy;

  @Autowired
  public Room(@Qualifier("manual") CleanStrategy cleanStrategy) {
    this.cleanStrategy = cleanStrategy;
  }

  public void clean() {
    this.cleanStrategy.clean();
  }
}

```