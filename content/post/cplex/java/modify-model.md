---
title: Modifying a Model using CPLEX Java API
date: 2020-07-22
draft: true
tags: ["cplex", "java"]
---

CPLEX supports various ways of modifying objects in a model;
the process of building a model exemplifies this functionality -- creating a model is essentially modifying an existing model by adding more elements to it.
It's sometimes desirable to remove components from a model, as in the case of getting a relaxed model by dropping constraints from it.
In other cases, we might just want to modifying a model without completing removing objects, for example, we might want to change the type of a variable from continuous to discrete.
In the following sections, I'll explore the ways we can interact with a model to achieve the possible scenarios described above.

## Decision variables

### Add variables

In row-wise modeling, variables are added to a model implicitly when objective function or constraints are added to the model.

In column-wise modeling, variables have to be added explicitly to a model after they're created -- CPLEX won't pickup a newly created variable automatically.

### Remove variables

### Modify variables

## Objective

### Add objective

### Remove objective

### Modify objective

## Constraints

### Add constraints

### Remove constraints

### Modify constraints
