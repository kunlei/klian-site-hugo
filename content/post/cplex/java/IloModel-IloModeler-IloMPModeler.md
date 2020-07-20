---
title: IloModel, IloModeler and IloMPModeler
draft: false
date: 2019-10-11
tags: ["cplex", "Java"]
---

I have been using CPLEX for a while but haven't paid much attention to how its various components are woven together to enable users building and solving optimization models.
It is time to dive deeper into CPLEX's design principle and see how it works internally.
This is an attempt to improve my knowledge about CPLEX, and the random notes below are just my thinking aloud.

### IloAddable

**IloAddable** is the base interface implemented by most classes in the package.
It marks one CPLEX class as a optimization component and provides functions to get/set its name.
This interface might look like below internally:

```java
public interface IloAddable {
    String getName();
    void setName(String name);
}
```

One implication of this design is that we can give each model component a name and method to retrieve it.

### IloModel

**IloModel** is another base interface specifying functions that optimization model classes must implement.
It is interesting to note that this interface itself extends IloAddable, which allows us assigning names to models.
Naturally, a model can accomplish little without components like variables, constraints and objective function(s) in it.
That's probably why this base modeling interface encompasses methods to add and remove instances of component classes.
Since the model has to allow either constraints or objectives being added to it, parameters of these add/remove methods are set as IloAddable objects.
Technically, we can add a modeling object into another modeling object.

```java
public interface IloModel extends IloAddable {
    IloAddable add(IloAddable object);
    IloAddable remove(IloAddable object);
    java.util.Iterator iterator();
}
```

It's worth noting that modeling components can be added individually or, more efficiently, in arrays.

### IloModeler

**IloModeler** builds on top of *IloModel* to add methods for creating variables, constraints and objectives, and many ancillary methods.
One may choose to include the various components into a model immediately after their constructions or do that in a later stage.
The resulting model represents a generic model that can be solved by either CPLEX or CP Optimizer.

```java
public interface IloModeler extends IloModel {
    // methods to create constraints and add to model
    Constraint addEq(...);
    Constraint addGe(...);
    Constraint addLe(...);
    Constraint addRange(...);

    // methods to create constraints without adding them to model
    Constraint eq(...);
    Constraint ge(...);
    Constraint le(...);
    Constraint range(...);

    // methods to create objectives and add to model
    Objective addMaximize(...);
    Objective addMinimize(...);
    Objective addObjective(...);

    // methods to create objectives without adding them to model
    Objective maximize(...);
    Objective minimize(...);
    Objective objective(...);

    // methods to create variables and add to model
    Variable boolVar(...);
    Variable intVar(...);
    Variable numVar(...);

    // ancillary methods
    IloAdd and(...);
    IloOr or(...);
    Expression not(...);

    Expression constant(...);
    Expression diff(...);
    Expression intExpr(...);
    Expression linearIntExpr(...);
    Expression numExpr(...);
    Expression linearNumExpr(...);
    Expression max(...);
    Expression min(...);
    Expression negative(...);
    Expression prod(...);
    Expression scalProd(...);
    Expression square(...);
    Expression sum(...);
}
```

### IloMPModeler

*IloMPModeler* extends on *IloModeler* and adds support for column-wise modeling and *IloLPMatrix*.
Some notable functions of this interface include:

```java
IloLPMatrix addLPMatrix();

IloColumn column(...);
IloColumnArray columnArray(...);

IloNumVar numVar(IloColumn col);
IloIntVar intVar(IloColumn col);
IloIntVar boolVar(IloColumn col);

IloConversion conversion(...);
```

### Notes

Essentially, *IloModel*, *IloModeler* and *IloMPModeler* defines three levels of abstraction to create optimization models.

+ *IloModel* just says that an optimization model should support addition and removal of modeling components.
+ *IloModeler* defines the modeling interface for creating variables, constraints and objectives, which can be used both in mathematical programming and constraint programming.
+ *IloMPModeler* focuses solely on mathematical programming by supporting certain specific functions like column-wise modeling and matrix-form modeling.

![test image](/img/logo.png)


