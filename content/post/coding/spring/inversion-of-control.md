---
title: Spring - Inversion of Control
date: 2020-08-02
draft: false
tags: ["java", "spring"]
---

In an earlier [post]({{< ref "dependency-injection.md" >}}), we examined the concept of dependency injection as well as the process of making two classes loosely coupled.
For the sake of completeness, the main function code is repeated here:

```java
public class Demo {
  public static void main(String[] args) {
    CleanStrategy cleanStrategy = new ManualClean();
    Room room = new Room(cleanStrategy);
    room.clean();
  }
}
```

Although we've made the `Room` and `ManualClean` classes loosely coupled, this code change still requires us manually supplying a `ManualClean` object upon the `Room` object construction.
In other words, we as developer still have to control the object construction process and it's natural to ask whether it can be further simplified.
It turns out that the answer is yes and it's actually where the Spring framework comes into our rescue.
In a nutshell, Spring takes over the control of object construction and enable us to focus on developing core algorithmic or business logics, which is called **inversion of control**.

In order to use the Spring framework, we have three things to do:

+ we have to tell Spring what classes we want it to manage for us
+ we have to tell Spring the dependencies between those classes
+ we have to tell Spring where to find those classes

We'll deep dive into these details in following sections.

### Create Beans

Spring calls the classes that it manages for us **beans**.
To tell Spring to take over the control of a class, we use the annotation `@Component`, which is illustrated using the below two classes.
This annotation effectively notifies Spring that we want to make them as beans managed by the framework.

```java
@Component
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

```java
@Component
public class ManualClean implements CleanStrategy {
  @Override
  public void clean() {
    System.out.println("clean manually!");
  }
}
```

### Specify Dependencies

In order for Spring to automatically inject dependencies for classes it manages for us, it has to know the relationships between classes in the first place.
In the case of our example, we use the annotation `@Autowired` to inform Spring that the `cleanStrategy` attribute is supposed to be wired with another bean.
This form of dependency injection is through constructors, and we can use the annotation on the attribute itself, although it's not recommended.

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

### Component Scan

We tell Spring the location of our classes through the annotation `@ComponentScan`:

```java
@ComponentScan(basePackages = "com.learn.spring.basic")
public class BasicConceptsApplication {

  public static void main(String[] args) {

    ApplicationContext applicationContext = SpringApplication.run(BasicConceptsApplication.class, args);
    Room room = applicationContext.getBean(Room.class);
    room.clean();
  }
}
```
