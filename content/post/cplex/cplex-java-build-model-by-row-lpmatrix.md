---
title: Build Optimization Models by Row with IloLPMatrix using CPLEX Java API
draft: true
date: 2019-10-21
tags: ["cplex", "Java"]
---

In a previous [post]({{< ref "./cplex-java-build-model-by-row.md" >}}), we examined how to build optimization models row-wise, in which decision variables and constraints are directly managed by an IloCplex object.
It turns out that there is another way to build/represent constraints in CPLEX and, in this post, we'll look at how to utilize *IloLPMatrix* to create and manage modeling components.

## An illustrative example

As usual, let's use a small example to do the case study.
It's a contrived model with no practical meanings, and it may not be feasible.

$$
\begin{align}
\text{max.} \quad & 2 x_1 + x_2 + 3x_3 + 2x_4 + 5x_5 \newline
\text{s.t.} \quad & x_1 + x_3 \geq 2 \newline
\quad & x_2 + x_4 + x_5 \geq 3 \newline
\quad & x_1 + x_2 + x_4 \geq 1 \newline
\quad & 0 \leq x_i \leq 100,\ \forall i \in \{ 1, \cdots, 5 \}
\end{align}
$$

## Key steps in using IloLPMatrix

Before we getting into the code details, it's worth mentioning that *IloLPMatirx* is a special interface defined in the Concert package to provide a holistic matrix-like representation of constraints and variables.
Note that in our aforementioned post of row-wise modeling, individual constraint is created and subsequently added to IloCplex directly.
IloCplex has all accesses to all modeling components within its space.
Whereas in the case of ILoLPMatrix, they are added to an object of classes implementing the IloLPMatrix interface, which acts like a composite object taking over some of the management responsibilities from IloCplex.
In other words, management of constraints is delegated to IloLPMatrix, and this devolvement prevents the constraints and variables from being accessed directly by IloCplex.

As shown in the code below, using IloLPMatrix requires creation of an IloLPMatrix object using either the *LPMatrix()* or the *addLPMatrix()* method. 

```java
import ilog.concert.IloColumnArray;
import ilog.concert.IloException;
import ilog.concert.IloLPMatrix;
import ilog.concert.IloNumVar;
import ilog.cplex.IloCplex;
import ilog.cplex.IloCplex.Param.Preprocessing;

public class BuildModelByRow {

  public static void main(String[] args) {
    try {
      // create IloCplex object
      IloCplex cplex = new IloCplex();

      // add a IloLPMatrix object
      IloLPMatrix matrix = cplex.addLPMatrix();

      // create new variables
      int numVars = 5;
      IloColumnArray colArray = cplex.columnArray(matrix, numVars);
      IloNumVar[] vars = cplex.numVarArray(colArray, 0, 100);

      // add constraints
      double[] lb = {2.0, 3.0, 1.0};
      double[] ub = {Double.MAX_VALUE, Double.MAX_VALUE, Double.MAX_VALUE};
      int[][] indices = {{0, 2}, {1, 3, 4}, {0, 1, 3}};
      double[][] values = {{1, 1}, {1, 1, 1}, {1, 1, 1}};
      matrix.addRows(lb, ub, indices, values);

      // define objective function
      double[] objCoefs = {2, 1, 3, 2, 5};
      cplex.addMinimize(cplex.scalProd(objCoefs, vars));

      // set parameter
      cplex.setParam(Preprocessing.Presolve, false);

      // solve
      if (cplex.solve()) {
        System.out.println("Obj = " + cplex.getObjValue());
      }

      cplex.end();
    } catch (IloException e) {
      e.printStackTrace();
    }
  }
}
```


## What's the difference?
