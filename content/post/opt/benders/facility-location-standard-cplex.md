---
title: Capacitated Facility Location Problem - MIP Model Solving using CPLEX
date: 2020-07-19
draft: false
tags: ["facility location", "benders decomposition"]
---

I'll demonstrate in this post how to use the CPLEX Java API to solve an capacitated facility location problem instance.
A testing instance *cap41* is taken from ORLib and the data input function will be illustrated first.
With the class holding the instance data ready, the MIP modeling class will be implemented and computational results will be shown in the end.

## Testing instance

The instance can be obtained directly from the following url:

```text
http://people.brunel.ac.uk/~mastjjb/jeb/orlib/files/cap41.txt
```

I'll use the *Instance* class to hold all the data, which encompasses some key information of this problem:

+ facility-related data
  + *numFacilities*: number of facilities available
  + *fixedCosts*: fixed cost of each facility
  + *capacities*: capacity of each facility
+ customer-related data
  + *numCustomers*: number of customers
  + *demands*: demand of each customer
+ assignment-related data
  + *flowCosts*: cost of serving a customer from a specific facility

The code below gives the *Instance* class definition, the key part of which is the *readData* function that takes an filename as input and scans all the data within the file.

```java
package com.learn.opt.benders;

import java.io.File;
import java.util.Scanner;
import java.io.FileNotFoundException;

/**
 * define a class that represents a facility location problem instance.
 */
public final class Instance {
  /**
   * total number of facilities
   */
  private int numFacilities;

  /**
   * total number of customers
   */
  private int numCustomers;

  /**
   * facility capacities
   * size: 1 * numFacilities
   */
  private int[] capacities;

  /**
   * facility fixed costs
   * size: 1 * numFacilities
   */
  private double[] fixedCosts;

  /**
   * customer demands
   * size: 1 * numCustomers
   */
  private int[] demands;

  /**
   * allocation cost from facilities to customers
   * size: numCustomers * numFacilities
   */
  private double[][] flowCosts;

  public Instance() {
    numCustomers = 0;
    numFacilities = 0;
    capacities = null;
    fixedCosts = null;
    demands = null;
    flowCosts = null;
  }

  /**
   * read data from file
   * @param filename name of the input file
   */
  public void readData(String filename) {
    try {
      File file = new File(filename);
      Scanner sc = new Scanner(file);

      numFacilities = sc.nextInt();
      numCustomers = sc.nextInt();

      // create data objects
      capacities = new int[numFacilities];
      fixedCosts = new double[numFacilities];
      demands = new int[numCustomers];
      flowCosts = new double[numCustomers][numFacilities];
      for (int i = 0; i < numCustomers; ++i) {
        flowCosts[i] = new double[numFacilities];
      }

      // read in facility capacities and fixed cost
      for (int f = 0; f < numFacilities; ++f) {
        capacities[f] = sc.nextInt();
        fixedCosts[f] = sc.nextDouble();
      }

      // read in customer demand and flow cost
      for (int c = 0; c < numCustomers; ++c) {
        demands[c] = sc.nextInt();
        for (int f = 0; f < numFacilities; ++f) {
          flowCosts[c][f] = sc.nextDouble() / demands[c];
        }
      }

      sc.close();
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    }
  }

  public int getNumFacilities() {
    return numFacilities;
  }

  public int getNumCustomers() {
    return numCustomers;
  }

  public int getCapacity(int fIdx) {
    return capacities[fIdx];
  }

  public double getFixedCost(int fIdx) {
    return fixedCosts[fIdx];
  }

  public int getDemand(int cIdx) {
    return demands[cIdx];
  }

  public double getFlowCost(int fIdx, int cIdx) {
    return flowCosts[cIdx][fIdx];
  }

  @Override
  public String toString() {
    StringBuilder builder = new StringBuilder("Instance{");
    builder.append("numFacilities: ").append(numFacilities)
        .append("\nnumCustomers: ").append(numCustomers);
    for (int f = 0; f < numFacilities; ++f) {
      builder.append("\nfacility ").append(f).append(": capacity: ")
          .append(capacities[f])
          .append("\tfixedCost: ")
          .append(fixedCosts[f]);
    }
    for (int c = 0; c < numCustomers; ++c) {
      builder.append("\ncustomer ").append(c).append(": demand: ").append(demands[c]);
    }
    for (int c = 0; c < numCustomers; ++c) {
      builder.append("\n");
      for (int f = 0; f < numFacilities; ++f) {
        builder.append(flowCosts[c][f]).append("\t");
      }
    }
    return builder.toString();
  }
}
```

## Model building and solving

The modeling code is pretty self-explanatory.
The *cplex* object is first created within the constructor to help us build key modeling components:

