---
title: IloAddable and IloModel
date: 2020-07-12
draft: false
tags: ["cplex", "java"]
---

As a frequent user of CPLEX through its Java API, I have been wondering for a long time how the various interfaces/classes are designed in a way that supports flexible model building and problem solving.
This post, and future ones, aims to help myself comprehend the inner designing logic.

A mathematical model consists of three components: decision variables, one or more optimization objectives and constraints.
The first step towards solving a model is to build a model that encompasses the three components.
If we have a Java class that represents a model, then one of its key functions would be add modeling components.
The Java *IloAddable* defines an interface that specifies the most fundamental function that a modeling component should have;
whereas *IloModel* defines an interface that defines the behaviors that a model is supposed to have.

## Essential functions of a modeling component

When we build a mathematical model, we would expect a way to uniquely identify each of its components, for which purpose we can assign a name to each modeling component added to the model.
In other words, a modeling component should at least support the name setting and retrieval functions:

```java
void setName(String name);
String getName();
```

Put it in another way, objects of classes implementing this interface have their corresponding names and can be included in a CPLEX model.

## Essential functions of a model

An empty model is useless we have ways to add or remove components to/from it.
Sometimes we might even want to add/remove multiple components at the same time.
It is therefore natural for a model interface to have the following behaviors:

```java
IloAddable add(IloAddable obj);
IloAddable[] add(IloAddable[] objs);
IloAddable remove(IloAddable obj);
IloAddable[] remove(IloAddable[] objs);
```

It's interesting to note that the *IloModel* itself implements the *IloAddable* interface, thereby enabling us to assign a name to a model.
I am not sure at this moment whether adding a model to an existing model makes sense or does anything useful, but theoretically, the definitions of the two interfaces allow this.