+ decision variables
  + *open*: a binary variable equals 1 if a facility is open, 0 otherwise
  + *flow*: a continuous variable that represents the demand of a customer that's satisfied by a facility
+ objective
  + *obj*: this defines the objective function that consists of the fixed cost and assignment cost
+ constraints
  + *consDemand*: the constraint requires that all customer demands must be fulfilled
  + *constCapacity*: this constraint makes sure that facility capacity is not violated

The *solve()* function calls CPLEX to find and output the optimal solution.

```java
package com.learn.opt.benders;

import ilog.concert.IloException;
import ilog.concert.IloLinearNumExpr;
import ilog.concert.IloNumVar;
import ilog.concert.IloObjective;
import ilog.concert.IloRange;
import ilog.cplex.IloCplex;

public class MIPModel {
  private final Instance instance;

  private IloCplex cplex;

  private IloNumVar[] open;
  private IloNumVar[][] flow;

  private IloObjective obj;
  private IloRange[] consDemand;
  private IloRange[] consCapacity;

  public MIPModel(Instance instance) {
    this.instance = instance;

    int numFacilities = instance.getNumFacilities();
    int numCustomers = instance.getNumCustomers();

    try {
      cplex = new IloCplex();

      // create decision variables that indicate whether a facility is open or not
      open = new IloNumVar[numFacilities];
      for (int f = 0; f < numFacilities; ++f) {
        open[f] = cplex.boolVar("open_" + f);
      }

      // create decision variables that represent the demand of customer c that are
      // satisfied by facility f
      flow = new IloNumVar[numFacilities][numCustomers];
      for (int f = 0; f < numFacilities; ++f) {
        for (int c = 0; c < numCustomers; ++c) {
          flow[f][c] = cplex.numVar(0, Double.MAX_VALUE, "flow_" + f + "," + c);
        }
      }

      // create objective function
      IloLinearNumExpr expr = cplex.linearNumExpr();
      for (int f = 0; f < numFacilities; ++f) {
        expr.addTerm(instance.getFixedCost(f), open[f]);
        for (int c = 0; c < numCustomers; ++c) {
          expr.addTerm(instance.getFlowCost(f, c), flow[f][c]);
        }
      }
      obj = cplex.addMinimize(expr, "obj");

      // create constraints
      consDemand = new IloRange[numCustomers];
      for (int c = 0; c < numCustomers; ++c) {
        expr.clear();
        for (int f = 0; f < numFacilities; ++f) {
          expr.addTerm(1.0, flow[f][c]);
        }
        consDemand[c] = cplex.addGe(expr, instance.getDemand(c), "demand_" + c);
      }

      consCapacity = new IloRange[numFacilities];
      for (int f = 0; f < numFacilities; ++f) {
        expr.clear();
        for (int c = 0; c < numCustomers; ++c) {
          expr.addTerm(1.0, flow[f][c]);
        }
        expr.addTerm(-instance.getCapacity(f), open[f]);
        consCapacity[f] = cplex.addLe(expr, 0, "capacity_" + f);
      }

    } catch (IloException e) {
      e.printStackTrace();
    }
  }

  public void solve() {
    try {
      if (cplex.solve()) {
        System.out.println("obj = " + cplex.getObjValue());

        int numFacilities = instance.getNumFacilities();
        int numCustomers = instance.getNumCustomers();
        double[] optOpen = cplex.getValues(open);
        double[][] optFlow = new double[numFacilities][];
        for (int f = 0; f < numFacilities; ++f) {
          optFlow[f] = cplex.getValues(flow[f]);
        }

        for (int f = 0; f < numFacilities; ++f) {
          System.out.print("facility " + f + ": ");
          if (optOpen[f] > 0.99) {
            System.out.println("open, ");
            for (int c = 0; c < numCustomers; ++c) {
              if (optFlow[f][c] > 0.01) {
                System.out.println("\tto customer " + c + " volume: " + optFlow[f][c]);
              }
            }
          } else {
            System.out.println("close, ");
          }
        }
      }
    } catch (IloException e) {
      e.printStackTrace();
    }
  }

}

```

A *Console* class is defined below to facilitate our experiment:

```java
package com.learn.opt.benders;

public class Console {
  public static void main(String[] args) {
    // create a problem instance
    Instance inst = new Instance();
    inst.readData("/Users/klian/dev/facility-location-benders/instance/cap41.txt");
    System.out.println(inst.toString());

    MIPModel mipModel = new MIPModel(inst);
    mipModel.solve();
  }
}
```

The terminal output is as follows:

```
Tried aggregator 1 time.
MIP Presolve eliminated 0 rows and 1 columns.
Reduced MIP has 66 rows, 815 columns, and 1615 nonzeros.
Reduced MIP has 15 binaries, 0 generals, 0 SOSs, and 0 indicators.
Presolve time = 0.00 sec. (0.64 ticks)
Probing time = 0.00 sec. (0.06 ticks)
Tried aggregator 1 time.
Reduced MIP has 66 rows, 815 columns, and 1615 nonzeros.
Reduced MIP has 15 binaries, 0 generals, 0 SOSs, and 0 indicators.
Presolve time = 0.00 sec. (0.75 ticks)
Probing time = 0.00 sec. (0.06 ticks)
MIP emphasis: balance optimality and feasibility.
MIP search method: dynamic search.
Parallel mode: deterministic, using up to 8 threads.
Root relaxation solution time = 0.00 sec. (0.36 ticks)

        Nodes                                         Cuts/
   Node  Left     Objective  IInf  Best Integer    Best Bound    ItCnt     Gap

      0     0  1018151.6250     7                1018151.6250       81         
*     0+    0                      1050749.6250  1018151.6250             3.10%
      0     0  1038238.6359     3  1050749.6250      Cuts: 37       97    1.19%
*     0+    0                      1043000.4500  1038238.6359             0.46%
      0     0  1040169.2296     1  1043000.4500      Cuts: 26      107    0.27%
*     0     0      integral     0  1040444.3750       Cuts: 8      111    0.00%
      0     0        cutoff        1040444.3750                    111    0.00%
Elapsed time = 0.04 sec. (8.69 ticks, tree = 0.01 MB, solutions = 3)

Implied bound cuts applied:  37
Flow cuts applied:  3
Mixed integer rounding cuts applied:  11
Lift and project cuts applied:  1
Gomory fractional cuts applied:  5

Root node processing (before b&c):
  Real time             =    0.04 sec. (8.74 ticks)
Parallel b&c, 8 threads:
  Real time             =    0.00 sec. (0.00 ticks)
  Sync time (average)   =    0.00 sec.
  Wait time (average)   =    0.00 sec.
                          ------------
Total (root+branch&cut) =    0.04 sec. (8.74 ticks)
obj = 1040444.375
facility 0: open, 
	to customer 2 volume: 672.0
	to customer 3 volume: 1337.0
	to customer 5 volume: 559.0
	to customer 13 volume: 143.0
	to customer 23 volume: 304.0
	to customer 25 volume: 337.0
	to customer 29 volume: 495.0
	to customer 39 volume: 328.0
	to customer 48 volume: 728.0
facility 1: open, 
	to customer 6 volume: 2370.0
	to customer 33 volume: 2630.0
facility 2: open, 
	to customer 33 volume: 5000.000000000001
facility 3: open, 
	to customer 10 volume: 5000.000000000001
facility 4: open, 
	to customer 11 volume: 904.0
	to customer 33 volume: 3360.0
	to customer 48 volume: 736.0
facility 5: open, 
	to customer 12 volume: 1466.0
	to customer 36 volume: 3534.0
facility 6: open, 
	to customer 14 volume: 615.0
	to customer 19 volume: 195.0
	to customer 21 volume: 807.0
	to customer 43 volume: 500.0
	to customer 47 volume: 49.00000000000001
facility 7: open, 
	to customer 0 volume: 146.0
	to customer 4 volume: 31.0
	to customer 8 volume: 33.0
	to customer 9 volume: 32.0
	to customer 15 volume: 564.0
	to customer 38 volume: 705.0
	to customer 42 volume: 275.0
	to customer 44 volume: 1609.0
	to customer 45 volume: 733.0
	to customer 46 volume: 222.0
facility 8: open, 
	to customer 7 volume: 1089.000000000001
	to customer 17 volume: 3016.0
	to customer 33 volume: 894.9999999999991
facility 9: close, 
facility 10: open, 
	to customer 10 volume: 494.9999999999991
	to customer 22 volume: 551.0
	to customer 27 volume: 577.0
	to customer 33 volume: 1027.0
	to customer 36 volume: 137.0
	to customer 37 volume: 2213.0
facility 11: open, 
	to customer 1 volume: 87.0
	to customer 16 volume: 226.0
	to customer 18 volume: 253.0
	to customer 20 volume: 38.0
	to customer 24 volume: 814.0
	to customer 34 volume: 325.0
	to customer 35 volume: 366.0
	to customer 40 volume: 1552.0
	to customer 41 volume: 1117.0
	to customer 49 volume: 222.0
facility 12: open, 
	to customer 26 volume: 4368.0
	to customer 44 volume: 632.0
facility 13: open, 
	to customer 28 volume: 482.0
	to customer 30 volume: 231.0
	to customer 31 volume: 322.0
	to customer 32 volume: 685.0000000000001
	to customer 40 volume: 129.0
facility 14: close, 
facility 15: close, 

Process finished with exit code 0
```